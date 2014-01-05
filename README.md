![status](https://secure.travis-ci.org/gulpjs/gulp-util.png?branch=master)

## Information

<table>
<tr> 
<td>Package</td><td>gulp-util</td>
</tr>
<tr>
<td>Description</td>
<td>Utility functions for gulp plugins</td>
</tr>
<tr>
<td>Node Version</td>
<td>>= 0.9</td>
</tr>
</table>

## Usage

```javascript
var gutil = require('gulp-util');

gutil.log('stuff happened', 'Really it did', gutil.colors.cyan('123'));
gutil.beep();

gutil.replaceExtension('file.coffee', '.js'); // file.js

var opt = {
  name: 'todd',
  file: someGulpFile
};
gutil.template('test <%= name %> <%= file.path %>', opt) // test todd /js/hi.js
```

### log(msg...)

Logs stuff. Already prefixed with [gulp] and all that. Use the right colors for values. If you pass in multiple arguments it will join them by a space.

```
values (files, module names, etc.) = magenta
numbers (times, counts, etc) = cyan
```

### colors

This is an instance of [chalk](https://github.com/sindresorhus/chalk)

### date

This is an instance of [node-dateformat](https://github.com/felixge/node-dateformat).

### linefeed

Just `\n`. Use this.

### replaceExtension(path, newExtension)

Replaces a file extension in a path. Returns the new path.

### isStream(obj)

Returns true or false if an object is a stream.

### isBuffer(obj)

Returns true or false if an object is a Buffer.

### template(string[, data])

This is a lodash.template function wrapper. You must pass in a valid gulp file object so it is available to the user or it will error. You can not configure any of the delimiters. Look at the [lodash docs](http://lodash.com/docs#template) for more info.

## new File(obj)

This is just [vinyl](https://github.com/wearefractal/vinyl)

```javascript
var file = new gulp.File({
  base: join(__dirname, './fixtures/'),
  cwd: __dirname,
  path: join(__dirname, './fixtures/test.coffee')
});
```

## noop()

Returns a stream that does nothing but pass data straight through.

```javascript
gulp.task('scripts', function() {
  gulp.src('src/**/*.js')
    .pipe(concat('script.js'))
    .pipe(gulp.env.production ? uglify() : gutil.noop())
    .pipe(gulp.dest('dist/');
});
```

## combine(streams...)

Argument may also be an array of streams. Returns a function that returns the combined stream. Useful for keeping things DRY. This is a simple wrapper around event-stream's `.pipeline`

```javascript
var scripts = gutil.combine(
    coffee({bare: true}),
    uglify(),
    concat('script.js'),
    gulp.dest('dist/')
);

gulp.task('default', function() {
  gulp.src('*.coffee').pipe(scripts());
});
```

## buffer(cb)

This is similar to es.wait but instead of buffering text into one string it buffers anything into an array (so very useful for file objects).

Returns a stream that can be piped to.

The stream will emit one data event after the stream piped to it has ended. The data will be the same array passed to the callback.

Callback is optional and receives two arguments: error and data

```javascript
gulp.src('stuff/*.js')
  .pipe(gutil.buffer(function(err, files){
  
  });
```

## new PluginError(pluginName, message[, options])

- pluginName should be the module name of your plugin
- message can be a string or an existing error
- By default the stack will not be shown. Set `options.showStack` to true if you think the stack is important for your error.
- If you pass an error in as the message the stack will be pulled from that, otherwise one will be created.

These are all acceptable forms of instantiation:

```javascript
var err = new gutil.PluginError('test', {
  message: 'something broke'
});

var err = new util.PluginError({
  plugin: 'test',
  message: 'something broke'
});

var err = new util.PluginError('test', 'something broke');

var err = new util.PluginError('test', 'something broke', {showStack: true});

var existingError = new Error('OMG');
var err = new util.PluginError('test', existingError, {showStack: true});
```

## LICENSE

(MIT License)

Copyright (c) 2013 Fractal <contact@wearefractal.com>

Permission is hereby granted, free of charge, to any person obtaining
a copy of this software and associated documentation files (the
'Software'), to deal in the Software without restriction, including
without limitation the rights to use, copy, modify, merge, publish,
distribute, sublicense, and/or sell copies of the Software, and to
permit persons to whom the Software is furnished to do so, subject to
the following conditions:

The above copyright notice and this permission notice shall be
included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED 'AS IS', WITHOUT WARRANTY OF ANY KIND,
EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF
MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND
NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE
LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION
OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION
WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.
