{ "category": "JavaScript",  "order": 4, "date": "2024-04-28 14:00" }
---
# JavaScriptでURLが有効か判別する

JavaScriptでURLが有効か判別するには[URL.canParse()](https://developer.mozilla.org/en-US/docs/Web/API/URL/canParse_static)を使います。

```js
URL.canParse('https://example.com/foo#bar?a=1&b=2')
// true
URL.canParse('foobar')
// false
```