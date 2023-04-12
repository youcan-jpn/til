# 新しいディレクトリにファイルを作成する

Goに限った話ではないけどメモ。
Goの`os.Create(filename)`でファイルを作成できるが，`filename=non-existing-dir/new.txt`のように存在しないディレクトリ以下を指定した場合はファイルを作成できずエラーとなる。

例1：失敗する例
```Go
	f2, err := os.Create("non-existing/sample.txt")
	if err != nil {
		fmt.Print(err)
	}
	defer f2.Close()
```

例2：エラーを回避する例（[参考：Goでファイル・ディレクトリの存在を確認する方法](./check_if_exists.md)）
```Go
	filename := "non-existing/sample.txt"
	if _, err := os.Stat(path.Dir(filename)); os.IsNotExist(err) {
		os.MkdirAll(path.Dir(filename), 0666)
	}
	f2, err := os.Create("non-existing/sample.txt")
	if err != nil {
		fmt.Print(err)
	}
	defer f2.Close()
```
