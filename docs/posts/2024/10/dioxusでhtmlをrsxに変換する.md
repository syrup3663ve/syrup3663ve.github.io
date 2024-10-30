# RustのフレームワークdioxusでHTMLをRSXに変換する方法について

内容は[dioxus-cliのtranslate](https://dioxuslabs.com/learn/0.5/CLI/translate)に記載のある通り。

## 検証時のバージョン

- [dioxus-cli：0.6.0-alpha.3](https://crates.io/crates/dioxus-cli/0.6.0-alpha.3)

## インストール

```
cargo install dioxus-cli
```

## 使い方

1. HTMLファイルからRSXへ変換する

   `-f`または`--file`オプションを使用する。

   ```
   dx translate --file hoge.html
   ```

   ソース
   ```html:hoge.html
   <div>hello world</div>
   ```

   出力
   ```
   div { "hello world" }
   ```

2. HTMLの文字列からRSXへ変換する

   `-r`または`--raw`オプションを使用する。

   ```
   dx translate --raw "<div>hello world</div>"
   ```

   文字列内に`"`が含まれる場合は`'`で囲うと正しく変換される。

   `'`で囲う場合（OK）
   ```
   dx translate --raw '<a class="link" href="/">Go to Somewhere</a>'
   ```
   ```
   a { href: "/", class: "link", "Go to Somewhere" }
   ```

   `"`で囲う場合（NG）
   ```
   dx translate --raw "<a class="link" href="/">Go to Somewhere</a>"
   ```
   ```
   a { href: "", class: "link" } "Go to Somewhere"
   ```

3. ファイルに出力する

   `-o`または`--output`オプションを使用する。

   未指定の場合は標準出力に出力される。

   ```
   dx translate --raw "<div>hello world</div>" --output hoge.rs
   ```

4. 変換したRSXでコンポーネントを作成する

   `-c`または`--component`オプションを使用する。

   ```
   dx translate --raw "<div>hello world</div>" --component
   ```

   ```
   fn component() -> Element {
       rsx! {    
           div { "hello world" }
       })
   }
   ```

## おまけ

helpの出力

`dx translate -h`

```shell
Translate a source file into Dioxus code

Usage: dx translate [OPTIONS]

Options:
  -c, --component        Activate debug mode
  -f, --file <FILE>      Input file
  -r, --raw <RAW>        Input file
  -o, --output <OUTPUT>  Output file, stdout if not present
      --bin <BIN>        Specify a binary target
  -h, --help             Print help

```

## 参考・リンク

- [dioxus](https://dioxuslabs.com/)
- [dioxus-cli](https://dioxuslabs.com/learn/0.5/CLI)
- [dioxus-cli-translate](https://dioxuslabs.com/learn/0.5/CLI/translate)