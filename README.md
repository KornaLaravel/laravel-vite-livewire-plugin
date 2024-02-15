# Vite Livewire Plugin

<a href="https://www.npmjs.com/package/@defstudio/vite-livewire-plugin"><img src="https://img.shields.io/npm/dt/@defstudio/vite-livewire-plugin" alt="Total Downloads"></a>
<a href="https://www.npmjs.com/package/@defstudio/vite-livewire-plugin"><img src="https://img.shields.io/npm/v/@defstudio/vite-livewire-plugin" alt="Latest Stable Version"></a>
<a href="https://www.npmjs.com/package/@defstudio/vite-livewire-plugin"><img src="https://img.shields.io/npm/l/@defstudio/vite-livewire-plugin" alt="License"></a>
<a href="https://twitter.com/FabioIvona?ref_src=twsrc%5Etfw"><img alt="Twitter Follow" src="https://img.shields.io/twitter/follow/FabioIvona?label=Follow&style=social"></a>


Vite has become Laravel's default frontend build tool.

This plugin (made by [def:studio](https://twitter.com/FabioIvona)) configures Vite for use with a Livewire setup, allowing components hot reload without losing their state when their blade or class files change.

Demo video available [below](#demo) in this page

## Installation

```bash
npm install --save-dev @defstudio/vite-livewire-plugin
```

## Usage

Livewire hot reload can be enabled by adding the `livewire()` plugin to the `vite.config.js` file:

```js
import {defineConfig} from 'vite';
import laravel from 'laravel-vite-plugin';

import livewire from '@defstudio/vite-livewire-plugin'; // <-- import

export default defineConfig({
    //...
    
    plugins: [
        laravel([
            input: [
                'resources/css/app.css',
                'resources/js/app.js',
            ],
            refresh: false, // <-- disables laravel autorefresh, to avoid conflicts
        ]),
        
        livewire({  // <-- add livewire plugin
            refresh: ['resources/css/app.css'],  // <-- will refresh css (tailwind ) as well
        }),
    ],
});
```

and add the livewire hot reload manager in your `app.js` file:

```js
//..

import {livewire_hot_reload} from 'virtual:livewire-hot-reload'

livewire_hot_reload();
```

After the Vite server is started, you should see the init message on your browser console:

```
[vite] connecting...
[vite] livewire hot reload ready.
[vite] connected.
```

From now on, when a `.blade.php` or Livewire `.php` class is updated, the hot reload will trigger a refresh for all Livewire components in page (and the app.css file as well):

```
[vite] css hot updated: /resources/css/app.css
[vite] livewire hot updated.
```

> **Warning**
> This Vite plugin, as Livewire needs to persist in page, is not fully compatible with other plugins that full refresh the page when a `.blade.php` file changes (i.e. laravel/vite-plugin with refresh option active)
> in order to make them work together, `blade` files in `**/livewire/**` should be excluded from blade hot reload. For Laravel Vite plugin, this config would solve the issue:
 
```js
// vite.config.js
export default defineConfig({
//...
    plugins: [
        //...
    
        laravel({
            // ...
            refresh: false,
        })
    ],
});
```

### Watching files for hot reload trigger

by default `livewire()` plugin will trigger hot reload when a `.blade.php` file changes in `resources/views/**` folders or a  `.php` file changes in `app/**/Livewire/**`, `app/**/Filament/**` or `app/View/Components/**` folders.

if you wish to add/change this behavior (because you have livewire files in other locations), this can be achieved using the `watch` config:

```js
// vite.config.js 

import livewire, {defaultWatches} from '@defstudio/vite-livewire-plugin';

export default defineConfig({
    //...
    
    plugins: [
        //...
        
        livewire({
            refresh: ['resources/css/app.css'],
            watch: [
                ...defaultWatches,
                '**/domains/**/Livewire/**/*.php',
            ]
        }),
    ],
});
```

### Opt in hot reload

In some cases (i.e. when working on non-livewire elements), you'll want to full reload the page when a blade file is changed.

By adding an `VITE_LIVEWIRE_OPT_IN=true` entry in your `.env` file an opt-in checkbox will show on the bottom right corner of the webpage, allowing you to enable/disable livewire hot reload. If disabled: a full page reload will be triggered when blade files are changed.

If you're using a debug toolbar like [Debugbar for Laravel](https://github.com/barryvdh/laravel-debugbar), [Clockwork](https://underground.works/clockwork/) or [Laravel Telescope Toolbar](https://github.com/fruitcake/laravel-telescope-toolbar), these are taken into account, while positioning the checkbox. No special configuration is needed.
There may be another (debug) toolbar or something else in your view interfere with the checkbox. Therefore, a config option `bottomPosition` is available to add more spacing to the bottom, to make the checkbox visible again. The value must be of type number and is considered to be pixels.

```js
// vite.config.js 

import livewire, {defaultWatches} from '@defstudio/vite-livewire-plugin';

export default defineConfig({
    //...
    
    plugins: [
        //...
        
        livewire({
            bottomPosition: 34,
        }),
    ],
});
```

## Changelog

Please see [CHANGELOG](CHANGELOG.md) for more information on what has changed recently. [Follow Us](https://twitter.com/FabioIvona) on Twitter for more updates about this package.

## Upgrade

Please see [UPGRADE GUIDE](UPGRADE.md) for more information on upgrading between versions

## Contributing

Please see [CONTRIBUTING](.github/CONTRIBUTING.md) for details.

## Security Vulnerabilities

Please review [our security policy](../../security/policy) on how to report security vulnerabilities.

## Credits

- [Fabio Ivona](https://github.com/def-studio)
- [All Contributors](../../contributors)

## License

The MIT License (MIT). Please see [License File](LICENSE.md) for more information.

## Demo

https://user-images.githubusercontent.com/8792274/176996785-d4e21ee5-b9f3-4e91-bf3a-4bbc0e87a148.mp4
