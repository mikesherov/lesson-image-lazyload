# Lazy-loading images

Open `index.html` in a browser (no build step: `python3 -m http.server` and visit it, or just
double-click the file).

## Before

Every `<img>` and the `<iframe>` load immediately, all at once, no matter how far down the page
they are:

```html
<img src="https://via.placeholder.com/400x1000"/>
<img src="https://via.placeholder.com/400x1001"/>
<img src="https://via.placeholder.com/400x1002"/>
<img src="https://via.placeholder.com/400x1003"/>
<img src="https://via.placeholder.com/400x1004"/>
<img src="https://via.placeholder.com/400x1005"/>
<iframe src="http://example.com" width="400" height="400"></iframe>
```

## After

Adding `loading="lazy"` defers each one until it's about to scroll into view, including inside a
`<picture>` and on an `<img>` with `srcset`:

```html
<img loading="lazy" src="https://via.placeholder.com/400x1000"/>
<img loading="lazy" src="https://via.placeholder.com/400x1001"/>
<img loading="lazy" src="https://via.placeholder.com/400x1002"/>
<img loading="lazy" src="https://via.placeholder.com/400x1003"/>
<picture>
  <source media="(min-width: 100px)" srcset="https://via.placeholder.com/1200x3000">
  <img loading="lazy" src="https://via.placeholder.com/400x1004"/>
</picture>
<img loading="lazy"
  srcset="https://via.placeholder.com/400x1005 400w, https://via.placeholder.com/800x2010 800w"/>
<iframe loading="lazy" src="http://example.com" width="400" height="400"></iframe>
```

Open the Network tab, load the page, and scroll: with `loading="lazy"`, only what's near the
viewport fetches at first, and the rest loads in as you scroll down to it.
