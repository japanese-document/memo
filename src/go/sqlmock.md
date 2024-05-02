{ "category": "Go",  "order": 7, "date": "2024-05-02 18:30" }
---
# sqlmockメモ

* `WithArgs()`に任意の引数を指定するには`sqlmock.AnyArg()`を渡す。

* [sqlx](https://japanese-document.github.io/sqlx/)と一緒に使う方法

```go
db, mock, err := sqlmock.New()
defer db.Close()
sqlxDB = sqlx.NewDb(db, "postgres") 
```

* `ExpectExec()`に渡す引数は[regexp.QuoteMeta()](https://pkg.go.dev/regexp#QuoteMeta)を適用して使うと便利