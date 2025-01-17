# requirejs-vue

![ECMAScript 5.1](https://img.shields.io/badge/es-5-brightgreen.svg)
![Github file size](https://img.shields.io/github/size/vikseriq/requirejs-vue/requirejs-vue.js.svg)

A Vue.js [Single File Components](https://vuejs.org/v2/guide/single-file-components.html) loader plugin for
[RequireJS](https://github.com/requirejs/requirejs).
One-file, browser or server, no additional fats, es5+ compatible, no babels, gluten-free.

## Demo on [Plunker](https://plnkr.co/edit/Y2cEa3)

On browser-side it uses XMLHttpRequest to fetch the .vue files, so you can only fetch files
that are on the *same domain as the html page*. Most browsers place restrictions on using
XMLHttpRequest from local file URLs, so use a web server to serve your ```.vue``` files.

The server-side building with RequireJS Optimizer [r.js](https://github.com/requirejs/r.js) also available and works well.

Since another templating engines or css preprocessors (like [less](https://github.com/guybedford/require-less)/sass)
not available as maintained AMD modules there are no plans to support them. Feel free to write your own 😉

## Install <a name="install"></a>

### Via package manager

To install with [yarn](https://github.com/yarnpkg/yarn)

```
yarn add requirejs-vue
```

Or good ol' ```npm```:
```
npm install --save requirejs-vue
```

And [Bower](http://bower.io/):

```
  bower install requirejs-vue
```

### Manual download

Download the [latest version](https://rawgit.com/vikseriq/requirejs-vue/master/requirejs-vue.js).

## Usage <a name="usage"></a>

Setup loader in RequireJS path like this:

    ...
        paths: {
            'Vue': 'path to Vue.js',
            'vue': 'path to requirejs-vue'
        },
        config: {
            'vue': {
                'css': 'inject'
                'templateVar': 'template'
            }
        }
    ...


Reference Vue file via the ```vue!```.
For example, to load the `component.vue` file that is in your ```baseUrl``` directory:

    require(['vue!component'], function (component) {
		//
    });

You can specify any alias for loader but update paths alias too.


### Configuration

Inside loaded file reference template by `templateVar` variable.
Stylesheets controlled by `css` configuration value. Currently only global styles supported.
Content of template will be cleared from whitespaces and comments. Nested `template` tags supported.

Loader support ```.vue``` and ```.html``` files.


#### `css`
`String | Function`, default: `inject`

Strategy to deal with component stylesheets. Variants:
	* `inject` - appending styles in new `<style>` tag to `document.body`
	* `skip` - do not process component css. Default action for r.js build mode.
	* `fn(style, option{name})` - callback function for css aggregation. `style` contains actual component
		style content, `option.name` hold component file reference


#### `templateVar`
`String`, default: `template`

Represent component template as `templateVar` variable in script closure.


### Basic usage
Sample .vue file supported by loader:

```vue
<template>
    <div v-cloak>Vue-demo component root</div>
</template>
<style>
    /* global styles */
    [v-cloak] {
        display: none;
    }
</style>
<script>
define(['Vue'], function(vue){
  Vue.component('vue-demo', {
    template: template
  })
});
</script>
```

### Server-side building with r.js

Plugin supports RequireJS Optimizer. Follow the usual r.js workflow: specify build config in ```build.js``` like:

```js
{
	baseUrl: './',
	name: 'init',
	out: './dist.js',
	findNestedDependencies: true,
	optimize: 'none',
	paths: {
	  Vue: 'node_modules/vue/dist/vue.min',
	  vue: 'node_modules/requirejs-vue/requirejs-vue',
	  app: 'your-entry-point'
}
  }
```

and then run builder:

```
r.js -o build.js
```

For advanced usage see [demo project](/demo/).

## License

MIT &copy; 2017 vikseriq