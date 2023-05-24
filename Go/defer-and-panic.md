# How to Rollback in go / panicになったときもdeferされた処理は行われるのか

goでsqlのトランザクションを扱うときに、どのように扱えば適切なのか不安になったので調べてみたところ、以下のように記述されているブログを見つけた：

```go
package main

import (
    "database/sql"
    _ "github.com/go-sql-driver/mysql"
)

func main() {
	db, _ := sql.Open("mysql", "user:password@tcp(host:port)/dbname")
	tx, _ := db.Begin()
	defer func() {
		if recover() != nil {
			tx.Rollback()
		}
	}()

	// *中略*

	// 何かの処理で問題が起こったとき
	if err != nil {
		tx.Rollback() // ロールバック
	} else {
		tx.Commit() // コミット
	}
}

```

このように書いてもいいが、Commit後にRollbackしても特に影響はなく、パフォーマンスにもほとんど影響がないとのことであった[1]。
なおかつpanicがどこかで起こった場合も、deferされた処理は実行されるため、panic時と通常のエラー時を区別しない場合は以下のように書けば十分：
```go
package main

import (
    "database/sql"
    _ "github.com/go-sql-driver/mysql"
)

func main() {
	db, _ := sql.Open("mysql", "user:password@tcp(host:port)/dbname")
	tx, _ := db.Begin()
	defer tx.Rollback()

	// *中略*

	if err == nil {
		tx.Commit()  // Commit()がerrを返しうるため、本当はそこのケアもする必要あり
    }
}

```

※panicが起きてもdeferされた処理が実行されることは以下のコードなどで確認できる（そもそもpanic時にdeferされた処理が実行されないのであれば、`recover()`をdeferする意味がなくなってしまう）

```go
package main

import "fmt"

func helloworld() {
	defer fmt.Println("World")
	fmt.Println("Hello")
	cause()
}

func cause() {
	panic("Panic caused!")
}
func main() {
	helloworld()
}
```

上記のプログラムを実行すると、
```bash
$ go run main.go
Hello
World
panic: Panic caused!

goroutine 1 [running]:
main.cause(...)
        /path/to/file/main.go:13
main.helloworld()
        /path/to/file/main.go:8 +0xb9
main.main()
        /path/to/file/main.go:16 +0x17
exit status 2
```
となり、deferされた処理も実行されていることがわかる。

## 参考文献
1. [Why defer a Rollback? - stack overflow](https://stackoverflow.com/questions/46421602/why-defer-a-rollback)
2. [Golang の defer 文と panic/recover 機構について](https://blog.amedama.jp/entry/2015/10/11/123535)