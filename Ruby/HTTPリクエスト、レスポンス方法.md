# 概要
フレームワークに頼らず、Rubyのみを使用したリクエスト、レスポンス処理を行いたいので、その方法をまとめてみた。
## HTTPリクエスト(GET) 簡易的な方法
1. URIクラスのparseクラスメソッドを呼び出し、リクエスト先のURIを指定する。指定したURIはhost名やポート番号などを単体で取得出来る。
`uri = URI.parse("https://sample.com/api")`
2. Net::HTTPクラスのget_responseクラスメソッドを呼び出し、指定したURIにリクエストを送った結果、レスポンスとして取得出来る。
`response = Net:HTTP.get_response(uri)`
## HTTPリクエスト(POST)
1. get送信と同じく、URIクラスを呼び出し、URIを指定。
`uri = URI.parse("https://sample.com/api")`
2. HTTPクラスのインスタンスを作成する。引数には先ほど作成したURIクラスを使用してhost名(sample.com)とport名(443)を指定
`http = Net::HTTP.new(uri.host, uri.port)`
