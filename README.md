# Snabbdom TypeStyle

[![npm](https://img.shields.io/npm/v/snabbdom-typestyle.svg)](https://www.npmjs.com/package/snabbdom-typestyle)

Maintainable, scalable, and elegant styling with [TypeStyle](https://github.com/typestyle/typestyle) + [Snabbdom](https://github.com/snabbdom/snabbdom)
* All the power and benefits of TypeStyle
* Internal handling of classname mapping
* Server-side rendering support

## Installation

```bash
npm install snabbdom-typestyle
```

## Usage

Simply pass `css` to your snabbdom virtual node!

```js
  import { Style } from 'snabbdom-typestyle';

  function view() {

      const buttonStyle: Style = {
          color: 'blue'
      };

      return (
          <button css={ buttonStyle }>
              My Button
          </button>
      );
  }
```
The css module is essentially a wrapper around [TypeStyle style](https://typestyle.github.io/#/core/-style-) and accepts the same arguments: Any number of `NestedCssProperties` (or `Style`, which is an alias provided by snabbdom-typestyle).

Make sure to pass the CSS module, along with the Props and Attributes modules, when initializing snabbdom.

```js
  import { init } from 'snabbdom';
  import PropsModule from 'snabbdom/modules/props';
  import AttrsModule from 'snabbdom/modules/attributes';
  import CssModule from 'snabbdom-typestyle';

  const modules = [
    PropsModule,
    AttrsModule,
    CssModule
  ];

  const patch = init(modules);
```

OR, if you are using [Cycle.js](https://github.com/cyclejs/cyclejs) pass `modules` in the options of `makeDOMdriver`.
```js
run(main, { DOM: makeDOMDriver('#root', { modules }) });
```

## Server-side Rendering
To use `snabbdom-typestyle` in a server-side rendered environment, initialize snabbdom with the `serverSideCssModule`.

```js
import { serverSideCssModule, collectStyles } from 'snabbdom-typestyle';
import modulesForHTML from 'snabbdom-to-html/modules';
import { h } from 'snabbdom';

const modules = [
  modulesForHTML.attributes,
  modulesForHTML.props,
  modulesForHTML.class,
  modulesForHTML.style,
  serverSideCssModule
];

const patch = init(modules);
```

When you are rendering your html, you can grab the styles via `collectStyles(node: VNode): String`.

```js
h('style#styles', collectStyles(vtree));
```

Then, on the client-side, pass a selector for the style element rendered by the server to `makeClientSideCssModule(styleElementSelector: string | undefined)`. Doing this avoids duplicating the style element when the application is hydrated.

```
import PropsModule from 'snabbdom/modules/props';
import AttrsModule from 'snabbdom/modules/attributes';
import { makeClientSideCssModule } from 'snabbdom-typestyle';

const modules = [
  PropsModule,
  AttrsModule,
  makeClientSideCssModule('#styles')
];
```

Take a look at the [Cycle.js example here](https://github.com/sklingler93/cyclejs/tree/master/examples/advanced/isomorphic)

## License

MIT
