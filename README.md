# plugin-scss

[![Build Status](https://api.travis-ci.org/KevCJones/plugin-scss.svg?branch=master)](https://travis-ci.org/KevCJones/plugin-scss)
[![devDependency Status](https://david-dm.org/KevCJones/plugin-scss/dev-status.svg)](https://david-dm.org/KevCJones/plugin-scss#info=devDependencies)

![At a Glance](assets/img/ataglance.jpg)

Originally forked from [plugin-sass](https://github.com/mobilexag/plugin-sass)

[SystemJS](https://github.com/systemjs/systemjs)
[SCSS](http://sass-lang.com) loader plugin. Can easily be installed with
[jspm](http://jspm.io) package manager.

```sh
$ jspm install scss
```

The decision that it was unlikely that plugin-sass team would ever merge our changes back into the plugin-sass. They have a fundamental differece in use. Plugin-sass is cleaner if all you want to do is load your styles into the head block. However we had a very different use case.

We wanted to transpile our .scss content and return the css as a string so that we could inject it where ever we please. In particular, we wanted to inject into Angular 2's styles decorator.

Using it looks like this in its various forms :

```js
System.import('./style.scss!').then(function(css){
  //inject css in head?
  //pass css into angular2 @component styles tag [up to you]
});
```

or synchronously

```js
import styles from './style.scss!';
//styles will contain your css ready again for what you need
```

You can also use the [older syntax](http://sass-lang.com/documentation/file.SASS_REFERENCE.html#syntax)
, known as the indented syntax (or sometimes just "_Sass_")

```js
System.import('./style.sass!scss').then(function(css){
  //inject your css anywhere you need
});
```

## Compile Targets

### Style

By default, `plugin-scss` does NOT inject compiled css into a `<style>` tag in the `<head>`. It instead returns your css as a string. Use `plugin-sass` if you need this behaviour.

### Strings for angular 2

As mentioned above - `plugin-scss` was a modification of the original plugin specifically because we needed a string that was for use in the @component decorator. It was different
enough to feel like a new plugin.

```js
import styles from './style.scss!';

@Component({
    template: '...',
    styles: [styles]
})
```

### SASS options

 To add SASS [options](https://github.com/medialize/sass.js/#using-the-sassjs-api)

 ```js
 sassPluginOptions: {
   "sassOptions": {

   }
 }
 ```


## Importing from jspm

You can import scss files from jspm packages *from within scss files* using the `jspm:` prefix. For example, if you have jspm installed `twbs/bootstrap-sass`:

```scss
@import 'jspm:bootstrap-sass/assets/stylesheets/bootstrap';
```

## Bundling using JSPM/SystemJS

We've work on the bundle support, this was where most of the work was done when extending from `plugin-sass`. Fundamentally, we didn't need to write into the bundle in a special way to inject the css into head. So instead, we used the hooks for translate and fetch to work very similarly to the normal 'inject' route in browser.

## Testing the plugin

```sh
$ npm install -g gulp
...
$ npm install
...
$ jspm install
```

Now you can test runtime compilation

```sh
$ gulp test:runtime
```

bundling

```sh
$ gulp test:bundle
```

or static bundling

```sh
$ gulp test:bundleStatic
```

After that open [http://localhost:3000](http://localhost:3000) in the browser
of your choice.
