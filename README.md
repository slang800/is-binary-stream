isBinaryFile
============

Detects if a file is binary in Node.js. Similar to [Perl's `-B` switch](http://stackoverflow.com/questions/899206/how-does-perl-know-a-file-is-binary),
in that:

* it reads the first few thousand bytes of a file
* checks for a `null` byte; if it's found, it's binary
* flags non-ASCII characters. After a certain number of "weird" characters, the
file is flagged as binary

All the logic is also pretty much ported from
[ag](https://github.com/ggreer/the_silver_searcher).

Note: if the file doesn't exist or it is empty, this function returns `false`.

## Installation

```
npm install isbinaryfile
```

## Usage

If you pass in one argument, this module assumes it's just the file path, and
performs the appropriate file read and stat functionality internally, as sync
options:

``` javascript
var isBinaryFile = require("isbinaryfile");

if (isBinaryFile(process.argv[2]))
  console.log("It is!")
else
  console.log("No.")
```

Ta da.

However, if you've already read and `stat()`-ed a file (for some other reason),
you can pass in both the file's raw data and the stat's `size` info to save
time:

```javascript
fs.readFile(process.argv[2], function(err, data) {
  fs.lstat(process.argv[2], function(err, stat) {
    if (isBinaryFile(data, stat.size))
      console.log("It is!")
    else
      console.log("No.")
  });
});
```

### Async

Previous to version 2.0.0, this program always ran in sync mode. Now, there's
an async option. Simply pass a function as your second parameter, and `isBinaryFile`
will figure the rest out:

``` javascript
var isBinaryFile = require("isbinaryfile");

isBinaryFile(process.argv[2], function(err, result) {
  if (err) return console.error(err);

  if (result)
    console.log("It is!")
  else
    console.log("No.")
}
```

## Testing

Install mocha on your machine:

```
npm install mocha -g
```

Then run `npm test`.
