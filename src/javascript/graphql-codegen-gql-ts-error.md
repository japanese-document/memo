{ "category": "JavaScript",  "order": 1, "date": "2024-04-07 14:00" }
---
# graphql-codegenを実行するとgql.tsがエラーになる

`graphql-codegen`を実行すると`gql.ts`で下記のようなエラーが発生しました。

```
Variable 'documents' implicitly has type 'any[]' in some locations where its type cannot be determined.
```

`codegen.ts`の`documents`に指定しているファイルに以下のようなクエリを記述して`graphql-codegen`を実行することでエラーが消えます。

```ts
const GET_CARS = gql(/* GraphQL */ `
query findCars {
  cars {
    name
    price
    user {
      name
    }
  }
}`);
```