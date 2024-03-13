{ "category": "JavaScript",  "order": 0, "date": "2024-03-13 22:20" }
---
# reduceを使った関数合成

```js
const f = [10000, 2000, 300, 40].map((i) => (a) => {console.log(i); return a + i}).reduce((a, c) => (v) => c(a(v)))
console.log(f(5))
// 10000
// 2000
// 300
// 40
// 12345
```