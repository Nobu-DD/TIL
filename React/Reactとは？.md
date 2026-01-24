Facebookが開発をしたJavaScriptのUIライブラリ。各機能のかたまりを**コンポーネント**として扱い、いくつかのコンポーネントを組み合わせてWebサイトを構築することが出来る。<br>
部品の再利用性を高めることで、コードの品質や開発スピードの向上を高める事が出来る。
## Reactコードの書き方
純粋のJSファイルで記述する場合、以下のコードが適用される。(関数定義)
```javascript
function Hello() {
  return React.createElement(
    'div',
    {className: 'hello'},
    'こんにちは！'
  )
}
```
`React.createElement`はReact要素を作成するための関数です。Reactはこの関数で作成した要素を再利用部品として扱っていくのですが、少し冗長なコードとなり可読性に欠けてしまいます。<br>
しかし、この問題を`jsx`ファイルを適用させることで解決することが出来ます。
### jsx
先ほど書いたjsファイルのコードを、以下のコードのように記述できます。
```javascript
function Hello() {
  return
  <div className={'hello'}>
    こんにちは！
  </div>
}
```
jsベースではタグ名やクラス名、テキストなどをcreateElementのパラメータに追加で書かかなくてはいけませんでしたが、jsxではhtmlベースの記述を組み込むことで、コードの意図がわかりやすくなりました。<br>
このようにReactではjsとhtmlを上手く共存することで、直感的にわかりやすく、馴染みやすいコードにすることが出来る。
