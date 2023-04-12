# Goでファイル・ディレクトリの存在を確認する方法

```Go
import (
    "os"
    "path"
)

file_loc := "/path/to/file.txt"

// check if the file exists
if _, err := os.Stat(file_loc); !os.IsNotExists(err) {
    // file already exists
}

// check if the directory exists
if _, err = os.Stat(path.Dir(file_loc)); !os.IsNotExists(err) {
    // directory already exists
}

```