# CSS

## How to center an &lt;img&gt;?

Method 1: &lt;img&gt; is an `inline` element. We can center it by converting it to a `block` element, then use the `margin: 0 auto;` trick.

```css
img.center {
    display: block;
    margin: 0 auto;
}
```

Method 2: if the parent element is a `block` element \(e.g. &lt;div&gt;\), we can use `text-align: center;` on the parent.

