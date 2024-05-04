{ "category": "GraphQL",  "order": 4, "date": "2024-05-04 21:00" }
---
# Apolloメモ

* __typenameとidでデータを特定してキャッシュに反映する。  
https://www.apollographql.com/docs/react/why-apollo/#zero-config-caching

* 同じクエリはキャッシュを利用することができるが、別のクエリで使用するには設定が必要  
https://www.apollographql.com/docs/react/caching/advanced-topics/#cache-redirects

* 手動でデータを取得するには[refetch](https://www.apollographql.com/docs/react/data/queries/#queryresult-interface-refetch)を使うか、[useLazyQuery](https://www.apollographql.com/docs/react/data/queries/#manual-execution-with-uselazyquery)を使う。

* [useSuspenseQuery](https://www.apollographql.com/docs/react/data/suspense#fetching-with-suspense)を`<Suspense>`の子コンポーネントで使うと、リクエスト中はfallbackが表示される。