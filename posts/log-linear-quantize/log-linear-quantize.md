# Log/linear quantization
## What is it?

Log/linear quantization is an algorithm which addresses the difficulty of
choosing the right aggregation resolution. Using the wrong resolution can lead
to collecting too much data (too fine-grained) or too little data
(too coarse-grained).

It addresses this problem by logarithmically aggregating by order of magnitude,
but linearly aggregating within an order of magnitude.

### Huh?

This means that small data points will be aggregated with exponentially
greater precision than larger numbers.

### For example...

So, say your metrics system logs the following data points:

    0.0231
    0.0247
    0.0445
    0.0457
    0.5423
    2
    3
    12
    14
    24
    124
    199

Log/linear quantization would reduce that data into:

    Bucket   Frequency
    0.02     2
    0.04     2
    0.5      1
    2        1
    3        1
    10       2
    20       1
    100      1


## [llquantize()](https://github.com/sentientwaffle/llquantize)
### Installation

    $ npm install llquantize

### Example

    var llquantize = require('llquantize')
      , llq = llquantize()

    // Input some data points.
    llq(0.023); llq(0.024)
    llq(0.044); llq(0.045)
    llq(0.54);  llq(0.55)
    llq(2);     llq(3)
    llq(12);    llq(14)
    llq(24)
    llq(124);   llq(199)

    // Get the accumulated data.
    llq()
    // =>
    // { "0.02": 2
    // , "0.04": 2
    // , "0.5": 2
    // , "2":   1
    // , "3":   1
    // , "10":  2
    // , "20":  1
    // , "100": 2
    // }


## More info

  * For more information about log/linear quantization (as used in DTrace), see
    [this blog post](http://dtrace.org/blogs/bmc/2011/02/08/llquantize/).
  * [llquantize](https://github.com/sentientwaffle/llquantize)

