# css-mqpacker-starter

### Mixin starter with scss for [css-mqpacker](https://www.npmjs.com/package/css-mqpacker)

You must use the package with scss and postcss.
You will find the webpack rule for compiling scss files with css-mqpacker below or use [webpack starter](https://github.com/MichaelGitArt/webpack-starter-gitart).

[Installation](#installation) <br>
[Breakpoints](#Breakpoints) <br>
[Init order](#init-order) <br>
[Mixins](#mixins) <br>
[Source mixin code](#source-mixin-code) <br>
[Possible order error](#possible-order-error) <br>
[Webpack rule for compiling](#webpack-rule-for-compiling) <br>

## Installation

Install the package

```shell script
npm i css-mqpacker-starter
```

Import in main scss file

```scss
@import "~css-mqpacker-starter";
@include initMediaPosition();
```

Now use it :)

```scss
@include sm {
  body {
    font-size: 15px;
  }
}
```

## Breakpoints

The package provide default breakpoints:

| Breakpoint | Pixels |
| ---------- | ------ |
| \$xl       | 1200px |
| \$lg       | 992px  |
| \$md       | 768px  |
| \$sm       | 576px  |
| \$xs       | 420px  |
| \$xxs      | 360px  |

## Mixins

There are media queries mixins for each breakpoint. Desktop First and Mobile First.

```scss
@include xl {
}
@include xl-up {
}
```

| down | up     |
| ---- | ------ |
| xl   | xl-up  |
| lg   | lg-up  |
| md   | md-up  |
| sm   | sm-up  |
| xs   | xs-up  |
| xxs  | xxs-up |

## Init order:

Put the mixin before custom scss code. It will create placeholder styles
for body tag. It's important for css-mqpacker; There can be wrong order media queries without the mixin.

```scss
@include initMediaPosition();
```

Possible error see [below](#possible-order-error)

### Source mixin code:

```scss
@mixin xl {
  @media only screen and (max-width: $xl - 1) {
    @content;
  }
}

@mixin xl-up {
  @media only screen and (min-width: $xl) {
    @content;
  }
}
```

#### Possible order error

In the case:

```scss
.card {
  // styles
  @include sm {
    // styles 2
  }
}

.container {
  max-width: 1200px;
  @include md {
    max-width: 500px;
  }
  @include sm {
    max-width: none;
  }
}
```

Output after css-mqpacker will be:

```scss
.card {
  // styles
}
.container {
  max-width: 1200px;
}

@media only screen and (min-width: $sm) {
  .card {
    // styles 2
  }
  .container {
    max-width: none;
  }
}
@media only screen and (min-width: $md) {
  .container {
    max-width: 500px;
  }
}
```

On screen \$sm and below style `.container{ max-width: none; }` will not be applied.
Use `@include initMediaPosition()` to prevent this error.

### Webpack rule for compiling

Here is example: [Webpack starter](https://github.com/MichaelGitArt/webpack-starter-gitart)

```js
// rules:
[
  {
    test: /\.scss$/,
    use: [
      isDev ? "style-loader" : MiniCssExtractPlugin.loader,
      {
        loader: "css-loader",
        options: { sourceMap: true },
      },
      {
        loader: "postcss-loader",
        options: {
          sourceMap: true,
          config: { path: "postcss.config.js" },
        },
      },
      {
        loader: "sass-loader",
        options: { sourceMap: true },
      },
    ],
  },
];
```
