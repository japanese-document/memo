{ "category": "Go",  "order": 2, "date": "2024-04-06 14:30" }
---
# gqlgen generateがうまくいかない

`go run github.com/99designs/gqlgen generate`を実行すると以下のエラーが発生しました。

```
../../go/pkg/mod/github.com/99designs/gqlgen@v0.17.45/internal/code/packages.go:14:2: missing go.sum entry for module providing package golang.org/x/tools/go/packages (imported by github.com/99designs/gqlgen/codegen/config); to add:
```

以下の内容の`tools.go`ファイルを作成して、`go mod tidy`を実行すると解決しました。

```go
//go:build tools
// +build tools

package tools

import (
	_ "github.com/99designs/gqlgen"
	_ "github.com/99designs/gqlgen/graphql/introspection"
)
```