# gem 'carrierwave'
Webアプリケーションにファイルアップロード機能を簡単に実装できるgem。<br>
[Git hub carriewwave](https://github.com/carrierwaveuploader/carrierwave)<br>
# 実装手順
### 1. gemをインストール、dockerを再起動する
```
# Gemfile

gem 'carrierwave', '2.2.2'
```
gemインストール<br>
`$ docker compose run web bundle install`<br>
docker 再起動<br>
`$ docker compose restart`<br>
### 2. アップローダーを生成し、アップローダーファイル作成<br>
`$ docker compose exec web rails generate uploader (アップローダーファイル名)`<br>
補足：GithubのREADMEには以下の手順で書かれている。上記のコマンドで以下２つのコマンドを一度に処理することが出来る。
  - アップローダーを生成<br>
  `$ rails generate uploader Avatar`<br>
  - アップローダーのファイルを作成する<br>
  `$ app/uploaders/(アップローダーファイル名.rb)`
### 3. 画像を保存しておくテーブルに`(テーブル名)_image`カラムを追加し、データベースに反映(migration)させる。
マイグレーションファイル作成<br>
`$ docker compose exec web rails g migraion (マイグレーション名) (追加するカラム)`<br>
マイグレーションファイルの内容をデータベースに反映させる<br>
`$ docker compose exec web rails db:migrate`<br>
補足：マイグレーションの実行状況を確認したい場合<br>
`$ docker compose exec web rails db:migrate:status`<br>
### 4. 画像を保存するモデルファイルにアップローダーをマウント(接続、関連付け)する。
```
class モデル名 < ApplicationRecord
  ...
  ...
  mount_uploader :(テーブル名)_image, (アップローダーファイル名)
end
```
#### mount_uploader
mount_uploaderを定義することで、アップローダーと画像保存されているモデルファイルと連携することが出来ます。
そうすると、