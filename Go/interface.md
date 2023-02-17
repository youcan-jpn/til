# interface

TypeScriptのinterfaceとは少し違ったのでメモ。

Goのinterfaceはそのインターフェースが持つべきメソッド（関数）を宣言したもの。
どのinterfaceを実装するものかを明示する必要はなく、あるinterfaceに必要なメソッドを全てもつ構造体はそのinterfaceを実装したものとみなされる（ダックタイピング）。

TypeScriptのinterfaceはクラスが持つべきメソッドとフィールドを定義したもので，さらにinterfaceの具体化されたクラスを実装するためには`implements`と明示的に実装する必要がある点で異なる．
TypeScriptのinterfaceのようにGoでもメソッドとフィールドを宣言したい場合は，構造体にインターフェースを埋め込む (embed) ことで似たようなことができる。

---
例1：Animalインターフェースとそれを実装したFish構造体
```Go
type Animal interface {
    Breath()
    Move()
    Eat()
}

type Fish struct {
    habitat         string  // 生息地
    isCarnivorous   bool    // 肉食かどうか
}

func (f Fish) Breath() {
    fmt.Println("エラ呼吸をする")
}

func (f Fish) Move() {
    fmt.Println("ひれを使って泳ぐ")
}

func (f Fish) Eat() {
    if f.isCarnivorous {
        fmt.Println("他の魚を襲って食べる")
        return
    }
    fmt.Println("水草などを食べる")
}
```
例1ではFish構造体がAnimalインターフェースを実装することを明示的に宣言していないが、Animalインターフェースに必要なメソッドが全て定義されているためAnimalインターフェースを実装していると判断される。

---
例2：複数interfaceを実装する構造体
```Go
type Animal interface {
    Breath()
    Move()
    Eat()
}

type Mammal interface {
    Feed()
}

type Human struct {
    Name    string
    Age     int
}

func (h Human) Breath() {
    fmt.Println("肺呼吸をする")
}

func (h Human) Move() {
    fmt.Println("歩いて移動する")
}

func (h Human) Eat() {
    fmt.Println("ご飯を食べる")
}

func (h Human) Feed() {
    fmt.Println("赤ちゃんにご飯をあげる")
}
```

例2ではHuman構造体がAnimalインターフェースとmammalインターフェースを実装している。

---

## 参考
- [Goのinterfaceが分からない人へ](https://qiita.com/rtok/items/46eadbf7b0b7a1b0eb08)
- [Type Implementing multiple interfaces in Go](https://golangbyexample.com/type-implementing-multiple-interfaces-go/)