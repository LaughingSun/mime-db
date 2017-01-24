# mime-db2

This module is very similar to  mime-db (from which it was forked) with a slightly different schema (see the Data Structure section below) and includes command line tools.  This module also extracts data from the original mime-db module.

It was grown from https://github.com/jshttp/mime-db.  Much of the source code has remained unchanged except for a few renaming of and additional keys.  While all of this is very simple, I think much credit must be given to the many fine people on the jshttp projects for their straight foreward and useful implementation of the mimes database and build tools.

[![NPM Version][npm-version-image]][npm-url]
[![NPM Downloads][npm-downloads-image]][npm-url]
[![Node.js Version][node-image]][node-url]
[![Build Status][travis-image]][travis-url]
[![Coverage Status][coveralls-image]][coveralls-url]

This is a database of all mime types.
It consists of a single, public JSON file and does not include any logic,
allowing it to remain as un-opinionated as possible with an API.
It aggregates data from the following sources:

- https://github.com/jshttp/mime-db/raw/master/db.json
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

for an md flavored analysis

```shell
./mime-db2 analyze image.bmp
```


for an json flavored analysis

```shell
./mime-db2 analyze --json image.bmp
```

for an json flavored report on a specific file extension or mime-type

```shell
./mime-db2 lookup --json image.bmp
./mime-db2 lookup --json -i .Bmp
./mime-db2 lookup --json image/*
./mime-db2 lookup --json !image/.+!
```

for an md flavored report on a specific file extension or mime-type

```shell
./mime-db2 lookup image.bmp
./mime-db2 lookup -i .Bmp
./mime-db2 lookup image/*
./mime-db2 lookup !image/.+!
```

for a json subset of the mime-db2 (or alternative) json database

```shell
./mime-db2 filter --json -i .Bmp
./mime-db2 filter --json image/*
./mime-db2 filter --json !image/.+!
```

to import data

```shell
./mime-db2 import * http://svn.apache.org/repos/asf/httpd/httpd/trunk/docs/conf/mime.types
./mime-db2 import * --source=format=apache-mime.types < mime.types
```

to export data

```shell
./mime-db2 export image/* --source=format=apache-mime.types > mime.types
```

to update a database

```shell
./mime-db2 update ./mime-db.json * http://svn.apache.org/repos/asf/httpd/httpd/trunk/docs/conf/mime.types
./mime-db2 import ./mime-db.json * --source=format=apache-mime.types < mime.types
```

for more information on cli usage including `-y` (auto confirm) and `-t` (test run only, no file modification) use `mine-db2 help`.

```shell
./mime-db2 help
```


## Data Structure

The JSON file is a map lookup for lowercased mime types.
Each mime type has the following properties:

- `.source`         - who defines the mime type.
- `.authorities[]   - who are the authorities for the source, see authorities notes.
- `.sourceUrl`      - the url of the source mime type file.
- `.sourceFormat`   - the file format for the sourceUrl file
    If not set, it's probably a custom media type.
    - `apache`      - [Apache common media types](http://svn.apache.org/repos/asf/httpd/httpd/trunk/docs/conf/mime.types)
    - `iana`        - [IANA-defined media types](http://www.iana.org/assignments/media-types/media-types.xhtml)
    - `nginx`       - [nginx media types](http://hg.nginx.org/nginx/raw-file/default/conf/mime.types)
- `.compressible`   - whether a file of this type is can be gzipped.
- `.defaultExtension` - known extensions associated with this mime type, if any.
- `.extensions[]`   - known extensions associated with this mime type, if any.
- `.defaultCharset` - the default charset associated with this type, if any.
- `.charsets[]`     - the charsets associated with this type, if any.
- `.external[]`     - associated files or urls, if any..

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

[npm-version-image]: https://img.shields.io/npm/v/mime-db2.svg
[npm-downloads-image]: https://img.shields.io/npm/dm/mime-db2.svg
[npm-url]: https://npmjs.org/package/mime-db2
[travis-image]: https://img.shields.io/travis/LaughingSun/mime-db2/master.svg
[travis-url]: https://travis-ci.org/LaughingSun/mime-db2
[coveralls-image]: https://img.shields.io/coveralls/LaughingSun/mime-db2/master.svg
[coveralls-url]: https://coveralls.io/r/LaughingSun/mime-db2?branch=master
[node-image]: https://img.shields.io/node/v/mime-db2.svg
[node-url]: http://nodejs.org/download/
