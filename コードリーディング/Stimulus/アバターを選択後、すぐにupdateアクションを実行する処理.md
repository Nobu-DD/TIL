# コード内容
`mypages/show.html.erb`アバターを選択するボタンの処理を記述
```ruby:mypages/show.html.erb
<%= form_with url: mypage_path, method: :patch, data: { controller: "avatar-upload" } do |f| %>
<label class="absolute -bottom-1 -right-1 bg-[#A0D8EF] hover:bg-[#7CC7E8] text-white p-2 rounded-full cursor-pointer shadow-lg">
  <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="w-4 h-4">
    <path d="M14.5 4h-5L7 7H4a2 2 0 0 0-2 2v9a2 2 0 0 0 2 2h16a2 2 0 0 0 2-2V9a2 2 0 0 0-2-2h-3l-2.5-3z" />
    <circle cx="12" cy="13" r="3" />
  </svg>
  <%= f.file_field :avatar, class: "hidden", data: { action: "change->avatar-upload#submitForm" } %>
</label>
<% end %>
```
`avatar_upload_controller.js`アバターを選択したら、即座にmypageのupdateアクションに遷移する処理を記述
```javascript:avatar_upload_controller.js
export default class extends Controller {
  submitForm(event) {
    this.element.requestSubmit();
  }
}
```
# 解説
1. form_withにavatar_upload_controller.jsファイルを読み込ませる為に、dataを付与させる<br>
`data: { controller: "avatar-upload" }`
2. form_with内に定義した**file_field**(アバター画像ファイルを選択するフィールド)にchangeイベントを付与する。<br>
`<%= f.file_field :avatar, class: "hidden", data: { action: "change->avatar-upload#submitForm" } %>`<br>
changeイベントが発行されると、avatar-upload_controller.jsが読み込まれ、その中の**submitForm**イベントハンドラが呼び出されます。
- changeイベント
input,select,textareaタグなどで格納されている要素が**変更**された時に発行されるイベントです。<br>
今回はvalueがnilだった**file_field**に画像ファイルを選択し、valueの値が画像ファイルによって変更されたので、イベントが発行されます。

3. submitFormの処理を実行する<br>
```javascript:avatar_upload_controller.js
// submitFormメソッドを呼び出す(引数としてeventを指定しているが、今回は送信の処理を実行したいだけなので空白でも問題なし。)
// submitForm()でも可。基本的に慣習としてeventを引数に指定しておく
submitForm(event) {
// thisは実行しているオブジェクトを指定し、elementはオブジェクトのプロバティ(html要素)を示している。
// htmlのWebAPIが用意しているrequestSubmitメソッドを使用して、フォームの送信処理を実行する
    this.element.requestSubmit();
}
```
今回のコードの場合、
this = avatar_uploadコントローラーのインスタンス(対象がformタグ)
element = avatar_uploadインスタンス内に存在する要素(form)

avatar_uploadコントローラーを適用させたform要素にrequestSubmit()を実行することで、変更したファイルをmypageコントローラーのupdateアクションに送信している。
