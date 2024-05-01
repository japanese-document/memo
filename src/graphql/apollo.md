{ "category": "GraphQL",  "order": 4, "date": "2024-04-29 22:00" }
---
# Apolloメモ

* __typenameとidでデータを特定してキャッシュに反映する。  
https://www.apollographql.com/docs/react/why-apollo/#zero-config-caching

* 同じクエリはキャッシュを利用することができるが、別のクエリで使用するには設定が必要  
https://www.apollographql.com/docs/react/caching/advanced-topics/#cache-redirects

* 手動でデータを取得するには[refetch](https://www.apollographql.com/docs/react/data/queries/#queryresult-interface-refetch)を使う  
もしくは[useLazyQuery](https://www.apollographql.com/docs/react/data/queries/#manual-execution-with-uselazyquery)を使う。