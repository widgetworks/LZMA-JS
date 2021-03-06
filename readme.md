LZMA in a Browser
===

[LZMA-JS](https://github.com/nmrugg/LZMA-JS) is a JavaScript implementation of the Lempel-Ziv-Markov chain (LZMA) compression algorithm.

What's New in 2.0
---
Two things: <b>speed</b> & <b>size</b>.

LZMA-JS 2.0 now minifies to almost half of 1.x and in some cases is 1,000x faster (particularly with high compression).

It is also more modular. The compression and decompression algorithms can be optionally separated to shrink the file size even more.

Here are some file size stats:

|    Filename    |   Method(s)   | Minified | Gzipped |
|:---------------|:--------------|---------:|--------:|
| lzma_worker.js | both          | 29.3 KB  | 11.2 KB |
| lzma-c.js      | compression   | 22.7 KB  |  9.0 KB |
| lzma-d.js      | decompression | 10.5 KB  |  4.3 KB |

Demos
---

Live demos can be found [here](http://nmrugg.github.io/LZMA-JS/ "Demos").

How to Use
---

First, load the bootstrapping code.
    
    /// In a browser:
    <script src="../src/lzma.js"></script>


Create the LZMA object.
    
    /// LZMA([optional path])
    /// If lzma_worker.js is in the same directory, you don't need to set the path.
    var my_lzma = new LZMA("../src/lzma_worker.js");

(De)Compress stuff.

    /// To compress:
    ///NOTE: mode can be 1-9 (1 is fast and pretty good; 9 is very slow and probably a bit better).
    ///      I suggest keeping mode low, like 1-3.
    ///      And by the way, 9 is not always the smallest.
    my_lzma.compress(string, mode, on_finish(result) {}, on_progress(percent) {});
    
    /// To decompress:
    my_lzma.decompress(byte_array, on_finish(result) {}, on_progress(percent) {});

Node.js
---

LZMA-JS is available in the npm repository.
    
    $ npm install lzma

It can be loaded with the following code:
    
    var my_lzma = require("lzma");
    

Notes
---

The calls to compress() and decompress() are asynchronous, so you need to supply a callback function if you
want to use the (de)compressed data.

If the decompression progress is unable to be calculated, the on_progress() function will be triggered once with the value <code>-1</code>.

LZMA-JS will try to use Web Workers if they are available.  If the environment does not support Web Workers,
it will just do something else, and it won't pollute the global scope.

LZMA-JS was originally based on [gwt-lzma](http://code.google.com/p/gwt-lzma/), which is a port of the LZMA SDK from Java into JavaScript.


But I don't want to use Web Workers
---

If you'd prefer not to bother with Web Workers, you can just include <code>lzma_worker.js</code> directly. For example:

    <script src="../src/lzma_worker.js"></script>

That will create a global <code>LZMA</code> <code>object</code> that you can use directly. Like this:

    LZMA.compress(string, mode, on_finish(result) {}, on_progress(percent) {});
    
    LZMA.decompress(byte_array, on_finish(result) {}, on_progress(percent) {});

Note that this <code>LZMA</code> variable is an <code>object</code>, not a <code>function</code>.

In Node.js, the Web Worker code is already skipped, so there's no need to do this.

And if you only need to compress or decompress and you're looking to save some bytes, instead of loading lzma_worker.js,
you can simply load lzma-c.js (for compression) or lzma-d.js (for decompression).

Of course, you'll want to load the minified versions if you're sending data over the wire.




License
---
<a href="https://raw.githubusercontent.com/nmrugg/LZMA-JS/master/LICENSE">MIT</a>
