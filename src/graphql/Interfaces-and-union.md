{ "category": "GraphQL",  "order": 0, "date": "2024-04-15 02:00" }
---
# GraphQLのインターフェイスとユニオンの違い

[インターフェイス(Interfaces)](https://graphql.org/learn/schema/#interfaces)はクエリで共通部分をまとめて記述することができるが、
[ユニオン(Union)](https://graphql.org/learn/schema/#union-types)はできない。
下記の例では、どちらも[インラインフラグメント(Inline Fragments)](https://graphql.org/learn/queries/#inline-fragments)を使っています。インラインフラグメントはクエリの戻り値の型に応じて戻り値のプロパティを変更することができます。

## Interfaces

### スキーマ

```
interface Foo {
  propertyA: String
}

type Bar implements Foo {
  propertyA: String
  propertyB: Int
}

type Baz implements Foo {
  propertyA: String
  propertyC: Boolean
}

type Query {
  foo: Foo
}
```

### クエリ

fooがBarの場合は`propertyA`と`propertyB`を返します。
fooがBazの場合は`propertyA`と`propertyC`を返します。

```
query {
  foo {
    propertyA
    ... on Bar {
      propertyB
    }
    ... on Baz {
      propertyC
    }
  }
}
```

## Union

### スキーマ

```
type Bar {
  propertyA: String
  propertyB: Int
}

type Baz {
  propertyA: String
  propertyC: Boolean
}

union FooUnion = Bar | Baz

type Query {
  foo: FooUnion
}
```

### クエリ

fooがBarの場合は`propertyA`と`propertyB`を返します。
fooがBazの場合は`propertyA`と`propertyC`を返します。

```
query {
  foo {
    ... on Bar {
      propertyA
      propertyB
    }
    ... on Baz {
      propertyA
      propertyC
    }
  }
}
```