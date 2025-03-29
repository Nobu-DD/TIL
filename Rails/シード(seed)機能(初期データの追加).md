## 概要
railsには**db/seeds.rb**というファイルが存在しています。
これはシードファイルというもので、開発環境やテストでデータベースに初期データを入れておきたい！！
という時に入れたいデータ内容を記述しておけば、コマンド一つで何度も初期データを入れることが出来ます。
## 方法
1. **db/seeds.rb**ファイルに生成したいデータ内容を記述する<br>
※前提として**gem 'faker'を使用する(ランダムな文字列を出力する)<br>
[git hub faker-ruby/faker](https://github.com/faker-ruby/faker?tab=readme-ov-file#generators)
```ruby:db/seeds.rb
10.times do
  User.create!(last_name: Faker::Name.last_name,
               first_name: Faker::name.first_name,
               email: Faker::Internet.unique.email,
               password: "password",
               password_confirmation: "password")
end

user_ids = User.ids

20.times do |index|
  user = User.find(user_ids.sample)
  user.boards.create!(title: "タイトル#{index}", body: "本文#{index}")
end
```
2. 以下のseedコマンドを実行すると初期データがデータベースに投入される

`$ docker compose exec web rails db:seed`
## 解説ポイント
#### create!()
データを作成するためにcreateメソッドを使用するが、!をつけることで**例外**(railsエラー画面)を発生することが出来る<br>
※!をつけないとデータ作成がエラーになってもそのままリダイレクトされてしまう<br>
参考資料:[【Rails/ActiveRecord】createとcreate!の違いについて](https://qiita.com/saitok7/items/20adc17112d6d0bcf9d5)
#### idsメソッド(SQL検索)
`モデル名.ids`と記述することで、主キーを使っているリレーションID(外部キー)を全て取得することが出来る<br>
※思考が整理できないため、一旦保留にします
