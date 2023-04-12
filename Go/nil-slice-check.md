# スライスの空チェックについて

他の人のコードをレビューしているときに、あるスライスがnilであるかをチェックした後にlenが0かどうかチェックしているのを見つけた。

```Go
func someFunc(input []*int) error {
    if input == nil || len(input) == 0 {
        HandleNil()
    }
}
```

上のコードは下のようにすれば必要十分。

```Go
func someFunc(input []*int) error {
    if len(input) == 0 {
        HandleNil()
    }
}
```


---

今回のケースでは要素数の判定もしていたため問題はなかったが、`input == nil`という判定方法はあまり良くない。

というのも、sliceが空であるがnilにならない場合というのが存在するためである。

```Go
package main

import "fmt"

type intPtrs []*int

func main() {
	emptySlice := intPtrs{}
	var nilSlice []*int
	fmt.Println(emptySlice == nil)  // *false*
	fmt.Println(nilSlice == nil)    // true
	fmt.Println(len(emptySlice))    // 0
	fmt.Println(len(nilSlice))      // 0
}

```

## 参考
- [[Go]なぜsliceの空チェックで「nil」ではなく「長さ」でチェックするのか](https://qiita.com/Yashy/items/1d4f277998866b186e19)