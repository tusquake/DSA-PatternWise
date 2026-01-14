# LRU Cache - Real World Applications

## Problem Statement

Design a data structure that follows the constraints of a Least Recently Used (LRU) cache. Implement the LRUCache class with:
- `get(key)` - Get the value of the key if it exists, otherwise return -1
- `put(key, value)` - Update or insert the value if the key exists. When the cache reaches its capacity, it should invalidate the least recently used item before inserting a new item.

Both operations should run in O(1) average time complexity.

**Example:**
```
Input:
["LRUCache", "put", "put", "get", "put", "get", "put", "get", "get", "get"]
[[2], [1,1], [2,2], [1], [3,3], [2], [4,4], [1], [3], [4]]

Output:
[null, null, null, 1, null, -1, null, -1, 3, 4]

Explanation:
LRUCache cache = new LRUCache(2);
cache.put(1, 1);    // cache: {1=1}
cache.put(2, 2);    // cache: {1=1, 2=2}
cache.get(1);       // returns 1, cache: {2=2, 1=1}
cache.put(3, 3);    // evicts key 2, cache: {1=1, 3=3}
cache.get(2);       // returns -1 (not found)
cache.put(4, 4);    // evicts key 1, cache: {3=3, 4=4}
cache.get(1);       // returns -1 (not found)
cache.get(3);       // returns 3
cache.get(4);       // returns 4
```

**Constraints:**
- 1 <= capacity <= 3000
- 0 <= key <= 10^4
- 0 <= value <= 10^5
- At most 2 * 10^5 calls to get and put

**Solution (Java):**
```java
class LRUCache {
    class Node {
        int key, value;
        Node prev, next;
        
        Node(int key, int value) {
            this.key = key;
            this.value = value;
        }
    }
    
    private Map<Integer, Node> cache;
    private int capacity;
    private Node head, tail;
    
    public LRUCache(int capacity) {
        this.capacity = capacity;
        this.cache = new HashMap<>();
        
        // Dummy head and tail nodes
        head = new Node(0, 0);
        tail = new Node(0, 0);
        head.next = tail;
        tail.prev = head;
    }
    
    public int get(int key) {
        if (!cache.containsKey(key)) {
            return -1;
        }
        
        Node node = cache.get(key);
        // Move to front (most recently used)
        remove(node);
        insertAtFront(node);
        
        return node.value;
    }
    
    public void put(int key, int value) {
        if (cache.containsKey(key)) {
            // Update existing key
            Node node = cache.get(key);
            node.value = value;
            remove(node);
            insertAtFront(node);
        } else {
            // Insert new key
            if (cache.size() >= capacity) {
                // Remove least recently used (tail)
                Node lru = tail.prev;
                remove(lru);
                cache.remove(lru.key);
            }
            
            Node newNode = new Node(key, value);
            insertAtFront(newNode);
            cache.put(key, newNode);
        }
    }
    
    private void remove(Node node) {
        node.prev.next = node.next;
        node.next.prev = node.prev;
    }
    
    private void insertAtFront(Node node) {
        node.next = head.next;
        node.prev = head;
        head.next.prev = node;
        head.next = node;
    }
}
```

---

## Core Concept

This problem is NOT just about caching. It teaches:

- Combining HashMap (O(1) lookup) + Doubly Linked List (O(1) insertion/deletion)
- Eviction policies
- Space-time trade-offs
- Access pattern optimization

Any place where you need fast access to recently used data with limited memory - this pattern applies.

---

## Real-World Use Cases

### 1. Web Browser Cache

**Problem:** Store recently visited web pages to avoid re-downloading

**Example:** Browser page cache (JavaScript)
```javascript
class BrowserCache {
    constructor(maxPages) {
        this.cache = new Map();
        this.maxPages = maxPages;
    }
    
    getPage(url) {
        if (!this.cache.has(url)) {
            // Page not in cache, fetch from network
            return this.fetchFromNetwork(url);
        }
        
        // Move to end (most recently used)
        const page = this.cache.get(url);
        this.cache.delete(url);
        this.cache.set(url, page);
        
        return page;
    }
    
    cachePage(url, content) {
        if (this.cache.has(url)) {
            this.cache.delete(url);
        }
        
        if (this.cache.size >= this.maxPages) {
            // Remove least recently used (first item)
            const firstKey = this.cache.keys().next().value;
            this.cache.delete(firstKey);
        }
        
        this.cache.set(url, content);
    }
}

// Browser can cache last 50 visited pages
// Back button is instant because pages are cached
```

**Use Case:**
- Chrome/Firefox page caching
- Back/Forward navigation
- Tab restoration

### 2. Database Query Cache

**Problem:** Cache frequent database query results to reduce database load

**Example:** Query result cache (Backend - Java)
```java
public class QueryCache {
    private LRUCache cache;
    
    public QueryCache(int capacity) {
        this.cache = new LRUCache(capacity);
    }
    
    public ResultSet executeQuery(String sql) {
        int queryHash = sql.hashCode();
        
        // Check cache first
        ResultSet cached = cache.get(queryHash);
        if (cached != null) {
            System.out.println("Cache hit!");
            return cached;
        }
        
        // Cache miss - execute query
        System.out.println("Cache miss - querying database");
        ResultSet result = database.execute(sql);
        
        // Store in cache
        cache.put(queryHash, result);
        return result;
    }
}

// Frequently run queries are cached
// Reduces database load by 70-80%
```

**Use Case:**
- MySQL query cache
- Redis caching layer
- ORM query optimization

### 3. CDN (Content Delivery Network)

**Problem:** Cache popular content at edge servers

**Example:** CDN edge cache (Backend - Java)
```java
public class CDNEdgeCache {
    private LRUCache contentCache;
    
    public CDNEdgeCache(int capacity) {
        this.contentCache = new LRUCache(capacity);
    }
    
    public byte[] getContent(String contentUrl) {
        int urlHash = contentUrl.hashCode();
        
        byte[] cached = contentCache.get(urlHash);
        if (cached != null) {
            // Serve from edge cache
            return cached;
        }
        
        // Fetch from origin server
        byte[] content = originServer.fetch(contentUrl);
        contentCache.put(urlHash, content);
        
        return content;
    }
}

// Popular videos/images cached at edge
// Netflix, YouTube use this heavily
```

**Use Case:**
- Cloudflare, Akamai CDN
- Video streaming (Netflix, YouTube)
- Static asset delivery

### 4. API Rate Limiting with Recent Requests

**Problem:** Track recent API requests per user to enforce rate limits

**Example:** Rate limiter (Backend - Java)
```java
public class RateLimiter {
    private Map<String, LRUCache> userRequestCache;
    private int maxRequestsPerMinute;
    
    public RateLimiter(int maxRequests) {
        this.userRequestCache = new HashMap<>();
        this.maxRequestsPerMinute = maxRequests;
    }
    
    public boolean allowRequest(String userId) {
        if (!userRequestCache.containsKey(userId)) {
            userRequestCache.put(userId, new LRUCache(maxRequestsPerMinute));
        }
        
        LRUCache userCache = userRequestCache.get(userId);
        long currentTime = System.currentTimeMillis();
        
        // Store request timestamp
        userCache.put(currentTime, 1);
        
        // Check if user exceeded limit
        return userCache.size() <= maxRequestsPerMinute;
    }
}

// Limits API calls to 100 per minute per user
```

**Use Case:**
- API gateways
- DDoS protection
- Fair usage enforcement

### 5. Session Management

**Problem:** Store active user sessions with automatic cleanup of inactive ones

**Example:** Session manager (Backend - Java)
```java
public class SessionManager {
    private LRUCache sessions;
    
    public SessionManager(int maxActiveSessions) {
        this.sessions = new LRUCache(maxActiveSessions);
    }
    
    public Session getSession(String sessionId) {
        Session session = sessions.get(sessionId.hashCode());
        
        if (session == null) {
            // Create new session
            session = new Session(sessionId);
            sessions.put(sessionId.hashCode(), session);
        }
        
        // Update last access time
        session.updateLastAccess();
        return session;
    }
    
    public void invalidateSession(String sessionId) {
        sessions.remove(sessionId.hashCode());
    }
}

// Keeps only last 10,000 active sessions
// Automatically evicts inactive sessions
```

**Use Case:**
- Web application sessions
- Shopping cart persistence
- Login state management

### 6. DNS Cache

**Problem:** Cache DNS lookups to avoid repeated DNS queries

**Example:** DNS resolver cache (JavaScript)
```javascript
class DNSCache {
    constructor(maxEntries) {
        this.cache = new Map();
        this.maxEntries = maxEntries;
    }
    
    resolve(domain) {
        if (this.cache.has(domain)) {
            // Move to end (recently used)
            const ip = this.cache.get(domain);
            this.cache.delete(domain);
            this.cache.set(domain, ip);
            return ip;
        }
        
        // Perform DNS lookup
        const ip = this.performDNSLookup(domain);
        
        if (this.cache.size >= this.maxEntries) {
            // Remove oldest entry
            const firstKey = this.cache.keys().next().value;
            this.cache.delete(firstKey);
        }
        
        this.cache.set(domain, ip);
        return ip;
    }
}

// Caches DNS lookups for fast resolution
// google.com → 142.250.185.46 (cached)
```

**Use Case:**
- Operating system DNS cache
- Browser DNS cache
- Router DNS cache

### 7. Image/Asset Cache in Mobile Apps

**Problem:** Cache recently viewed images to avoid re-downloading

**Example:** Image cache (JavaScript - React Native)
```javascript
class ImageCache {
    constructor(maxImages) {
        this.cache = new Map();
        this.maxImages = maxImages;
    }
    
    getImage(imageUrl) {
        if (this.cache.has(imageUrl)) {
            const image = this.cache.get(imageUrl);
            // Mark as recently used
            this.cache.delete(imageUrl);
            this.cache.set(imageUrl, image);
            return image;
        }
        
        return null; // Need to download
    }
    
    cacheImage(imageUrl, imageData) {
        if (this.cache.size >= this.maxImages) {
            // Remove least recently viewed image
            const firstUrl = this.cache.keys().next().value;
            this.cache.delete(firstUrl);
        }
        
        this.cache.set(imageUrl, imageData);
    }
}

// Instagram/Facebook feed scrolling
// Keeps last 100 images in memory
```

**Use Case:**
- Social media apps (Instagram, Facebook)
- E-commerce product images
- News feed images

### 8. Compilation Cache (Build Systems)

**Problem:** Cache compiled files to speed up incremental builds

**Example:** Build cache (Backend - Java)
```java
public class BuildCache {
    private LRUCache compiledFiles;
    
    public BuildCache(int capacity) {
        this.compiledFiles = new LRUCache(capacity);
    }
    
    public CompiledObject getCompiled(String sourceFile) {
        int fileHash = calculateHash(sourceFile);
        
        CompiledObject cached = compiledFiles.get(fileHash);
        if (cached != null && !isModified(sourceFile, cached)) {
            // Use cached compilation
            return cached;
        }
        
        // Recompile
        CompiledObject compiled = compile(sourceFile);
        compiledFiles.put(fileHash, compiled);
        
        return compiled;
    }
}

// Webpack, Gradle, Maven use similar caching
// Speeds up builds by 10x
```

**Use Case:**
- Webpack module bundling
- Java/Maven compilation
- Docker layer caching

---

## Why This Matters in Production

### Performance Impact
```
Without Cache:
- Database query: 50-200ms
- With 1000 requests/sec = 50-200 seconds of DB time per second!

With LRU Cache:
- Cache hit: <1ms
- 80% cache hit rate = 80% of requests served in <1ms
- DB load reduced by 80%
```

### Memory Management
```java
// Fixed memory usage
LRUCache cache = new LRUCache(1000); // Max 1000 items

// Automatic eviction prevents OutOfMemoryError
// No manual cleanup needed
```

### Real Numbers
- **Reddit:** 90% cache hit rate on front page
- **Facebook:** Billions of memcached requests/sec
- **Netflix:** 95% of streaming served from CDN cache

---

## Interview Tip

When explaining this problem, say:

"LRU Cache is fundamental to modern system design. It combines HashMap for O(1) lookups with a doubly linked list for O(1) eviction. It's used everywhere - browser caching, database query caching, CDNs, session management, DNS resolution. The key insight is maintaining recency ordering while providing constant-time operations. This pattern reduces latency, database load, and network bandwidth while managing memory efficiently."

This demonstrates deep understanding of practical system optimization.

---

## Key Takeaway

LRU Cache is the backbone of performance optimization in production systems. The HashMap + Doubly Linked List combination enables O(1) get/put operations while automatically evicting least recently used items. Every high-performance system uses some form of LRU caching - from your browser to Netflix's CDN.