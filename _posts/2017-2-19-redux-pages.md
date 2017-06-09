---
layout: post
title: ReduxでのRoutingはredux-pagesでやっていく
---

Reduxでの理想のRoutingライブラリを求めて作ったredux-pagesの紹介記事です。

リポジトリ: [ryo33/redux-pages](https://github.com/ryo33/redux-pages)

実用例: [ryo33/Yayaka19](https://github.com/ryo33/Yayaka19)

## 以前の方法での不満
redux-pagesを作るに当たって、まず、前にSPAサイトを作っていて不満に感じたことをまとめました。
その中で、特に解決したかったものが以下の4つです。

1. Pathのパース結果はRouter(View)だけでなく、Middlewareでも必要である
2. 現在のページがStateに格納されていてほしい
3. Pathをリダイレクト処理などで手書きしたくない
4. ページ遷移に関連する処理はMiddlewareに書きたい

(1)で言いたいことは、Pathのパース結果は画面描画時だけではなく、
表示されているページによってMiddlewareでの処理内容を変えたいときにも必要だということです。
redux-pagesのREADMEに書かれている
> Parse once, route anywhere

は、このことを言っています。

(2)は、Pathと画面表示は常に一致しているわけではないということです。
例えば、閲覧権限の無いページのPathにアクセスして、エラーページのPathにリダイレクトされるまでの間は何を描画するべきでしょうか。
また、Webサイトの中にはページ移動後にコンテンツのダウンロードが終わるまでは前のページが表示されたままのものがあります。
このように、Pathと画面表示は常に一致するわけではありません。
しかし、Routerで描画したり、Middlewareで処理の分岐をしたりするためには、
現在のページが常に分かるようになっている必要があります。

(3)は、Pathを手書きすると、間違っていても発見しづらく、またPath形式の変更が容易でなくなってしまうためです。

(4)は個人的な好みです。

## redux-pagesの基本的なアイデア
以上で述べた不満を元にした基本的な考えは以下の3つです。

1. Stateには現在どのページを表示すべきなのかが保持される
2. ページオブジェクトを定義して、ページ遷移Actionや、そのページへのPathはメソッドで生成する
3. ページ遷移は単一のActionをdispatchすることで行う

不満1を解消するのは(1)(2)、不満2を解消するのは(1)、不満3を解消するのは(2)、不満4を解消するのは(3)です。

(3)で単一のActionになっているのはMiddlewareでのページ遷移処理を単純なコードで書くためです。
例えば別のページへのリダイレクトは`dispatch`するだけでいいですし、
ページ遷移を遅らせるときも単に`nextDispatch`を呼ぶのを遅らせるだけです。

## 基本的な動作イメージ
redux-pagesを使ってページ遷移を行うときの基本的な動きを説明します。

### Actionをdispatchしてページ遷移
1. redux-pagesのMiddlewareにActionが到達したときにPathが変更される
2. Middlewareで様々な処理が行われる
3. Stateが変更されViewもそれに応じて変更される

これが、基本的なページ遷移時の処理の流れです。
ユーザー認証やリダイレクトなどは(2)のMiddlewareの中で行います。

### locationの変化によるページ遷移
こちらは、ブラウザの戻るボタンなどが押されたときに使われます。

1. Pathのパース結果でActionが投げられる
2. Middlewareで様々な処理が行われる
3. Stateが変更されViewもそれに応じて変更される

## 実用
redux-pagesは[Yayaka19](yayaka.net)というSPAアプリケーションで実際に使用しました。
とりあえず今のところは大きな問題など無く運用できてます。

前述の不満の解消以外の利点としては複数のページに適用したい処理をまとめて書けることが挙げられます。
例えば、アクセス権限のあるページかどうか調べてリダイレクトを行う処理を各ページごとに分けて書く必要はなく、
単に`allowedPageNames.includes(action.payload.name)`というような条件文に応じてリダイレクトを行うだけです。

あと、使ってから気づいたのですが、`nextDispatch`を遅らせることによりページ遷移を遅らせる技は滅多に使わないと思います。
ロードが終了するまで前ページを表示するよりは、ページ遷移時のHookでisLoading状態にして、あのグルグルを表示したほうがSPAぽくなります。

ちなみにページ遷移時のMiddlewareの処理には[redux-middlewares](https://github.com/ryo33/redux-middlewares)を用いています。
例えば、ログイン確認とそれによるリダイレクトには`createReplacer`を、ページ遷移時のHookには`createAsyncHook`を使います。
個人的に直感的にMiddlewareを書くことができるので気に入っています。
ただ、既に[redux-saga](https://github.com/redux-saga/redux-saga)や
[redux-logic](https://github.com/jeffbski/redux-logic)などを導入している場合はそちらを使ったほうがいいです。

## 課題
例えば`/posts/:postNumber`というパスがあって`postNumber`を文字列型から数値型にキャストしたいといったときに、
エラー処理も含めるとMiddlewareでActionを変換する必要があります。
これをどうにかページ定義時に書くことができるようにしたいと考えています。

あとは、導入時の雛形が比較的多いのもどうにかして減らしたいです。

## さいごに
割とまともに使えて、辛いポイントもそんなにないので、とりあえずしばらくはこれを使っていこうと思います。
