# 簡易的なSet型
Pythonなどには重複しない要素のコレクションを表すset型が存在するが、Goにはそのような型が存在しない。
複数の文字列から重複を排除したい場面は少なくないので、そのようなときの対応法についてメモ。


```Go
// このスライスから重複を排除したい
list := []string{
    "key1",
    "key2",
    "key3",
    "key1",
    "key4",
}

set := make(map[string]struct{})
for _, v := range list {
    set[v] = struct{}{}
}

fmt.Printf("%#v\n", set)
// map[string]struct {}{
//   "key1":struct {}{},
//   "key2":struct {}{},
//   "key3":struct {}{}
//   "key4":struct {}{}
// }

// keyだけ取り出す
for k = range set {
    fmt.Println(k)
}
// "key1"
// "key2"
// "key3"
// "key4"
```

※struct{}はメモリを圧迫しない。

## 参考
- [Go言語での集合(Set)の扱い方とテスト](https://qiita.com/kotaroooo0/items/fe63ecc68723ab6ee81f)