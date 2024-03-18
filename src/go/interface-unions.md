{ "category": "Go",  "order": 1, "date": "2024-03-18 19:00" }
---
# Goのinterfaceのunionsの例

```go
package main

import "fmt"

type Foo struct {
	foo string
}

type Bar struct {
	bar string
}

type Baz interface {
	Foo | Bar
}

func f[T Baz](a T) {
	switch b := any(a).(type) {
	case Foo:
		fmt.Println(b.foo)
	case Bar:
		fmt.Println(b.bar)
	}
}

func main() {
	a := Foo{foo: "foo"}
	b := Bar{bar: "bar"}
	f(a)
	f(b)
}
```