# scope(uniqueness:scope)
直訳すると、一意性約の範囲を指定する...(多分)
競馬に例えてみると...
1〜12レースまでに出走する馬と、それに騎乗する騎手がいます。
今回はレース視点で考えてみようと思います。まずは単純にuniquenessを定義した場合...
```ruby:app/models/race.rb
class Race < ApplicationRecord
  belongs_to :horse
  belongs_to :jockey

  validates :jocker_id, uniqueness: true
```
まずIKZEという騎手がいます。そしてIKZE騎手は2頭ほど乗りこなしている(?)馬がいるとしましょう。<br>
IKZE騎手はDream Journeyという馬に乗ることになりました。<br>
無事に騎乗を終えて、次はOrfevreという馬に乗ってレースに向かっていたのですが...<br>
なんと一回レースに出ただけでもう騎乗資格が無いと言われてしまいました！<br>
これを聞いたOrfevreは怒ってしまい、乗っていたIKZEを振り落としてしまいました！<br>
※IKZE騎手は何も悪くありません<br>

反省を生かしたJ◯Aは以下のルールを追記しました
```ruby:app/models/race.rb
class Race < ApplicationRecord
  belongs_to :horse
  belongs_to :jockey

  validates :jocker_id, uniqueness: { scope: :horse_id}
```
このルールによると、１頭の馬に乗る騎手は一人だけ、という範囲指定内で制約することになります。
この一意制約のおかげで、IKZEは別のレースでDream Journerにも、Orfevreにも、たくさんの馬に騎乗することが
出来たのでした✨️めでたしめでたし...

ということで、scopeで指定した`horse_id`が同じ値だった場合のみ、jocker_idに一意制約が適用されるそうです。
