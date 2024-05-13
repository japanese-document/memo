{ "category": "GraphQL",  "order": 4, "date": "2024-05-13 21:00" }
---
# Apolloメモ

## リクエスト 

* 手動でデータを取得するには[refetch](https://www.apollographql.com/docs/react/data/queries/#queryresult-interface-refetch)を使うか、[useLazyQuery](https://www.apollographql.com/docs/react/data/queries/#manual-execution-with-uselazyquery)を使う。

* [useSuspenseQuery](https://www.apollographql.com/docs/react/data/suspense#fetching-with-suspense)を`<Suspense>`の子コンポーネントで使うと、リクエスト中はfallbackが表示される。

* `<Suspense>`の中に`<Suspense>`を入れると各`<Suspense>`内でデータを取得すると上位の`<Suspense>`でのデータ取得を待つ必要があるので表示が遅くなります。  
その場合は[useLoadableQuery、useBackgroundQuery、useReadQuery](https://www.apollographql.com/docs/react/data/suspense/#avoiding-request-waterfalls)を使います。リクエストをスキップするには[skipToken](https://www.apollographql.com/docs/react/api/react/hooks/#skiptoken)を指定します。

* Reactのスコープの外やルーターと組み合わせて使う場合は[preloadQuery](https://www.apollographql.com/docs/react/data/suspense/#initiating-queries-outside-react)を使う。

* mutationの結果(loading、errors)を初期化したい場合は[reset](https://www.apollographql.com/docs/react/data/mutations/#resetting-mutation-status)を使う。(キャッシュは初期化されない)

## キャッシュ

* __typenameとidでデータを特定してキャッシュに反映する。  
https://www.apollographql.com/docs/react/why-apollo/#zero-config-caching

* 同じクエリはキャッシュを利用することができるが、別のクエリで使用するには設定が必要  
https://www.apollographql.com/docs/react/caching/advanced-topics/#cache-redirects

* mutationの結果をキャッシュに反映したい場合は、mutationのレスポンスで[`__typename`と`id`](https://www.apollographql.com/docs/react/data/mutations/#include-modified-objects-in-mutation-responses)(mutationの戻り値だけではidに対応するデータを取得する目的のキャッシュを変更することはできるが、キャッシュされている配列の中に存在するデータを変更することはできない)を返すか、[update関数を定義する](https://www.apollographql.com/docs/react/data/mutations/#the-update-function)か、[onQueryUpdatedコールバック関数を定義する](https://www.apollographql.com/docs/react/data/mutations/#refetching-after-update)。

* 手動でキャッシュを更新したい場合、[client.refetchQueries()](https://www.apollographql.com/docs/react/data/refetching/)を使う。

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