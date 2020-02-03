# Getting started with dual screens

This is a simple walkthrough designed to help you add support for dual screens to your existing web app.

## CSS media queries for dual screens

Dual screen support is primarily through CSS media queries:

```css
/* Dual screens in landscape mode */
@media (spanning: single-fold-vertical) {
   ...
}

/* Dual screens in portrait mode */
@media (spanning: single-fold-horizontal) {
   ...
}
```

These two media queries are visualized below:
![visual representation of dual screen media queries](https://github.com/zouhir/spanning-css-polyfill/raw/master/images/spanning-media-query.svg?sanitize=true)

These media queries allow you to take advantage of dual screens. To extend your existing web app to support dual screens, you'll add these media queries and add your CSS inside of them.

In the [Daily Prophet Magazine demo](https://devday2020e76c4a5534b64a.z21.web.core.windows.net/), we [use these media queries](/styles.css) to change how the app content is displayed:

```css
/* Support dual screens in landscape mode */
@media (spanning: single-fold-vertical) {
    /* Scroll horizontally */
    .wrapper {
        overflow-x: scroll;
    }

    /* Display the article text in columns flowing left to right. */
    .article {
        display: flex;
        flex-wrap: wrap;
        flex-direction: column;
    }
}
```


## Dual screen CSS environment variables

You can utilize dual screen variables in your CSS to layout your content precisely.

```css
/* The article content should fill up one screen. */
.article > * {
    max-width: env(fold-left); /* Right up to the edge of the hinge */
    margin-right: env(fold-width); /* Give some right margin so we don't display text over the hinge. */
}
```

Notice how we use `env(fold-left)` and `env(fold-width)`. These are dual screen CSS environment variables that can help you layout your content precisely on dual screen devices.

There are [4 environment variables](https://github.com/zouhir/spanning-css-polyfill#device-fold-css-environment-variables): 

* fold-top
* fold-left
* fold-width
* fold-height

## JavaScript API

Part of the dual screen web proposed standard includes a JavaScript API to programmatically get access to the screen geometry:

```js
const screens = window.getWindowSegments();
console.log(screens.length); // 2 for dual screen devices
console.log(screens[0]); // {left: 0, top: 0, width: 350, height: 530}
console.log(screens[1]); // {left: 360, top: 0, width: 350, height: 530}
```


## Polyfills for testing in today's browsers

These CSS media queries for dual screen devices are undergoing standardization process, so not all browsers yet support these media queries. Edge Canary will ship support behind a flag in the first quarter of 2020, with upstream Chromium and other browsers shipping later.

That said, you can test on the web today by using the [Spanning Polyfill](https://github.com/zouhir/spanning-css-polyfill) and [Window Segments Polyfill](https://github.com/zouhir/windowsegments-polyfill). These allow you to test your spanning media queries on a single-screen device, regardless of browser support.

To use the polyfill:

```js
<script src="spanning-css-polyfill.js"></script>

// Simulate dual screen landscape.
const config = window.__foldables_env_vars__;
config.update({
   spanning: "single-fold-vertical",
   foldSize: 30,
   browserShellSize: 20
});
```

We [use this polyfill](/index.html) in the [Daily Prophet Magazine demo](https://devday2020e76c4a5534b64a.z21.web.core.windows.net/) to simulate and test the layout. You can toggle dual screen layout in the demo using the hamburger menu in the top left.