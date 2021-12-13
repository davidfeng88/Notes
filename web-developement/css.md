# HTML

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

### \<meta>

```markup
<meta charset="utf-8">
<meta name="key" content="value">
```

## URL Intro

18 characters are reserved: they can only appear at certain places in URL. If those characters are used elsewhere in URL, they need to be escaped (`%` + ASCII code). Encoding other characters are not recommended.

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

Other characters (not reserved, not legal) will be encoded to UTF-8 by browser (汉字).

{% embed url="https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/encodeURIComponent" %}

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

## HTML character encoding

```markup
<p>hello</p>
<!-- 等同于 -->
<p>&#104;&#101;&#108;&#108;&#111;</p>
<!-- 等同于 -->
<p>&#x68;&#x65;&#x6c;&#x6c;&#x6f;</p>
```

entity: `&name;`

* `<`：`&lt;`
* `>`：`&gt;`
* `"`：`&quot;`
* `'`：`&apos;`
* `&`：`&amp;`
* `©`：`&copy;`
* `#`：`&num;`
* `§`：`&sect;`
* `¥`：`&yen;`
* `$`：`&dollar;`
* `£`：`&pound;`
* `¢`：`&cent;`
* `%`：`&percnt;`
* `*`：`$ast;`
* `@`：`&commat;`
* `^`：`&Hat;`
* `±`：`&plusmn;`
* 空格：`&nbsp;`

## HTML tags

* `<header>`
* `<footer>`
* `<main>`
* `<article>`
* `<aside>`
* `<section>`
* `<nav>`
* `<h1>` \~ `<h6>`
* `<hgroup>`

### Text tags

* \<br>: for inline elements

```markup
<pre>hello

   world</pre>

preformatted, keeps spaces and newline
multiline <code> has to be in <pre>
<pre>
<code>
  let a = 1;
  console.log(a);
</code>
</pre>
```

* \<strong> \<b>
* \<em> \<i> (emphasize)
* \<sub> \<sup> \<var> (variable in math formula)
* \<u> underline \<s> strikethrough
* \<blockquote> \<cite> \<q>
* `<ins>`标签是一个行内元素，表示原始文档添加（insert）的内容。`<del>`与之类似，表示删除（delete）的内容。它们通常用于展示文档的删改。
* \<small>: no need for CSS.
* \<mark>: for highlight.
* \<ruby> for pinyin [https://wangdoc.com/html/text.html#ruby](https://wangdoc.com/html/text.html#ruby)

### Table tags

* \<ol>: attributes: reversed, start="5", type
  * `type`属性指定数字编号的样式。目前，浏览器支持以下样式。
    * `a`：小写字母
    * `A`：大写字母
    * `i`：小写罗马数字
    * `I`：大写罗马数字
    * `1`：整数（默认值）

```markup
<ol type="a" start="3">
  <li>列表项 A</li>
  <li>列表项 B</li>
  <li>列表项 C</li>
</ol>
```

* \<li>: value

```markup
<ol>
  <li>列表项 A</li>
  <li value="4">列表项 B</li>
  <li>列表项 C</li>
</ol>
```

### Image

responsive image [https://wangdoc.com/html/image.html](https://wangdoc.com/html/image.html)

### Link

* \<a> can have `mailto:` as href.
* \<link>: rel="stylesheet" relation

### Other tags

* tables [https://wangdoc.com/html/table.html](https://wangdoc.com/html/table.html)
* Forms [https://wangdoc.com/html/form.html](https://wangdoc.com/html/form.html)
* New tags
  * \<dialog>: native modal
  * \<details> \<summary>
