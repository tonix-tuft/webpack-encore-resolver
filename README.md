[![Latest Stable Version](https://poser.pugx.org/bentools/webpack-encore-resolver/v/stable)](https://packagist.org/packages/bentools/webpack-encore-resolver)
[![License](https://poser.pugx.org/bentools/webpack-encore-resolver/license)](https://packagist.org/packages/bentools/webpack-encore-resolver)
[![Build Status](https://img.shields.io/travis/bpolaszek/webpack-encore-resolver/master.svg?style=flat-square)](https://travis-ci.org/bpolaszek/webpack-encore-resolver)
[![Coverage Status](https://coveralls.io/repos/github/bpolaszek/webpack-encore-resolver/badge.svg?branch=master)](https://coveralls.io/github/bpolaszek/webpack-encore-resolver?branch=master)
[![Quality Score](https://img.shields.io/scrutinizer/g/bpolaszek/webpack-encore-resolver.svg?style=flat-square)](https://scrutinizer-ci.com/g/bpolaszek/webpack-encore-resolver)
[![Total Downloads](https://poser.pugx.org/bentools/webpack-encore-resolver/downloads)](https://packagist.org/packages/bentools/webpack-encore-resolver)


# Webpack Encore Resolver

[Webpack Encore](https://symfony.com/doc/current/frontend.html) can work as a standalone Javascript library with `yarn add @symfony/webpack-encore`. 
However, to dynamically load assets (runtime, vendors, versionned assets, ...), 
you still need Symfony/Twig on the back-end part along with the [webpack-encore-bundle](https://github.com/symfony/webpack-encore-bundle).

So, here is a standalone PHP package to port `asset()`, `encore_entry_js_files()`, `encore_entry_css_files()`, `encore_entry_script_tags()`, `encore_entry_link_tags()` functions 
of [Webpack Encore](https://symfony.com/doc/current/frontend.html) outside of Twig's scope, in a vanilla PHP project.

The goal is to support versioned assets, and automatically load `runtime.js`, vendors etc.

## Installation

```bash
composer require bentools/webpack-encore-resolver
```

## Example Usage

Consider this `webpack.config.js` file:

```js
const Encore = require('@symfony/webpack-encore');

Encore
    .setOutputPath('public/build/')
    .setPublicPath('/build')
    .addEntry('main', './assets/js/main.js')
    .enableVersioning(true)
    // ...
;

module.exports = Encore.getWebpackConfig();
```

You can generate versioned assets tags the following way:

```php
<?php
use function BenTools\WebpackEncoreResolver\encore_entry_link_tags;
use function BenTools\WebpackEncoreResolver\encore_entry_script_tags;

require_once __DIR__ . '/../vendor/autoload.php';
?>
<!doctype html>
<html>
<head>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no">
    <?php encore_entry_link_tags('main');?>
    <?php encore_entry_script_tags('main');?>
    <!-- ... -->
```

## Caveats

Multiple webpack configurations aren't supported at the moment. PRs welcome!
 

## Tests

```bash
./vendor/bin/phpunit
```

## License

MIT.

