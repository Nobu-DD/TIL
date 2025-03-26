#collectionオプションと省略記法
### collectionオプション
モデルで作ったインスタンス変数(今回は@boards)をcollectionで定義することで
「@boardsを使ってeach(繰り返し)するよ！」と認識するそうです。
`<%= render 'board', collection: @boards %>`
このようにeachを使うと3行になるのを1行でまとめることが出来る
###### これに省略記法を使うと...?
`<%= render @boards %>`
今回のパターンで必須になる条件は(呼び出し元・**indexファイル**、部分テンプレートファイル・**_boardファイル**)

1. indexファイル(元)と_boardファイル(部分)が同じディレクトリ
2. _boardファイル(部分)が単数形
3. _boardファイル内(部分)の変数名がindexファイル(元)で指定した変数(@boards)の単数形(board)

これに全て当てはまると省略記法が使えるそうです。
