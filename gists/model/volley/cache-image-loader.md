### Volley: CacheImageLoader

There are cases when you need to clean your memory. In [Volley Request Manager][1] I used `fields` to save cache references. There is more practical solution, just create a wrapper for `ImageLoader` :

**Usage**

```java
CacheImageLoader loader = ... // init loader

loader.getMemoryCache().remove(url); // remove item by key
loader.getMemoryCache().evictAll(); // remove all

loader.getDiskCache().remove(url); // remove item by key
loader.getDiskCache().clear(); // remove all
```

**Wrapper sources :**


```java
public class CacheImageLoader extends ImageLoader {

    /**
     * Constructs a new ImageLoader.
     *
     * @param queue      The RequestQueue to use for making image requests.
     * @param imageCache The cache to use as an L1 cache.
     */

    private final MemoryCache mMemoryCache;
    private final Cache mDiskCache;

    public CacheImageLoader(RequestQueue queue, MemoryCache memoryCache) {
        super(queue, memoryCache);
        mMemoryCache = memoryCache;
        mDiskCache = queue.getCache();
    }

    public MemoryCache getMemoryCache() {
        return mMemoryCache;
    }
    public Cache getDiskCache() {
        return mDiskCache;
    }
}
```

  [1]: https://github.com/yakivmospan/volley-request-manager
