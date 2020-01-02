# hit-cache
A memory caching library, which uses hits to a key as cache expiry vector.

This is a wrapper on top of the https://github.com/ptarjan/node-cache which provides time & hits based cache expiry, simply put content which are fetched frequently stay in the cache for longer vs content which do not have same frequency get pushed out of it.

## Example:
```javascript
const cache = require('memory-cache');
const hitCacheType = require('memory-cache').HitCache;
const hitCache = new hitCacheType(cache);
hitCache.set("Laukik", "Disappears in 5 seconds, cause not hits.", 5000);
setTimeout(() => {
    console.log("Laukik: " + hitCache.get("Laukik")); //Expected: Null
}, 6000);
hitCache.set("Popular-Laukik", "Will not disapper in 5 seconds, cause it got hit.", 5000);
//IMP:Every call to get for a given key will increase the life by lifespan param provided at the time of set.
setTimeout(() => {
    console.log("Popular-Laukik: " + hitCache.get("Popular-Laukik")); //Expected: "Will not disapper in 5 seconds, cause it got hit."
}, 2000);
setTimeout(() => {
    console.log("Popular-Laukik: " + hitCache.get("Popular-Laukik")); //Expected: "Will not disapper in 5 seconds, cause it got hit."
}, 6000);
```

## API
### Constructor `constructor(cache)`
* Parameter `cache`: Instance of the cache which should hold values. Eg:`require('cache').Cache` or `require('cache')`

### set `set(key, value, lifeSpan)`
* Parameter `key` : Key for the cached value.
* Parameter `value` : Content to be cached.
* Parameter `lifeSpan` : Default expiry time for the content, after which it will be evicted from cache unless it has hits.

### get `get(key)`
* Parameter `key` : Key for the cached value.
* Returns `null` if the key is not found, else returns content and increases its lifespan by lifespan parameter defined at the time of set.
