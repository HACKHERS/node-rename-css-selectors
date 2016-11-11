# Rename CSS Selectors (RCS)

[![Build Status](https://travis-ci.org/JPeer264/node-rename-css-selectors.svg?branch=master)](https://travis-ci.org/JPeer264/node-rename-css-selectors)
[![Coverage Status](https://coveralls.io/repos/github/JPeer264/rename-css-selectors/badge.svg)](https://coveralls.io/github/JPeer264/rename-css-selectors)

This module renames all CSS selectors in the given files. It will collect all selectors from the given CSS files. Do not worry about your selectors, `rcs` will do it for you.

**NOT YET IMPLEMENTED** You can also use a config file, if you already had other projects with the same classes. So all your projects have the same minified selector names - always.

## Contents

- [Installation](#installation)
- [Usage](#usage)
- [RCS Config](#rcs-config)
- [Methods](#methods)
- [LICENSE](#license)

## Installation

Install with [npm](https://docs.npmjs.com/cli/install) or [yarn](https://yarnpkg.com/en/docs/install)

```js
npm install --save rename-css-selectors
```
or
```js
yarn add rename-css-selectors
```

## Usage

```js
const rcs = require('rename-css-selectors')

// first you have to process the css files
// to get a list of variables you want to minify
// options is optional
rcs.processCss('**/*.css', options, err => {
    // all css files are now saved, renamed and stored in the selectorLibrary

    // now it is time to process all other files
    rcs.process('**/*.js', options, err => {
        // that's it
    });
});
```

## RCS config

> Just create a `.rcsrc` in your project root or you can add everything in your `package.json` with the value `rcs`

- [Example](#example)
- [Exclude](#exclude-classes-and-ids)
- [Include from other projects](#include-renamed-classes-from-other-project)

### Example

The `.rcsrc` or the a own config file:

```json
{
    "exclude": [
        "js",
        "flexbox"
    ]
}
```

The `package.json`:

```json
{
    "name": "Your application name",
    "rcs": {
        "exclude": [
            "js",
            "flexbox"
        ]
    }
}
```

### Exclude Classes and IDs

`exclude`

What if you are using something such as Modernizr and you do not want to minify some selectors?

Let's exclude 4 classes and id's: `js`, `flexbox`, `canvas` and `svg`

```json
{
    "exclude": [
        "js",
        "flexbox",
        "canvas",
        "svg"
    ]
}
```

### Include renamed classes from other project

**todo**

## Methods

- [processCss](#processcss)
- [process](#process)
- [generateLibraryFile](#generatelibraryfile)
- [includeConfig](#includeconfig)

### processCss

**processCss(src, [options], callback)**

Store all matched selectors into the library and saves the new generated file with all renamed selectors.

Options:

- overwrite (boolean): ensures that it does not overwrite the same file accidently. Default is `false`
- cwd (string): the working directory in which to seach. Default is `process.cwd()`
- newPath (string): in which folder the new files should go. Default is `rcs`
- flatten (boolean): flatten the hierarchie - no subfolders. Default is `false`

Example:

```js
const rcs = require('rename-css-selectors')

rcs.processCss('**/*.css', options, err => {
    if (err) return console.error(err)

    console.log('Successfully wrote new files and stored values');
}
```

### process

**process(src, [options], callback)**

> **Important!** processCss should run first, otherwise there are no minified selectors

> *Note:* If you replace JavaScript files, you might overwrite words within a string which is not supposed to be a css selector

Matches all strings `" "` or `' '` and replaces all matching words which are the same as the stored css values.

Options:

- overwrite (boolean): ensures that it does not overwrite the same file accidently. Default is `false`
- cwd (string): the working directory in which to seach. Default is `process.cwd()`
- newPath (string): in which folder the new files should go. Default is `rcs`
- flatten (boolean): flatten the hierarchie - no subfolders. Default is `false`

Example:

```js
const rcs = require('rename-css-selectors')

// the same for html or other files
rcs.process('**/*.js', options, err => {
    if (err) return console.error(err)

    console.log('Successfully wrote new files');
}
```

### generateLibraryFile

**generateLibraryFile(pathLocation, [options], callback)**

> *Note:* if you are using the options either cssMapping or cssMappingMin must be set to true

Generates mapping files: all minified, all original selectors or both. They are stored as object in a variable. The file is named as `renaming_map.js` or `renaming_map_min.js`.

Options:

- cssMapping (boolean): writes `renaming_map.js`. Default is `true`
- cssMappingMin (boolean): writes `renaming_map_min.js`. Default is `false`
- extended (boolean): instead of a string it writes an object with meta information. Default is `false`
- **NOT YET IMPLEMENTED** json (boolean): writes an json instead of a js. Default is `false`

```js
const rcs = require('rename-css-selectors')

// the same for html or other files
rcs.generateLibraryFile('./mappings', options, err => {
    if (err) return console.error(err)

    console.log('Successfully wrote mapping files');
}
```

Output in `renaming_map_min.js`:

```js
var CSS_NAME_MAPPING_MIN = {
    'e': 'any-class',
    't': 'another-class'
};
```

### includeConfig

**includeConfig([pathLocation])**

> All available configs [here](#rcs-config)

RCS will lookup first for a `.rcsrc` of the current directory. If there is no such file, it will lookup for a `package.json` with a `"rcsrc": {}` in it. You can also write any path in the parameters and write your own config file. This function is synchronous.

```js
const rcs = require('rename-css-selectors')

rcs.includeConfig()
```

# LICENSE

MIT © [Jan Peer Stöcklmair](https://www.jpeer.at)