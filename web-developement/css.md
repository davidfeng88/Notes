# HTML, CSS

## Ref

{% embed url="https://wangdoc.com/html/index.html" %}

## HTML Intro

* block elements: take a whole line.
* inline elements: no new line.

Tags are case insensitive. `<img>` is the same as `<IMG>`.

Attributes are case insensitive. `onclick` is the same as `onClick`. Usually values are in double quotes.

HTML ignores indentation and newlines.

```markup
<title>My Blog</title>

is the same as

<title>
    My Blog
</title>

<!-- 这是一个注释 -->
<!--
  <p>hello world</p>
-->
```

Browser combines multiple whitespaces, \t, \n, \r.

```markup
<p>hello      world</p>
is the same as
<p>hello world</p>
```

### &lt;meta&gt;

```markup
<meta charset="utf-8">
<meta name="key" content="value">
```

## URL Intro

18 characters are reserved: they can only appear at certain places in URL. If those characters are used elsewhere in URL, they need to be escaped \(`%` + ASCII code\). Encoding other characters are not recommended.

* `space`：%20
* `!`：%21
* `#`：%23
* `$`：%24
* `&`：%26
* `'`：%27
* `(`：%28
* `)`：%29
* `*`：%2A
* `+`：%2B
* `,`：%2C
* `/`：%2F
* `:`：%3A
* `;`：%3B
* `=`：%3D
* `?`：%3F
* `@`：%40
* `[`：%5B
* `]`：%5D

Other characters \(not reserved, not legal\) will be encoded to UTF-8 by browser \(汉字\).

## Attributes

```markup
<p id="p1" class="big bright"></p>
<div title="版权说明">
  <p>本站内容使用创意共享许可证，可以自由使用。</p>
</div>
title will be shown as a tooltip when hovered.
```

`data-` attributes can be used by CSS or JavaScript. CSS example:

```css
/* HTML 代码如下
<div data-role="mobile">
Mobile only content
</div>
*/
div[data-role="mobile"] {
  display:none;
}

/* HTML 代码如下
<div class="test" data-content="This is the div content">test</div>​
*/
.test {
  display: inline-block;
}
.test:after {
  content: attr(data-content);
}
```

event handler: values are JavaScript code. Keys:

`onabort, onautocomplete, onautocompleteerror, onblur, oncancel, oncanplay, oncanplaythrough, onchange, onclick, onclose, oncontextmenu, oncuechange, ondblclick, ondrag, ondragend, ondragenter, ondragexit, ondragleave, ondragover, ondragstart, ondrop, ondurationchange, onemptied, onended, onerror, onfocus, oninput, oninvalid, onkeydown, onkeypress, onkeyup, onload, onloadeddata, onloadedmetadata, onloadstart, onmousedown, onmouseenter, onmouseleave, onmousemove, onmouseout, onmouseover, onmouseup, onmousewheel, onpause, onplay, onplaying, onprogress, onratechange, onreset, onresize, onscroll, onseeked, onseeking, onselect, onshow, onsort, onstalled, onsubmit, onsuspend, ontimeupdate, ontoggle, onvolumechange, onwaiting`

## How to center an &lt;img&gt;?

Method 1: &lt;img&gt; is an `inline` element. We can center it by converting it to a `block` element, then use the `margin: 0 auto;` trick.

```css
img.center {
    display: block;
    margin: 0 auto;
}
```

Method 2: if the parent element is a `block` element \(e.g. &lt;div&gt;\), we can use `text-align: center;` on the parent.

