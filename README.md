# sacjs
Generic n-way [Set Associative Cache]() for node.js.


## Install

```
npm install sacjs
```

## Usage

```
var Cache = require('sacjs');

var cache = new Cache({ assoc: 4, size: 10000, evict: 'lru' });

cache.put(key, data);

// ...

var data = cache.get(key);
```

`key` used to store data in the cache can be any scalar or object/array. You can supply `serialize` option that will be used to convert keys to strings, otherwise [json-stable-stringify]() will be used. Serialized keys are hashed using [Dan Bernstein's algorythm]().


## Options

Options object is passed to cache constructor.

- assoc - cache associativity level (the number of slots per set). For any given key an item can be stored in any of the slots in the set. Higher associativity improves hit ratio but reduces cache performance.

- size - cache size (the number of sets of slots the cache will store). The total number of items the cache can store is `assoc * size`

- evict - eviction algorythm. Can be either 'lru' (least recently used), 'mru' (most recently used) or any function that will receive the set of slots as a parameter and should return the index of the item that should be evicted. Implementations of eviction algorythms are in `cache.js` and `slotset.spec.js`. For available usage statistics see `Slot` class in `slotset.js`.

- serialize - custom key serialization algorythm. Should return the same string for the same key every time it is called.


## Cache statistics

Cache usage statistics are available in `stat` property of the cache instance. The `stat` object has the following properties:

- misses - the number of misses
- hits - the number of hits
- stored - the number of items that were stored in free slots
- replaced - the number of item that were replaced (different value for the same key)
- evicted - the number of items that were evicted

