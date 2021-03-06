# PostCSS JS [![Build Status][ci-img]][ci]

<img align="right" width="95" height="95"
     title="Philosopher’s stone, logo of PostCSS"
     src="http://postcss.github.io/postcss/logo.svg">

[PostCSS] for React Inline Styles, Radium, [Free Style] and other CSS-in-JS.

For example, to use [Autoprefixer], [RTLCSS] or [postcss-write-svg] plugins
in your workflow.

[postcss-write-svg]: https://github.com/jonathantneal/postcss-write-svg
[Autoprefixer]:      https://github.com/postcss/autoprefixer
[Free Style]:        https://github.com/blakeembrey/free-style
[PostCSS]:           https://github.com/postcss/postcss
[RTLCSS]:            https://github.com/jonathantneal/postcss-write-svg
[ci-img]:            https://travis-ci.org/postcss/postcss-js.svg
[ci]:                https://travis-ci.org/postcss/postcss-js

## Usage

### Processing

```js
let prefixer = postcssJs.sync([ autoprefixer ]);

let style = prefixer({
    display: 'flex'
});

style //=> { display: ['-webkit-box', '-webkit-flex', '-ms-flexbox', 'flex'] }
```

### Compile CSS-in-JS to CSS

```js
let style = {
    top: 10,
    '&:hover': {
        top: 5
    }
};

postcss().process(style, { parser: postcssJs }).then( (result) => {
    result.css //=> top: 10px;
               //   &:hover { top: 5px; }
})
```

### Compile CSS to CSS-in-JS

```js
let css  = '@media screen { z-index: 1 }'
let root = postcss.parse(css);

postcssJs.objectify(root) //=> { '@media screen': { zIndex: '1' } }
```

## API

### `sync(plugins): function`

Create PostCSS processor with simple API, but with only sync PostCSS plugins
support.

Processor is just a function, which takes one style object and return other.

### `async(plugins): function`

Same as `sync`, but also support async plugins.

Returned processor will return Promise.

### `parse(obj): Root`

Parse CSS-in-JS style object to PostCSS `Root` instance.

It converts numbers to pixels and parses
[Free Style] like selectors and at-rules:

```js
{
    '@media screen': {
        '&:hover': {
            top: 10
        }
    }
}
```

This methods use Custom Syntax name convention, so you can use it like this:

```js
postcss().process(obj, { parser: postcssJs })
```

### `objectify(root): object`

Convert PostCSS `Root` instance to CSS-in-JS style object.
