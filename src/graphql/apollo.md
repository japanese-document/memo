{ "category": "GraphQL",  "order": 4, "date": "2024-05-06 21:00" }
---
# Apolloメモ

* __typenameとidでデータを特定してキャッシュに反映する。  
https://www.apollographql.com/docs/react/why-apollo/#zero-config-caching

* 同じクエリはキャッシュを利用することができるが、別のクエリで使用するには設定が必要  
https://www.apollographql.com/docs/react/caching/advanced-topics/#cache-redirects

* 手動でデータを取得するには[refetch](https://www.apollographql.com/docs/react/data/queries/#queryresult-interface-refetch)を使うか、[useLazyQuery](https://www.apollographql.com/docs/react/data/queries/#manual-execution-with-uselazyquery)を使う。

* [useSuspenseQuery](https://www.apollographql.com/docs/react/data/suspense#fetching-with-suspense)を`<Suspense>`の子コンポーネントで使うと、リクエスト中はfallbackが表示される。

* `<Suspense>`の中に`<Suspense>`を入れると各`<Suspense>`内でデータを取得すると上位の`<Suspense>`でのデータ取得を待つ必要があるので表示が遅くなります。  
その場合は[useLoadableQuery、useBackgroundQuery、useReadQuery](https://www.apollographql.com/docs/react/data/suspense/#avoiding-request-waterfalls)を使います。リクエストをスキップするには[skipToken](https://www.apollographql.com/docs/react/api/react/hooks/#skiptoken)を指定します。

* Reactのスコープの外やルーターと組み合わせて使う場合は[preloadQuery](https://www.apollographql.com/docs/react/data/suspense/#initiating-queries-outside-react)を使う。
