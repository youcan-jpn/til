# Viteでのディレクトリエイリアス
TSの相対importを使う際、階層が深くなると`..`の個数が多くなってしまいがちだったので、webpackの設定を変えることで
```TypeScript
import { hoge } from '../../../src/hoge'
```
と書く代わりに、srcディレクトリに`#`というエイリアスを貼って
```TypeScript
import { hoge } from '#/hoge'
```
のように書いていた。

Viteでも同じようなことをするには、`vite.config.ts`で
```TypeScript
resolve: {
  alias: {
    "#": resolve(__dirname, "src"),
  },
},
```
のように設定すればよい。

また、`tsconfig.json`でも
```JSON
{
  "compilerOptions": {
    "paths": {
        "#/*": ["src/*"]
    }
  }
}
```
のように記載しておく。

## 実例
- [dab/frontend/vite.config.ts](https://github.com/youcan-jpn/dab/blob/main/frontend/vite.config.ts)
- [dab/frontend/tsconfig.json](https://github.com/youcan-jpn/dab/blob/main/frontend/tsconfig.json)

## 参考
- [Vite 共通オプション](https://ja.vitejs.dev/config/shared-options.html#resolve-alias)