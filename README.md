<!--

@license Apache-2.0

Copyright (c) 2021 The Stdlib Authors.

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

   http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.

-->

<!-- lint disable maximum-heading-length -->

# ahaversinBy

[![NPM version][npm-image]][npm-url] [![Build Status][test-image]][test-url] [![Coverage Status][coverage-image]][coverage-url] <!-- [![dependencies][dependencies-image]][dependencies-url] -->

> Compute the [inverse half-value versed sine][@stdlib/math/base/special/ahaversin] of each element retrieved from an input strided array via a callback function.

<section class="intro">

</section>

<!-- /.intro -->



<section class="usage">

## Usage

To use in Observable,

```javascript
ahaversinBy = require( 'https://cdn.jsdelivr.net/gh/stdlib-js/math-strided-special-ahaversin-by@umd/browser.js' )
```

To vendor stdlib functionality and avoid installing dependency trees for Node.js, you can use the UMD server build:

```javascript
var ahaversinBy = require( 'path/to/vendor/umd/math-strided-special-ahaversin-by/index.js' )
```

To include the bundle in a webpage,

```html
<script type="text/javascript" src="https://cdn.jsdelivr.net/gh/stdlib-js/math-strided-special-ahaversin-by@umd/browser.js"></script>
```

If no recognized module system is present, access bundle contents via the global scope:

```html
<script type="text/javascript">
(function () {
    window.ahaversinBy;
})();
</script>
```

#### ahaversinBy( N, x, strideX, y, strideY, clbk\[, thisArg] )

Computes the [inverse half-value versed sine][@stdlib/math/base/special/ahaversin] of each element retrieved from an input strided array `x` via a callback function and assigns each result to an element in an output strided array `y`.

```javascript
function accessor( v ) {
    return v;
}

var x = [ 0.0, 0.5, 1.0, 0.25, 0.75 ];
var y = [ 0.0, 0.0, 0.0, 0.0, 0.0 ];

ahaversinBy( x.length, x, 1, y, 1, accessor );
// y => [ 0.0, ~1.571, ~3.142, ~1.047, ~2.094 ]
```

The function accepts the following arguments:

-   **N**: number of indexed elements.
-   **x**: input [`Array`][mdn-array], [`typed array`][mdn-typed-array], or an array-like object (excluding strings and functions).
-   **strideX**: index increment for `x`.
-   **y**: output [`Array`][mdn-array], [`typed array`][mdn-typed-array], or an array-like object (excluding strings and functions).
-   **strideY**: index increment for `y`.
-   **clbk**: callback function.
-   **thisArg**: execution context (_optional_).

The invoked callback function is provided six arguments:

-   **value**: input array element.
-   **idx**: iteration index (zero-based).
-   **xi**: input array strided index (`offsetX + idx*strideX`).
-   **yi**: output array strided index (`offsetY + idx*strideY`).
-   **x**: input array/collection.
-   **y**: output array/collection.

To set the callback execution context, provide a `thisArg`.

```javascript
function accessor( v ) {
    this.count += 1;
    return v;
}

var context = {
    'count': 0
};

var x = [ 0.0, 0.5, 1.0, 0.25, 0.75 ];
var y = [ 0.0, 0.0, 0.0, 0.0, 0.0 ];

ahaversinBy( x.length, x, 1, y, 1, accessor, context );
// y => [ 0.0, ~1.571, ~3.142, ~1.047, ~2.094 ]

var cnt = context.count;
// returns 8
```

The `N` and `stride` parameters determine which elements in `x` and `y` are accessed at runtime. For example, to index every other value in `x` and to index the first `N` elements of `y` in reverse order,

```javascript
function accessor( v ) {
    return v;
}

var x = [ 0.0, 0.5, 1.0, 0.25, 0.75, 0.68 ];
var y = [ 0.0, 0.0, 0.0, 0.0, 0.0, 0.0 ];

ahaversinBy( 3, x, 2, y, -1, accessor );
// y => [ ~2.094, ~3.142, 0.0, 0.0, 0.0, 0.0 ]
```

Note that indexing is relative to the first index. To introduce an offset, use [`typed array`][mdn-typed-array] views.

```javascript
var Float64Array = require( '@stdlib/array-float64' );

function accessor( v ) {
    return v;
}

// Initial arrays...
var x0 = new Float64Array( [ 0.0, 0.5, 1.0, 0.25, 0.75, 0.68 ] );
var y0 = new Float64Array( [ 0.0, 0.0, 0.0, 0.0, 0.0, 0.0 ] );

// Create offset views...
var x1 = new Float64Array( x0.buffer, x0.BYTES_PER_ELEMENT*1 ); // start at 2nd element
var y1 = new Float64Array( y0.buffer, y0.BYTES_PER_ELEMENT*3 ); // start at 4th element

ahaversinBy( 3, x1, -2, y1, 1, accessor );
// y0 => <Float64Array>[ 0.0, 0.0, 0.0, ~1.939, ~1.047, ~1.571 ]
```

#### ahaversinBy.ndarray( N, x, strideX, offsetX, y, strideY, offsetY, clbk\[, thisArg] )

Computes the [inverse half-value versed sine][@stdlib/math/base/special/ahaversin] of each element retrieved from an input strided array `x` via a callback function and assigns each result to an element in an output strided array `y` using alternative indexing semantics.

```javascript
function accessor( v ) {
    return v;
}

var x = [ 0.0, 0.5, 1.0, 0.25, 0.75 ];
var y = [ 0.0, 0.0, 0.0, 0.0, 0.0 ];

ahaversinBy.ndarray( x.length, x, 1, 0, y, 1, 0, accessor );
// y => [ 0.0, ~1.571, ~3.142, ~1.047, ~2.094 ]
```

The function accepts the following additional arguments:

-   **offsetX**: starting index for `x`.
-   **offsetY**: starting index for `y`.

While [`typed array`][mdn-typed-array] views mandate a view offset based on the underlying `buffer`, the `offsetX` and `offsetY` parameters support indexing semantics based on starting indices. For example, to index every other value in `x` starting from the second value and to index the last `N` elements in `y`,

```javascript
function accessor( v ) {
    return v;
}

var x = [ 0.0, 0.5, 1.0, 0.25, 0.75, 0.68 ];
var y = [ 0.0, 0.0, 0.0, 0.0, 0.0, 0.0 ];

ahaversinBy.ndarray( 3, x, 2, 1, y, -1, y.length-1, accessor );
// y => [ 0.0, 0.0, 0.0, ~1.939, ~1.047, ~1.571 ]
```

</section>

<!-- /.usage -->

<section class="notes">

## Notes

-   If a provided callback function does not return any value (or equivalently, explicitly returns `undefined`), the value is **ignored**.

    ```javascript
    function accessor() {
        // No-op...
    }

    var x = [ 0.0, 0.5, 1.0, 0.25, 0.75 ];
    var y = [ 0.0, 0.0, 0.0, 0.0, 0.0 ];

    ahaversinBy( x.length, x, 1, y, 1, accessor );
    // y => [ 0.0, 0.0, 0.0, 0.0, 0.0 ]
    ```

</section>

<!-- /.notes -->

<section class="examples">

## Examples

<!-- eslint no-undef: "error" -->

```html
<!DOCTYPE html>
<html lang="en">
<body>
<script type="text/javascript" src="https://cdn.jsdelivr.net/gh/stdlib-js/random-base-uniform@umd/browser.js"></script>
<script type="text/javascript" src="https://cdn.jsdelivr.net/gh/stdlib-js/array-filled@umd/browser.js"></script>
<script type="text/javascript" src="https://cdn.jsdelivr.net/gh/stdlib-js/math-strided-special-ahaversin-by@umd/browser.js"></script>
<script type="text/javascript">
(function () {

function accessor( v, i ) {
    if ( (i%3) === 0 ) {
        // Simulate a "missing" value...
        return;
    }
    return v;
}

var x = filledarray( 0.0, 10, 'generic' );
var y = filledarray( null, 10, 'generic' );

var i;
for ( i = 0; i < x.length; i++ ) {
    x[ i ] = uniform( 0.0, 1.0 );
}
console.log( x );
console.log( y );

ahaversinBy.ndarray( x.length, x, 1, 0, y, -1, y.length-1, accessor );
console.log( y );

})();
</script>
</body>
</html>
```

</section>

<!-- /.examples -->

<!-- Section for related `stdlib` packages. Do not manually edit this section, as it is automatically populated. -->

<section class="related">

</section>

<!-- /.related -->

<!-- Section for all links. Make sure to keep an empty line after the `section` element and another before the `/section` close. -->


<section class="main-repo" >

* * *

## Notice

This package is part of [stdlib][stdlib], a standard library for JavaScript and Node.js, with an emphasis on numerical and scientific computing. The library provides a collection of robust, high performance libraries for mathematics, statistics, streams, utilities, and more.

For more information on the project, filing bug reports and feature requests, and guidance on how to develop [stdlib][stdlib], see the main project [repository][stdlib].

#### Community

[![Chat][chat-image]][chat-url]

---

## License

See [LICENSE][stdlib-license].


## Copyright

Copyright &copy; 2016-2022. The Stdlib [Authors][stdlib-authors].

</section>

<!-- /.stdlib -->

<!-- Section for all links. Make sure to keep an empty line after the `section` element and another before the `/section` close. -->

<section class="links">

[npm-image]: http://img.shields.io/npm/v/@stdlib/math-strided-special-ahaversin-by.svg
[npm-url]: https://npmjs.org/package/@stdlib/math-strided-special-ahaversin-by

[test-image]: https://github.com/stdlib-js/math-strided-special-ahaversin-by/actions/workflows/test.yml/badge.svg?branch=v0.0.8
[test-url]: https://github.com/stdlib-js/math-strided-special-ahaversin-by/actions/workflows/test.yml?query=branch:v0.0.8

[coverage-image]: https://img.shields.io/codecov/c/github/stdlib-js/math-strided-special-ahaversin-by/main.svg
[coverage-url]: https://codecov.io/github/stdlib-js/math-strided-special-ahaversin-by?branch=main

<!--

[dependencies-image]: https://img.shields.io/david/stdlib-js/math-strided-special-ahaversin-by.svg
[dependencies-url]: https://david-dm.org/stdlib-js/math-strided-special-ahaversin-by/main

-->

[chat-image]: https://img.shields.io/gitter/room/stdlib-js/stdlib.svg
[chat-url]: https://gitter.im/stdlib-js/stdlib/

[stdlib]: https://github.com/stdlib-js/stdlib

[stdlib-authors]: https://github.com/stdlib-js/stdlib/graphs/contributors

[umd]: https://github.com/umdjs/umd
[es-module]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Modules

[deno-url]: https://github.com/stdlib-js/math-strided-special-ahaversin-by/tree/deno
[umd-url]: https://github.com/stdlib-js/math-strided-special-ahaversin-by/tree/umd
[esm-url]: https://github.com/stdlib-js/math-strided-special-ahaversin-by/tree/esm
[branches-url]: https://github.com/stdlib-js/math-strided-special-ahaversin-by/blob/main/branches.md

[stdlib-license]: https://raw.githubusercontent.com/stdlib-js/math-strided-special-ahaversin-by/main/LICENSE

[mdn-array]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array

[mdn-typed-array]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/TypedArray

[@stdlib/math/base/special/ahaversin]: https://github.com/stdlib-js/math-base-special-ahaversin/tree/umd

</section>

<!-- /.links -->
