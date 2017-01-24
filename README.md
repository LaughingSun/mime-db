# mime-db2

This module is a slightly different schema from mime-db and includes a few more build tools.  This module also extracts data from the original mime-db module.

It was formed from https://github.com/jshttp/mime-db.  Much of the source code has remaioned unchanged.  While all of this is very simple, I think much credit must be given to the many fine people on the jshttp projects for their straight foreward and useful implementation of the mimes database and build tools.

[![NPM Version][npm-version-image]][npm-url]
[![NPM Downloads][npm-downloads-image]][npm-url]
[![Node.js Version][node-image]][node-url]
[![Build Status][travis-image]][travis-url]
[![Coverage Status][coveralls-image]][coveralls-url]

This is a database of all mime types.
It consists of a single, public JSON file and does not include any logic,
allowing it to remain as un-opinionated as possible with an API.
It aggregates data from the following sources:

- http://www.iana.org/assignments/media-types/media-types.xhtml
- http://svn.apache.org/repos/asf/httpd/httpd/trunk/docs/conf/mime.types
- http://hg.nginx.org/nginx/raw-file/default/conf/mime.types

## Installation

```bash
npm install mime-db2
```

### Database Download

If you're crazy enough to use this in the browser, you can just grab the
JSON file using [RawGit](https://rawgit.com/). It is recommended to replace
`master` with [a release tag](https://github.com/jshttp/mime-db/tags) as the
JSON format may change in the future.

```
https://cdn.rawgit.com/jshttp/mime-db/master/db.json
```

## Module usage

```js
var db = require('mime-db2');

// grab data on .js files
var data = db['application/javascript'];
```

## Tools cli usage

for an md flavored report

```shell
./mime-db2-tool lookup -i .Bmp
./mime-db2-tool lookup image/*
./mime-db2-tool lookup !image/.+!
```

for a json subset of the mime-db2 (or alternative) json database

```shell
./mime-db2-tool filter -i .Bmp
./mime-db2-tool filter image/*
./mime-db2-tool filter !image/.+!
```

to import data

```shell
./mime-db2-tool import * http://svn.apache.org/repos/asf/httpd/httpd/trunk/docs/conf/mime.types
./mime-db2-tool import * --source=format=apache-mime.types < mime.types
```

to export data

```shell
./mime-db2-tool export image/* --source=format=apache-mime.types > mime.types
```

to update a database

```shell
./mime-db2-tool update ./mime-db.json * http://svn.apache.org/repos/asf/httpd/httpd/trunk/docs/conf/mime.types
./mime-db2-tool import ./mime-db.json * --source=format=apache-mime.types < mime.types
```

for more information on cli usage including `-y` (auto confirm) and `-t` (test run only, no file modification) use `mine-db2 help`.

```shell
./mime-db2-tool help
```


## Data Structure

The JSON file is a map lookup for lowercased mime types.
Each mime type has the following properties:

- `.source` - where the mime type is defined.
    If not set, it's probably a custom media type.
    - `apache` - [Apache common media types](http://svn.apache.org/repos/asf/httpd/httpd/trunk/docs/conf/mime.types)
    - `iana` - [IANA-defined media types](http://www.iana.org/assignments/media-types/media-types.xhtml)
    - `nginx` - [nginx media types](http://hg.nginx.org/nginx/raw-file/default/conf/mime.types)
- `.extensions[]` - known extensions associated with this mime type.
- `.compressible` - whether a file of this type is can be gzipped.
- `.charset` - the default charset associated with this type, if any.

If unknown, every property could be `undefined`.

## Contributing

To edit the database, only make PRs against `src/custom.json` or
`src/custom-suffix.json`.

To update the build, run `npm run update`.

## Adding Custom Media Types

The best way to get new media types included in this library is to register
them with the IANA. The community registration procedure is outlined in
[RFC 6838 section 5](http://tools.ietf.org/html/rfc6838#section-5). Types
registered with the IANA are automatically pulled into this library.

[npm-version-image]: https://img.shields.io/npm/v/mime-db.svg
[npm-downloads-image]: https://img.shields.io/npm/dm/mime-db.svg
[npm-url]: https://npmjs.org/package/mime-db
[travis-image]: https://img.shields.io/travis/jshttp/mime-db/master.svg
[travis-url]: https://travis-ci.org/jshttp/mime-db
[coveralls-image]: https://img.shields.io/coveralls/jshttp/mime-db/master.svg
[coveralls-url]: https://coveralls.io/r/jshttp/mime-db?branch=master
[node-image]: https://img.shields.io/node/v/mime-db.svg
[node-url]: http://nodejs.org/download/
