# URL processing

{% embed url="https://stackoverflow.com/a/15979390" %}

```javascript
var parser = document.createElement('a');
parser.href = "http://example.com:3000/pathname/?search=test#hash";

parser.protocol; // => "http:"
parser.host;     // => "example.com:3000"
parser.hostname; // => "example.com"
parser.port;     // => "3000"
parser.pathname; // => "/pathname/"
parser.hash;     // => "#hash"
parser.search;   // => "?search=test"
parser.origin;   // => "http://example.com:3000"
```

browser compatibility: [https://developer.mozilla.org/en-US/docs/Web/API/HTMLHyperlinkElementUtils/hostname](https://developer.mozilla.org/en-US/docs/Web/API/HTMLHyperlinkElementUtils/hostname)

