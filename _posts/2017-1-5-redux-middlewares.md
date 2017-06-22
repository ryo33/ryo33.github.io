---
layout: post
title: ReduxでMiddlewareを使ってサクッと解決する
---

## はじめに

これはredux-middlewaresの紹介です。

## Middlewareについて

Middlewareがどのようなものかついては、非常に分かりやすい記事があるのでそれを貼っておきます。  
http://qiita.com/kuy/items/57c6007f3b8a9b267a8e  
僕がMiddlewareを活用するようになったのは、この記事を読んでからです。

[Optional] こちらの記事も併せて読むとさらに理解が進むと思います。  
http://qiita.com/kuy/items/c6784fe443f1d5c7bbdc

## Stateの整合性を保つ
Stateの整合性を壊してしまう可能性のあるAction (DANGEROUS_ACTION)があり、様々な場所から投げられる可能性があるとします。
また、そのアクションが整合性を保つか確かめることができる`validateAction(state, action) => boolean`という関数が定義されているとします。

Stateの整合性を保つためにはActionを投げる前に必ず確かめるという方法がありますが、Middlewareを使うことで、もっと簡単で確実に整合性を保つことができます。

まず生のmiddleware APIを使った例を示します。
```javascript
const filterMiddleware = ({getState, dispatch}) => nextDispatch => action => {
  if (action.type === DANGEROUS_ACTION) {
    if (validateAction(getState(), action)) {
      return nextDispatch(action)
    }
  } else {
    return nextDispatch(action)
  }
}
```
これでもできますが`({getState, dispatch}) => nextDispatch => action`の部分などをすべてのMiddlewareで毎回書くのは面倒です。
これをredux-middlewaresの`createMiddleware`関数を使って書いてみると、このようになります。
```javascript
const filterMiddleware = createMiddleware(
  ({ action }) => action.type === DANGEROUS_ACTION,
  ({ getState, nextDispatch, action }) => {
    if (validateAction(getState(), action)) {
      return nextDispatch(action)
    }
  }
)
```
また、`({ action }) => action.type === DANGEROUS_ACTION`の部分を短縮して、このようにも書くことができます。
```javascript
const filterMiddleware = createMiddleware(
  DANGEROUS_ACTION,
  ({ getState, nextDispatch, action }) => {
    if (validateAction(getState(), action)) {
      return nextDispatch(action)
    }
  }
)
```
redux-middlewaresの`createFilter`関数を使うことで、さらに簡単に書くことができます。
```javascript
const filterMiddleware = createFilter(
  DANGEROUS_ACTION,
  ({ getState, action }) => validateAction(getState(), action)
)
```

## 非同期処理をキャンセルする
Reduxでの非同期処理をどこでどのように行うかが議論になっているのをよく見ますが、
この例ではMiddlewareで非同期処理とそのキャンセル処理を実装します。

redux-middlewaresの`createMiddleware`関数と`composeMiddleware`関数を使います。

`composeMiddleware`関数は複数のMiddlewareを１つのMiddlewareにまとめることができる関数です。
```javascript
const createAsyncMiddleware = () => {
  let task = null
  const cancelMiddleware = createMiddleware(
    CANCEL_ASYNC_TASK, // action.type === CANCEL_ASYNC_TASK
    () => { // Cancellation
      if (task) {
        clearTimeout(task)
        task = null
      }
    }
  )
  const taskMiddleware = createMiddleware(
    START_ASYNC_TASK, // action.type === START_ASYNC_TASK
    ({ action }) => { // Async task
      const { text } = action.payload
      task = setTimeout(() => console.log(text), 1000)
    }
  )
  return composeMiddleware(
    cancelMiddleware, taskMiddleware
  )
}

const asyncMiddleware = createAsyncMiddleware()
```
`cancelMiddleware`と`taskMiddleware`内の処理は同じMiddlewareの中に書くこともできますが、
２つに分けることで処理が分かりやすくなっています。

## さいごに

redux-middlewaresの詳細なAPIは[ryo33/redux-middlewares](https://github.com/ryo33/redux-middlewares)を参照してください。
