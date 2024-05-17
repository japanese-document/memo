
{ "category": "GraphQL",  "order": 5, "date": "2024-05-17 12:15" }
---
# graphql-codegenのエラー

[graphql-codegen](https://github.com/dotansimha/graphql-code-generator)を実行すると下記のエラーが発生しました。

```
GraphQL Document Validation failed with 1 errors;
Error 0: This anonymous operation must be the only defined operation.
```

下記のようにqueryやmutationに名前を付与することで解決しました。

`mutation ($input: NewTodo!) {` -> `mutation add ($input: NewTodo!) {`

`query {` -> `query todos {`