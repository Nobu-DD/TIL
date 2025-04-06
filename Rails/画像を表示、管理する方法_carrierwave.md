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
mount_uploaderを定義することで、アップローダーと画像保存(画像ファイルの名前)されているテーブルのモデルと連携することが出来ます。
そうすると、画像のアップロードや画像の取得、さらには保存されている画像のURLの取得が可能になる(モデルにcarrierwaveが新たな動的メソッドを追加する)
### 5. アップロードできるファイルを指定する
```ruby
# board_image_uploader.rb

class BoardImageUploader < CarrierWave::Uploader::Base
  ...

  def extension_allowlist
    %w[jpg jpeg gif png]
  end

  ...
end
```
#### %wについて
まず%記法について復習する
#### %記法
主に配列や変数の値を定義する時、文字列だったら' 'や" "などのエスケープで囲む必要がありますが、％記法を使うことで
クォーテーションなどのエスケープを省略することが出来ます。
#### 改めて%wとは？
上記で書いたような配列の中身を定義する時に使用します。
わざわざクォーテーションなどで囲む必要がなく、[]と空白で区切ることで配列として機能します
`%w[jpg jpeg gif png]`
### 6. 画像が入っていない掲示板の場合、デフォルトの画像を指定する
もし画像が登録されていない掲示板が表示された場合、画像表示が空欄になってしまいます。
なので事前にデフォルトの画像を登録、画像を指定することで、アプリケーションのデザインを損なわずに済ますことが出来ます。
```ruby
board_image_uploader.rb

class BoardImageUploader < CarrierWave::Uploader::Base
  ...
  def default_url
    'board_placeholder'
  end
  ...
end
```
※ここから書くのは完全に僕の想定している内容なので、間違っている可能性があります
今回BoardImageUploaderクラスにdefault_urlを定義していますが、
継承元のCarrierWaveモジュール内のUploaderモジュール内にdefault_urlメソッドを使用する処理が記述されているはず(多分)
そこでuploaderファイル内にdefault_urlを定義することで、掲示板に表示する初期画像を指定することが出来る。
