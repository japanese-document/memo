{ "category": "Go",  "order": 5, "date": "2024-04-19 20:30" }
---
# GoでArray.unshift()を行う

GoでJavaScriptの`Array.unshift()`のような処理を行うには以下のようにします。

```go
package main

import "fmt"

func main() {
	a := []int{1, 2, 3}
	a = append([]int{0}, a...)
	// [0 1 2 3]
	fmt.Print(a)
}
```