https://www.playframework.com/documentation/ja/2.4.x/ScalaHome

# HTTP プログラミング
## アクション、コントローラ。レスポンス
Play App が受け取ったリクエストのほとんどは`Action`によって処理される。
基本的に`play.api.mvc.Action`はリクエストを処理してクライアントへ送るレスポンスを生成する`(play.api.Request => play.api.mvc.Result)`型の関数。
```scala
val echo = Action { request =>
  Ok("Got request [" + request + "]")
}
```
アクションは`play.api.mvc.Result`型の値を返し、これはクライアントへ送信されるHTTPレスポンスを表している。上記の例では`Ok`は`text/plain`のレスポンスボディを含むステータス`200 OK`のレスポンスを生成している。

## アクションの定義
最もシンプルなやつ
```scala
Action {
    Ok("Hello world")
}
```

もちろん引数をとったりもできる。
```scala
Action { request =>
    Ok(s"Got request: $request")
}
```

## コントローラとアクションジェネレータ
`Controller`は `Action`を生成するただのシングルトンオブジェクト。
一番シンプルなやつ

```scala
package cotrollers
import play.api.mvc._

class Application extends Controller {
    def index = Action {
        Ok("It works!")
    }
}
```

## シンプルなResult
Resultとはwebクライアントへ送信されるHTTPレスポンスを表すオブジェクトえｄステータスコードとレスポンスヘッダー式・メッセージボディをまとめたもの。

`play.api.mvc.Result`として定義されている。

```scala
def index = Action {
    Result {
        header = ResponesHeader(200, Map(CONTENT_TYPE -> "text/plain")),
        body = Enumerator("Hello world!".getBytes())
    }
}
```

みたいに書くこともできるし、

```scala
def index = Action {
    Ok("Hello!")
}
```

のように書くこともできる。(↑と同じレスポンスを生成する。)
