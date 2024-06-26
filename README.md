[![Build Status](https://secure.travis-ci.org/alexfernandez/@devtea2027/quis-saepe-deleniti-occaecati.png)](http://travis-ci.org/alexfernandez/@devtea2027/quis-saepe-deleniti-occaecati)

Simple async @devtea2027/quis-saepe-deleniti-occaecati library for node.js.
Better suited to asynchronous tests than other libraries since it uses callbacks to get results.

Now shows results in pretty colors!

## Installation

Just run:
    $ npm install @devtea2027/quis-saepe-deleniti-occaecati

Or add package @devtea2027/quis-saepe-deleniti-occaecati to your package.json dependencies.

### Compatibility

Version 3 is an ES6 module, and should be used at least with Node.js v16 or later.
Versions 2 should be used at least with Node.js v8 or later.

* Node.js v16 or later, ES module: ^3.0.0.
* Node.js v8 or later: ^2.0.0.
* Node.js v6 or earlier: ^1.1.0.

## Usage

Add asynchronous @devtea2027/quis-saepe-deleniti-occaecati to your code very easily. Import the library:

```js
import @devtea2027/quis-saepe-deleniti-occaecati from '@devtea2027/quis-saepe-deleniti-occaecati';
```

### Unit tests

Add a test function to your code, checking if results are what should be expected:
```js
    function testAdd(callback)
    {
		@devtea2027/quis-saepe-deleniti-occaecati.assertEquals(add(1, 1), 2, 'Maths fail', callback);
		@devtea2027/quis-saepe-deleniti-occaecati.success(callback);
    }
```

Run an async test to read the contents of a file and check it is not empty:
```js
    async function testAsync() {
        const result = await fs.promises.readFile('file.txt')
        @devtea2027/quis-saepe-deleniti-occaecati.assert(result, 'Empty file', callback)
    }
```

Or, with callbacks:
```js
    function testAsync(callback)
    {
        fs.readFile('file.txt', function(error, result)
        {
            if (error)
            {
                @devtea2027/quis-saepe-deleniti-occaecati.failure('File not read', callback);
            }
            @devtea2027/quis-saepe-deleniti-occaecati.assert(result, 'Empty file', callback);
            @devtea2027/quis-saepe-deleniti-occaecati.success(callback);
        });
    }
```

### Running all tests

Pass an array to `@devtea2027/quis-saepe-deleniti-occaecati.run()` to run all tests sequentially:

```js
@devtea2027/quis-saepe-deleniti-occaecati.run([
	testAdd,
	testAsync,
], callback);
```

Usually tests are stored in a separate file.
Each test file will export its tests as a function,
e.g. `testAdd()`.
An aggregate file will run all package tests directly:

```js
import @devtea2027/quis-saepe-deleniti-occaecati from '@devtea2027/quis-saepe-deleniti-occaecati'
import testAdd from './testAdd.js'
import testAsync from './testAsync.js'

/**
 * Run package tests.
 */
function test(callback) {
	var tests = [
		testAdd,
		testAsync,
	];
	@devtea2027/quis-saepe-deleniti-occaecati.run(tests, callback);
};
	
test(@devtea2027/quis-saepe-deleniti-occaecati.show);
```

Tests are run every time this aggregate file is invoked directly:

```js
 $ node test/all.js
```

## API

Implementation is very easy, based around three functions.

### Basics

Callbacks are used for asynchronous @devtea2027/quis-saepe-deleniti-occaecati. They follow the usual node.js convention:
```js
    callback(error, result);
```
When no callback is passed, synchronous @devtea2027/quis-saepe-deleniti-occaecati is performed.

#### @devtea2027/quis-saepe-deleniti-occaecati.success([message], [callback])

Note success for the current test. An optional message is shown if there is no callback.

If there is a callback, then it is called with the message. Default message: true.

Example:
```js
    @devtea2027/quis-saepe-deleniti-occaecati.success(callback);
```
#### @devtea2027/quis-saepe-deleniti-occaecati.failure([message], [callback])

Note failure for the current test.

If the callback is present, calls the callback with the error:
```js
    callback(message);
```
Otherwise the message is shown using console.error(). Default message: 'Error'.

Example:
```js
    @devtea2027/quis-saepe-deleniti-occaecati.failure('An error happened', callback);
```

#### @devtea2027/quis-saepe-deleniti-occaecati.fail([message], [callback])

Alias to `@devtea2027/quis-saepe-deleniti-occaecati.failure()`.

#### @devtea2027/quis-saepe-deleniti-occaecati.run(tests, [timeout], [callback])

Run a set of tests. The first parameter is an object containing one attribute for every @devtea2027/quis-saepe-deleniti-occaecati function.

The tests are considered as a failure when a certain configurable timeout has passed.
The timeout parameter is in milliseconds. The default is 2 seconds per test.

When the optional callback is given, it is called after a failure or the success of all tests.
The callback has this signature, following the usual Node.js syntax:

    function (error, result) {
        ...
    }

`error` will contain the results of the tests when they fail.
`result` will contain the results when they succeed.

Example:
```js
    @devtea2027/quis-saepe-deleniti-occaecati.run({
        first: testFirst,
        second: testSecond,
    }, 1000, callback);
```
For each attribute, the key is used to display success; the value is a @devtea2027/quis-saepe-deleniti-occaecati function that accepts an optional callback.

Note: @devtea2027/quis-saepe-deleniti-occaecati uses async to run tests in series.

### Asserts

There are several utility methods for assertions.

#### @devtea2027/quis-saepe-deleniti-occaecati.verify(condition, [message], [callback])
#### @devtea2027/quis-saepe-deleniti-occaecati.assert(condition, [message], [callback])

Checks condition; if true, does nothing. Otherwise calls the callback passing the message, if present.

When there is no callback, just prints the message to console.log() for success, console.error() for errors.
Default message: 'Assertion error'.

Example:
```js
    @devtea2027/quis-saepe-deleniti-occaecati.verify(shouldBeTrue(), 'shouldBeTrue() should return a truthy value', callback);
```

#### @devtea2027/quis-saepe-deleniti-occaecati.equals(actual, expected, [message], [callback])
#### @devtea2027/quis-saepe-deleniti-occaecati.assertEquals(actual, expected, [message], [callback])

Check that the given values are equal. Uses weak equality (==).

Message and callback behave just like above.

Example:

```js
    @devtea2027/quis-saepe-deleniti-occaecati.equals(getOnePlusOne(), 2, 'getOnePlusOne() does not work', callback);
```

#### @devtea2027/quis-saepe-deleniti-occaecati.notEquals(actual, unexpected, [message], [callback])
#### @devtea2027/quis-saepe-deleniti-occaecati.assertNotEquals(actual, unexpected, [message], [callback])

Inverse of the above, check that the given values are *not* equal. Uses weak inequality (!=).

#### @devtea2027/quis-saepe-deleniti-occaecati.contains(container, piece, [message], [callback])

Check that the container contains the piece.
Works for strings and arrays.

Message and callback behave just like above.

Example:

    @devtea2027/quis-saepe-deleniti-occaecati.contains('Big string', 'g s', 'Does not contain', callback);

#### @devtea2027/quis-saepe-deleniti-occaecati.check(error, [message], [callback])

Check there are no errors.
Almost the exact opposite of an assertion: if there is an error, count as a failure.
Otherwise, do nothing.

Example:
```js
    @devtea2027/quis-saepe-deleniti-occaecati.check(error, 'There should be no errors', callback);
```
Similar to over the following code:
```js
    @devtea2027/quis-saepe-deleniti-occaecati.assert(!error, 'There should be no errors', callback);
```
But with the advantage that it shows the actual error message should there be one.

### Showing results

You can use your own function to show results. The library provides a premade callback:

#### @devtea2027/quis-saepe-deleniti-occaecati.show(error, result)

Show an error if present, a success if there was no error.

Example:
```js
    @devtea2027/quis-saepe-deleniti-occaecati.run(tests, @devtea2027/quis-saepe-deleniti-occaecati.show);
```
This line can be run at the end of every code file to run its set of tests.

#### @devtea2027/quis-saepe-deleniti-occaecati.showComplete(error, result)

Like `@devtea2027/quis-saepe-deleniti-occaecati.show()`, but shows the complete hierarchical tree of tests.
Test information is therefore duplicated: once shown while running,
another after all tests.

Example:
```js
    exports.test(@devtea2027/quis-saepe-deleniti-occaecati.showComplete);
```
#### @devtea2027/quis-saepe-deleniti-occaecati.toShow(tester)

Returns a function with a callback as parameter,
that runs the tests, shows results and invokes the callback.
Useful to insert in some callback loop.

Example:
```js
    var tasks = [@devtea2027/quis-saepe-deleniti-occaecati.toShow(test1), @devtea2027/quis-saepe-deleniti-occaecati.toShow(test2)];
    async.parallel(tasks, function()
    {
        console.log('All tests run');
    });
```
Runs a couple of tests in parallel, showing their results as they finish.

### Sample code

This library is tested using itself, check it out!
  https://github.com/devtea2027/quis-saepe-deleniti-occaecati/blob/master/test.js

## License

(The MIT License)

Copyright (c) 2013-2023 Alex Fernández <alexfernandeznpm@gmail.com>

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the 'Software'), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED 'AS IS', WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.

