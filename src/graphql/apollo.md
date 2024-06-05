{ "category": "GraphQL",  "order": 4, "date": "2024-05-13 21:00" }
---
# Apolloメモ

## リクエスト 

レスポンスに応じたデフォルトの処理をセットする  
https://www.apollographql.com/docs/react/networking/advanced-http-networking/#customizing-response-logic

fetch関数を変更する  
https://www.apollographql.com/docs/react/networking/advanced-http-networking#custom-fetching

### Query

* 手動でデータを取得するには[refetch](https://www.apollographql.com/docs/react/data/queries/#queryresult-interface-refetch)を使うか、[useLazyQuery](https://www.apollographql.com/docs/react/data/queries/#manual-execution-with-uselazyquery)を使う。

* [useSuspenseQuery](https://www.apollographql.com/docs/react/data/suspense#fetching-with-suspense)を`<Suspense>`の子コンポーネントで使うと、リクエスト中はfallbackが表示される。

* `<Suspense>`の中に`<Suspense>`を入れると各`<Suspense>`内でデータを取得すると上位の`<Suspense>`でのデータ取得を待つ必要があるので表示が遅くなります。  
その場合は[useLoadableQuery、useBackgroundQuery、useReadQuery](https://www.apollographql.com/docs/react/data/suspense/#avoiding-request-waterfalls)を使います。リクエストをスキップするには[skipToken](https://www.apollographql.com/docs/react/api/react/hooks/#skiptoken)を指定します。

* Reactのスコープの外やルーターと組み合わせて使う場合は[preloadQuery](https://www.apollographql.com/docs/react/data/suspense/#initiating-queries-outside-react)を使う。

* 取得してデータを変更してはいけない。

### Mutation

* mutationの結果(loading、errors)を初期化したい場合は[reset](https://www.apollographql.com/docs/react/data/mutations/#resetting-mutation-status)を使う。(キャッシュは初期化されない)

* サーバのレスポンスではなく、リクエスト内容をキャッシュに反映するには`optimisticResponse`を使う。

### Header

* 静的にヘッダーをセットするには[headers](https://www.apollographql.com/docs/react/networking/basic-http-networking)を設定する。

* 動的にヘッダーをセットするには[setContext()](https://www.apollographql.com/docs/react/networking/authentication#header)を使う。

## Link

* [onError](https://www.apollographql.com/docs/react/api/link/apollo-link-error)はリクエストの後に実行される

* [forward](https://www.apollographql.com/docs/react/api/link/introduction/#the-forward-function)で次のリンクに移動

* [forwaordの戻り値のmap()](https://www.apollographql.com/docs/react/api/link/introduction/#handling-a-response)でリクエスト後の処理を登録することができる

* Operationオブジェクトの[setContext()](https://www.apollographql.com/docs/react/api/link/introduction/#managing-context)の[context](https://www.apollographql.com/docs/react/api/link/apollo-link-http#context-options)


## キャッシュ

* クエリとそのパラメータごとにキャッシュされる。

* 同じクエリはキャッシュを利用することができるが、別のクエリで使用するには設定が必要  
https://www.apollographql.com/docs/react/caching/advanced-topics/#cache-redirects

* mutationの結果をキャッシュに反映したい場合は、mutationのレスポンスで[`__typename`と`id`](https://www.apollographql.com/docs/react/data/mutations/#include-modified-objects-in-mutation-responses)(mutationの戻り値だけではidに対応するデータを取得する目的のキャッシュを変更することはできるが、キャッシュされている配列の中に存在するデータを変更することはできない)を返すか、[update関数を定義する](https://www.apollographql.com/docs/react/data/mutations/#the-update-function)か、[onQueryUpdatedコールバック関数を定義する](https://www.apollographql.com/docs/react/data/mutations/#refetching-after-update)。

* 手動でキャッシュを更新したい場合、[client.refetchQueries()](https://www.apollographql.com/docs/react/data/refetching/)を使う。  
Reactではclientは[useApolloClient](https://www.apollographql.com/docs/react/api/react/hooks/#useapolloclient)で取得する。

### キャッシュのキー

* デフォルトでは`__typename`と`id`でデータを特定してキャッシュに反映する。  
https://www.apollographql.com/docs/react/why-apollo/#zero-config-caching

* キーに使用するプロパティやキーの生成方法を変更することができる。
https://www.apollographql.com/docs/react/caching/cache-interaction

### キャッシュを操作する

以下はクエリのみ操作する。つまり、サーバにリクエストが送信されない。
Apollo Clientは[InMemoryCache](https://www.apollographql.com/docs/react/api/cache/InMemoryCache/)をプロキシしている。

#### クエリで操作

[client.readQuery](https://www.apollographql.com/docs/react/caching/cache-interaction#readquery) / [client.writeQuery](https://www.apollographql.com/docs/react/caching/cache-interaction#writequery) / [client.updateQuery](https://www.apollographql.com/docs/react/api/cache/InMemoryCache/#updatequery)

* `client.writeQuery`で既存のデータを上書きすることができる。データの一部を渡した場合、その部分のみ上書きされる。

* `updateFragment`と`updateQuery`はデータを取得して、それを基に上書きする処理ができる。

##### readQuery

キャッシュに対応する値がない場合は`null`を返す。

#### フラグメントで操作

`client.readFragment` / `client.writeFragment` / [client.updateFragment](https://www.apollographql.com/docs/react/api/cache/InMemoryCache/#updatefragment) / `client.useFragment`

#### 手動で操作

[cache.modify](https://www.apollographql.com/docs/react/caching/cache-interaction/#using-cachemodify)

[cache.identify(obj)](https://www.apollographql.com/docs/react/caching/cache-interaction#obtaining-an-objects-cache-id)は`obj`のidを取得する。

refのフィールドにアクセスるには`readField()`を使う。

useMutationで使用するとサーバへのアクセスなしにキャッシュを変更することができる。

#### キャッシュからデータを削除する

`cache.evict({ id: 'foo-id' })`はidが`'foo-id'`のデータをキャッシュから削除します。
`cache.gc()`で孤立したデータを削除する。

すべてのキャッシュをクリアするには`client.clearStore()`を使う。

#### fetchMore

* パラメータを無視してクエリ毎にキャッシュするには`typePolicies`を設定する。

* キャッシュされたデータの取得方法を変更するには`typePolicies`の`read`を変更する。

## フラグメント

* [フラグメント](https://graphql.org/learn/queries/#fragments)は再利用可能な型の部分集合

* 以下のように埋め込むことができる

```js
const FooFragment = gql`
  fragment FooFragment on Bar {
    name
  }
`;

const client = new ApolloClient({
  uri: 'https://example.com/',
  cache: new InMemoryCache({
    fragments: createFragmentRegistry(gql`
      ${UserFragment}
    `),
  }),
});
```

* フラグメントはそれを使うクエリに[埋め込む](https://www.apollographql.com/docs/react/data/fragments/#example-usage)か[fragmentsオプション]()に登録する必要がある。

* フラグメントをコンポーネントに関連付ける[例](https://www.apollographql.com/docs/react/data/fragments/#creating-colocated-fragments)

* [possibleTypes](https://www.apollographql.com/docs/react/data/fragments/#using-fragments-with-unions-and-interfaces)にinterfaceやunionの親子関係を記述する。  
https://the-guild.dev/graphql/codegen/plugins/other/fragment-matcher

* [useFragment](https://www.apollographql.com/docs/react/data/fragments/#usefragment)を使うとフラグメントに関連したデータを効率的に取得することができます。

## type

* 以下のように型のフィールドに[引数](https://graphql.org/learn/schema/#arguments)を渡すことができる  
```
type Car {
  id: ID!
  name: String!
  size(unit: LengthUnit = METER): Float
}
```

## ディレクティブ

### GraphQL標準のディレクティブ

https://spec.graphql.org/June2018/#sec-Type-System.Directives

### @client

フィールドがローカルに存在するデータであり、そのデータ取得がサーバーへのクエリではなくクライアント側のキャッシュまたはローカルストアから行われることを示すフラグとして機能します。

### @connect

`@connection`ディレクティブを付与すると、同じクエリでも`@connection`ディレクティブに与えられた値毎に別のクエリとして区別されます。これによって、同じクエリで用途に応じて複数の値をキャッシュすることができます。

### @defer

指定したフィールドを遅延的に取得する。

### @export

指定したフィールドを変数として使うことができるようにする。

### @nonreactive

指定したフィールドが変更されても再レンダリングが発生しないようにする。

## ローカルステート

### リアクティブ

`makeVar`関数を`read`関数か`useReactiveVar`フックを使ってリアクティブな処理にします。

値を取得するには`makeVar`関数の戻り値の関数を実行する。

### @client

`@client`ディレクティブをクエリのフィールドに付与するとローカルにのみ有効なフィールドになる。

`@clinet`ディレクティブが付与されたフィールドの値を生成は`typePolicies`の`read`関数で行う。

`@client @export(as: "authorId")`のようにローカルステートのフィールドをクエリで使うことができます。

### クライアント用のクエリを定義する

[typeDefs](https://www.apollographql.com/docs/react/local-state/client-side-schema/)に`extend type`したクエリを指定する。

## テスト

https://www.apollographql.com/docs/react/development-testing/testing

## パフォーマンス 

バンドルサイズを小さくする方法
https://www.apollographql.com/docs/react/development-testing/reducing-bundle-size

