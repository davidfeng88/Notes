# Hardware

### Hardware

* [Scott Meyers: CPU Caches and Why You Care](https://www.youtube.com/watch?v=WDIkqP4JbkE)

  Linear array traversal are extremely cache friendly. For big n, big-O rules. But for smaller n, give array a second thought.

Hardware will prefetch the data and instructions if you have a nice predictable pattern. So sequence code is the fastest. Branching, function calls will all slow things down. But don't sacrifice readability. The bottleneck might as well be the network latency.

Be careful with heterogenous arrays: the process instruction for each element might be different, invalidate the instruction cache line each time. Sort sequences by type. Make "fast paths" branch-free sequences. PGO: profile guilded optimization. Compiler first build the program with logging. Then test against typical use cases. Then rebuild with source code and the result of the first test. It will choose to inline some common paths and functions. May gain 10-20% time. WPO: while program optimization.

Cache coherency - Multithread: if you have at least one writer, use mutex or atomic instructions. But those takes time.

False sharing: independent values/variables fall on one cache line; different cores concurrently access that line; frequently; at least one is writer.

data-oriented programming: lay data in memory in a way that will make CPU cache happy. Video game programming, put the isLive info together, outside of the objects.

Cache Associatibility: also affect performance.

