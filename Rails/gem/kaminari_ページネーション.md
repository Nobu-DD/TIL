# kaminariとは
ページネーション機能を簡単に実装することが出来るgem。
## 使用方法
`Gemfile`に`gem 'kaminari', '1.2.2'`を記入し、インストールするだけ。<br>
`bundle install`<br><br>
＊docker環境の方は<br>
`docker compose run web bundle install`
## 超基本なkaminariメソッド
### page(取得するページ数)
取得したモデルオブジェクトのページ数を設定できます。<br>
例えば、動物園の動物管理アプリの1ページの取り出した件数が20件だった場合...<br><br>
`Animal.page(1) = 1~20件`<br>
`Animal.page(2) = 21~40件`<br>
`Animal.page(3) = 41~60件`<br>
...<br>
このように、viewで表示させたい内容をページごとに変えられることが出来ます。
#### でもこれだとページ数ごとに記入しないといけないのでは？？
今回の場合だと、仮に200ページ分の膨大な量があるととても対応しきれません。なので、以下の記述方法が一般的(多分)かと思います。<br>
`Animal.page(params[:page])`<br>
この書き方であれば、ブラウザからページ数のパラメータが送られてくるので、ページごとに取り出してくるデータを切り替えることが出来ます。
#### でもでも、1ページ目はパラメータが送られてこないのでは？？？
確かに、今回の例だと1ページ目と2ページ目のパラメータは以下の通りになります<br>
1ページ目 `/animals`<br>
2ページ目 `/animals?page=2`<br>
これだと1ページ目を読み込んだ時に、pageメソッドに入る引数が空になってしまってエラーが起こるのでは？と僕も思いました。<br>
なので、以下のモデルオブジェクトを呼び出してみました。<br>
`Animal.page()`<br>
これはpageメソッドに何も記述しないことで、必然的にnilが入るようにしています(多分)<br>
そうすると、以下の結果が返ってきました。(pageに対応するメソッドを呼び出してみる)<br>
```ruby
Animal.page().current_page  => 1 (現在のページ数)
Animal.page().next_page     => 2 (現在から次のページ数)
Animal.page().first_page?   => true (最初のページかどうか？)
Animal.page().last_page?    => false (最後のページかどうか？)
```
今回の結果を元にすると、引数が指定されてない場合、自動的に1ページ目のデータを取り出してくるそうですね✨️<br>
### per(1ページごとに取り出す件数)
ページごとの件数を指定することが出来ます。<br>
先ほど1ページごとに20件取り出した場合、と例え(動物園)を出してみましたが...<br>
本来であれば以下のコードを書く必要があります。<br><br>
`Animal.page(params[:page]).per(20)`<br>
このコードの意味としては、「**動物一覧テーブルから指定されたページ(1ページ目,2ページ目など)のデータを20件取得する**」ということになります。<br>
#### いちいちperを指定してられないよ!!!
という方には、`config/initializers/kaminari_config.rb`内にてアプリケーション全体のページネーションを設定することが出来ます。<br>
以下のコマンドを実行して、作成されたconfigファイルにて`config.default_per_page`を指定できます(コメントアウトを解除する必要があります)<br>
`rails g kaminari:config`<br>
docker環境の方は以下のコマンドになります。<br>
`docker compose exec web rails g kaminari:config`<br>

※仮に20件取得にしたい場合...<br>
```ruby:kaminari_config.rb
# frozen_string_literal: true

Kaminari.configure do |config|
  config.default_per_page = 20 
  # config.max_per_page = nil
  # config.window = 4
  # config.outer_window = 0
  ...
end
```
この設定をすれば、以下のコードでも20件取得することが可能になります！<br>
`Animal.page(params[:page])` = 自動的に20件呼び出される<br>
`Animal.page(params[:page].per(30))` = 個別に設定すれば30件の取得も可
