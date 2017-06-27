---
layout: post
title: kuyさんと話したこと
---

## はじめに

以前からお会いしたいと思っていた[kuy](https://twitter.com/kuy)さんと会って話をすることができた。
結構色々なことを話したので備忘録的にブログに書くことにした。

## 技術的な話

※ 私が会話後に思ったことなどが含まれるので、実際に会話したときの結論やkuyさんの考えとは違う可能性がある。

### Redux

Reduxのコンセプトを破った方がうまいこといくならそうするべきか、みたいな話をした。
後からredux-thunkには「別に無理して守る必要は無い」というメッセージが含まれているのかもしれないと思ったりした。

### ReduxとReact

ReduxかReactのどちらが将来的に技術的負債になりやすいかでいうと断然Reactじゃないかみたいな話。
Reduxは割りと簡単に移行できそう？

### React Vue Redux redux-saga の教育コスト

ReactかVueかで迷ったときにReactの方が教育コストが小さいと思って選んだ話とその理由、
redux-sagaでのsagaの作り方よりもReduxのreducerの作り方のほうが難しく感じる人がいて驚いた話などをkuyさんに話した。
各個人が普段やっている分野によってどちらの方が教育コストが高い/低いというのが逆転する場合があるみたいな話や、習得が簡単に感じたとしてもちゃんと理解して使えているかは分からないみたいな話をした。

### Reduxルーターライブラリ合戦

kuyさん作のredux-towerと私のredux-pagesの話。
redux-towerは解決すべき問題に出会う機会が少なくてなかなか進まないという話だった。
redux-pagesはもうほとんど完成形だが、使っているpathのテンプレートエンジンに多少不満があるので別のに乗り換えたいと考えている話をした。
bouzuyaさん作のbathが良さそうという話になった。

### ElixirとPhoenix

ElixirやPhoenixについて聞かれたので、WebサーバーのようなErlang/OTPが向いている分野ではElixirは本当に書きやすいという話をした。
あとPhoenix Channelsが便利。

### オートマトン

オートマトンの魅力や可能性などについてkuyさんに紹介してもらった。
オートマトンを使って最近ボトルネックになりつつあるマシンなどの全体に対して適用されるようなエラー対策（メモリのエラーチェック等）の必要性を無くし、コンピューターの処理速度を更に向上させることを目指す研究があるみたいな話だった（と思う）。

### Ocaml

OcamlとかReasonとかの話を聞いた。
Ocaml製Webサーバーを作る厳しさについての話など。

### Keras

Kerasを使って色々やろうとしていることやKerasとChainerについてよく言われる話を紹介した。
Define and runかDefine by runみたいな話。
一応Keras作者のツイートも紹介した。

### GAN

GANの紹介。深層学習のヤベー奴。

### Word2Vec

Word2Vecは結構応用できるみたいな話。

### Docker

Dockerについてぼんやりとした知識しか持っていなかったのでkuyさんに教えてもらった。

### Kubernetes

Kubernetesで実際にサーバーの再起動とかが行われる様子を見せてもらったりした。

### 標準ライブラリ

標準ライブラリが洗練されていない言語は辛い、みたいな話。

### Yayaka.net

Yayaka.netのソースコードはElixirの学習に使えるか？みたいな話になり、一部に役立つコードもあるかもしれないが、ほとんどのコードは雑に書かれていることを説明した。

### Yayakaプロトコル

話した合計時間的にはこの話題が一番長い。
Yayakaプロトコルはマイクロサービス的で、さらにマイクロフロントエンド的な面も持つみたいな話。
マイクロフロントエンドについては初めて聞いたのでkuyさんに教えてもらった。
時代は脱モノリシック。

## その他の話

### アニメ、音楽、ゲーム、ドラマ、映画

趣味についての話。
ものすごく雑な概要としては、私がフリフラの布教活動をした。