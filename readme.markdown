# hyperstream

stream html into html at a css selector

[![build status](https://secure.travis-ci.org/substack/hyperstream.png)](http://travis-ci.org/substack/hyperstream)

# example

``` js
var hyperstream = require('hyperstream');
var fs = require('fs');

var hs = hyperstream({
    '#a': fs.createReadStream(__dirname + '/a.html'),
    '#b': fs.createReadStream(__dirname + '/b.html')
});
var rs = fs.createReadStream(__dirname + '/index.html');
rs.pipe(hs).pipe(process.stdout);
```

```
$ node example/hs.js
<html>
  <body>
    <div id="a"><h1>a!!!</h1></div>
    <div id="b"><b>bbbbbbbbbbbbbbbbbbbbbb</b></div>
  </body>
</html>
```

# methods

``` js
var hyperstream = require('hyperstream')
```

## var hs = hyperstream(streamMap)

Return a duplex stream that takes an html stream as input and produces an html
stream as output, inserting the streams given by `streamMap` at the css selector
keys.

If `streamMap` values are strings or functions, update the contents at the css
selector key with their contents directly without using a stream.

If `streamMap` values are non-stream objects, iterate over the keys and set
attributes for each key. If you use a special `_html` key with a stream value,
you can set attributes and stream contents for the same element:

These attributes are special. Each attribute can be a string, buffer, or stream:

_html - set the inner content as raw html

_text - set the inner content as text encoded as html entities

_append, _appendText - add text to the end of the inner content encoded as html entities

_appendHtml - add raw html to the end of the inner content

_prepend, _prependText - add text to the beginning of the inner content encoded as html entities

_prependHtml - add raw html to the beginning of the inner context

``` js
hyperstream({
    '#content': {
        _html: stream,
        'data-start': 'cats!',
        'data-end': 'cats!\ufff'
    }
})
```

## hs.select(), hs.update(), hs.replace(), hs.remove()

Proxy through methods to the underlying
[trumpet](https://github.com/substack/node-trumpet) instance.

# install

With [npm](https://npmjs.org) do:

```
npm install hyperstream
```

# license

MIT
