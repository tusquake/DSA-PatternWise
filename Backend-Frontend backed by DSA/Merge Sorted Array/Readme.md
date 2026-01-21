# Merge Sorted Array - Real World Applications

## Problem Statement

You are given two integer arrays nums1 and nums2, sorted in non-decreasing order, and two integers m and n, representing the number of elements in nums1 and nums2 respectively. Merge nums2 into nums1 as one sorted array. The final sorted array should be stored inside nums1. nums1 has a length of m + n, where the first m elements are the ones to merge, and the last n elements are set to 0 and should be ignored.

**Example 1:**
```
Input:  nums1 = [1,2,3,0,0,0], m = 3, nums2 = [2,5,6], n = 3
Output: [1,2,2,3,5,6]
Explanation: Merge [1,2,3] and [2,5,6] into nums1
```

**Example 2:**
```
Input:  nums1 = [1], m = 1, nums2 = [], n = 0
Output: [1]
```

**Example 3:**
```
Input:  nums1 = [0], m = 0, nums2 = [1], n = 1
Output: [1]
```

**Constraints:**
- nums1.length == m + n
- nums2.length == n
- 0 <= m, n <= 200
- 1 <= m + n <= 200
- -10^9 <= nums1[i], nums2[j] <= 10^9

**Solution (Java):**
```java
// Approach 1: Merge from end (Optimal - No extra space)
// Time: O(m + n), Space: O(1)
public void merge(int[] nums1, int m, int[] nums2, int n) {
    int p1 = m - 1;      // Pointer for nums1
    int p2 = n - 1;      // Pointer for nums2
    int p = m + n - 1;   // Pointer for merged position
    
    // Merge from end to beginning
    while (p2 >= 0) {
        // If nums1 has elements and current element is larger
        if (p1 >= 0 && nums1[p1] > nums2[p2]) {
            nums1[p] = nums1[p1];
            p1--;
        } else {
            nums1[p] = nums2[p2];
            p2--;
        }
        p--;
    }
}

// Approach 2: Using extra space (simpler logic)
// Time: O(m + n), Space: O(m)
public void mergeWithExtraSpace(int[] nums1, int m, int[] nums2, int n) {
    int[] temp = new int[m];
    
    // Copy nums1 to temp
    for (int i = 0; i < m; i++) {
        temp[i] = nums1[i];
    }
    
    int p1 = 0, p2 = 0, p = 0;
    
    // Merge temp and nums2 into nums1
    while (p1 < m && p2 < n) {
        if (temp[p1] <= nums2[p2]) {
            nums1[p++] = temp[p1++];
        } else {
            nums1[p++] = nums2[p2++];
        }
    }
    
    // Copy remaining elements
    while (p1 < m) nums1[p++] = temp[p1++];
    while (p2 < n) nums1[p++] = nums2[p2++];
}

// Approach 3: Simple but not in-place
// Time: O((m+n)log(m+n)), Space: O(1)
public void mergeSort(int[] nums1, int m, int[] nums2, int n) {
    // Copy nums2 into nums1
    for (int i = 0; i < n; i++) {
        nums1[m + i] = nums2[i];
    }
    
    // Sort the entire array
    Arrays.sort(nums1);
}
```

---

## Core Concept

This problem is NOT just about merging arrays. It teaches:

- Two-pointer technique
- In-place array manipulation
- Merging sorted data efficiently
- Space optimization strategies

Any place where you need to combine sorted datasets, merge log files, consolidate data streams, or aggregate sorted results - this pattern applies.

---

## Real-World Use Cases

### 1. Database Query Results: Merging Sorted Data

**Problem:** Merge results from multiple database shards/partitions

**Example:** Database result merger (Backend - Java)
```java
public class DatabaseResultMerger {
    
    public List<Record> mergeShardResults(
            List<Record> shard1, 
            List<Record> shard2) {
        // Both shards return results sorted by timestamp
        // Merge them efficiently
        
        List<Record> merged = new ArrayList<>(
            shard1.size() + shard2.size()
        );
        
        int i = 0, j = 0;
        
        while (i < shard1.size() && j < shard2.size()) {
            if (shard1.get(i).getTimestamp() <= shard2.get(j).getTimestamp()) {
                merged.add(shard1.get(i++));
            } else {
                merged.add(shard2.get(j++));
            }
        }
        
        // Add remaining records
        while (i < shard1.size()) merged.add(shard1.get(i++));
        while (j < shard2.size()) merged.add(shard2.get(j++));
        
        return merged;
    }
}

// Used by: MongoDB, Cassandra for distributed queries
// Elasticsearch shard result aggregation
```

**Use Case:**
- Distributed database queries
- Shard result aggregation
- Multi-partition data merging

### 2. Log File Analysis: Merging Server Logs

**Problem:** Merge log files from multiple servers in chronological order

**Example:** Log merger (Backend - Java)
```java
public class LogFileMerger {
    
    public void mergeLogs(String[] server1Logs, String[] server2Logs) {
        // Both log arrays sorted by timestamp
        // Merge into single chronological view
        
        String[] merged = new String[server1Logs.length + server2Logs.length];
        int p1 = 0, p2 = 0, p = 0;
        
        while (p1 < server1Logs.length && p2 < server2Logs.length) {
            long time1 = extractTimestamp(server1Logs[p1]);
            long time2 = extractTimestamp(server2Logs[p2]);
            
            if (time1 <= time2) {
                merged[p++] = server1Logs[p1++];
            } else {
                merged[p++] = server2Logs[p2++];
            }
        }
        
        // Copy remaining
        while (p1 < server1Logs.length) {
            merged[p++] = server1Logs[p1++];
        }
        while (p2 < server2Logs.length) {
            merged[p++] = server2Logs[p2++];
        }
    }
    
    private long extractTimestamp(String logEntry) {
        // Extract timestamp from log format
        return Long.parseLong(logEntry.split(" ")[0]);
    }
}

// Used by: Splunk, ELK Stack, CloudWatch
// System monitoring and debugging
```

**Use Case:**
- Log aggregation systems
- Debugging distributed systems
- Audit trail generation

### 3. E-Commerce: Merging Product Search Results

**Problem:** Combine search results from multiple sources (inventory, recommendations)

**Example:** Search result merger (JavaScript)
```javascript
class SearchResultMerger {
    
    mergeResults(inventoryResults, recommendedResults) {
        // Both sorted by relevance score (descending)
        // Merge maintaining order
        
        const merged = [];
        let i = 0, j = 0;
        
        while (i < inventoryResults.length && j < recommendedResults.length) {
            if (inventoryResults[i].score >= recommendedResults[j].score) {
                merged.push(inventoryResults[i++]);
            } else {
                merged.push(recommendedResults[j++]);
            }
        }
        
        // Add remaining
        while (i < inventoryResults.length) {
            merged.push(inventoryResults[i++]);
        }
        while (j < recommendedResults.length) {
            merged.push(recommendedResults[j++]);
        }
        
        return merged;
    }
    
    deduplicateAndMerge(source1, source2) {
        const merged = this.mergeResults(source1, source2);
        
        // Remove duplicates while maintaining order
        const seen = new Set();
        return merged.filter(item => {
            if (seen.has(item.id)) return false;
            seen.add(item.id);
            return true;
        });
    }
}

// Amazon, eBay combine inventory and recommendations
```

**Use Case:**
- Product search aggregation
- Multi-source recommendation systems
- Search result ranking

### 4. Social Media: Merging Multiple User Feeds

**Problem:** Combine posts from multiple sources in chronological order

**Example:** Feed merger (JavaScript)
```javascript
class SocialFeedMerger {
    
    mergeFeeds(followingFeed, trendingFeed) {
        // Both feeds sorted by timestamp (newest first)
        // Merge to show unified timeline
        
        const merged = [];
        let i = 0, j = 0;
        
        while (i < followingFeed.length && j < trendingFeed.length) {
            const time1 = followingFeed[i].timestamp;
            const time2 = trendingFeed[j].timestamp;
            
            // Newer posts first (descending order)
            if (time1 >= time2) {
                merged.push(followingFeed[i++]);
            } else {
                merged.push(trendingFeed[j++]);
            }
        }
        
        // Add remaining posts
        while (i < followingFeed.length) {
            merged.push(followingFeed[i++]);
        }
        while (j < trendingFeed.length) {
            merged.push(trendingFeed[j++]);
        }
        
        return merged;
    }
}

// Twitter, Facebook timeline generation
// Instagram feed merging
```

**Use Case:**
- Social media timeline generation
- Activity feed aggregation
- News feed compilation

### 5. Analytics: Merging Metric Data Streams

**Problem:** Combine metrics from multiple monitoring sources

**Example:** Metrics merger (Backend - Java)
```java
public class MetricsMerger {
    
    public MetricPoint[] mergeMetrics(
            MetricPoint[] cpu, 
            MetricPoint[] memory) {
        // Both sorted by timestamp
        // Merge for unified dashboard view
        
        MetricPoint[] merged = new MetricPoint[cpu.length + memory.length];
        int p1 = 0, p2 = 0, p = 0;
        
        while (p1 < cpu.length && p2 < memory.length) {
            if (cpu[p1].timestamp <= memory[p2].timestamp) {
                merged[p++] = cpu[p1++];
            } else {
                merged[p++] = memory[p2++];
            }
        }
        
        // Copy remaining
        while (p1 < cpu.length) merged[p++] = cpu[p1++];
        while (p2 < memory.length) merged[p++] = memory[p2++];
        
        return merged;
    }
    
    public void displayUnifiedMetrics(MetricPoint[] merged) {
        // Show CPU and Memory metrics in chronological order
        for (MetricPoint point : merged) {
            System.out.println(point.timestamp + ": " + 
                             point.type + " = " + point.value);
        }
    }
}

// Used by: Datadog, New Relic, Prometheus
// System performance monitoring
```

**Use Case:**
- Performance monitoring dashboards
- System health visualization
- Real-time analytics

### 6. Financial Systems: Merging Transaction Lists

**Problem:** Combine transaction records from multiple accounts/sources

**Example:** Transaction merger (Backend - Java)
```java
public class TransactionMerger {
    
    public Transaction[] mergeTransactions(
            Transaction[] account1, 
            Transaction[] account2) {
        // Both arrays sorted by transaction date
        // Merge for consolidated statement
        
        Transaction[] merged = new Transaction[
            account1.length + account2.length
        ];
        int p1 = 0, p2 = 0, p = 0;
        
        while (p1 < account1.length && p2 < account2.length) {
            if (account1[p1].date.compareTo(account2[p2].date) <= 0) {
                merged[p++] = account1[p1++];
            } else {
                merged[p++] = account2[p2++];
            }
        }
        
        // Add remaining transactions
        while (p1 < account1.length) {
            merged[p++] = account1[p1++];
        }
        while (p2 < account2.length) {
            merged[p++] = account2[p2++];
        }
        
        return merged;
    }
    
    public double calculateBalance(Transaction[] merged) {
        double balance = 0;
        for (Transaction txn : merged) {
            balance += txn.amount;
        }
        return balance;
    }
}

// Banks merge checking + savings transactions
// Credit card statement generation
```

**Use Case:**
- Banking statement generation
- Account consolidation
- Financial reporting

---

## Why This Matters in Production

### Space Optimization - Merge from End
```
Why merge from end to beginning?

nums1 = [1,2,3,0,0,0]  (last 3 positions empty)
nums2 = [2,5,6]

Start from end:
- Compare largest elements
- Place at end of nums1
- No overwriting of unprocessed elements
- O(1) extra space!

If we merged from start:
- Would overwrite nums1 elements
- Need O(m) extra space to save nums1
```

### Performance Comparison
```
Approach 1 - Merge from end: O(m+n) time, O(1) space ✓
Approach 2 - Extra array: O(m+n) time, O(m) space
Approach 3 - Sort after copy: O((m+n)log(m+n)) time, O(1) space

Production standard: Approach 1
```

### Real-World Scale
- **Database Systems:** Merge billions of records from shards
- **Log Aggregators:** Process millions of log entries/second
- **E-Commerce:** Combine thousands of search results
- **Social Media:** Merge feeds from hundreds of sources

---

## Interview Tip

When explaining this problem, say:

"Merge Sorted Array teaches efficient merging using the two-pointer technique. The key insight is merging from end to beginning, which eliminates the need for extra space since nums1 already has room at the end. This pattern is fundamental to distributed systems - database shards merging query results, log aggregators combining server logs, search engines merging ranked results, social media platforms combining feeds, and analytics systems merging metric streams. The O(m+n) time, O(1) space solution is production-ready and used in systems processing billions of sorted records daily."

This demonstrates understanding of both the algorithm and its critical role in distributed data processing.

---

## Key Takeaway

Merge Sorted Array is a blueprint for efficiently combining sorted datasets using the two-pointer technique. Merging from end to beginning enables O(1) space complexity, making it essential for database result aggregation, log file merging, search result combination, feed generation, metrics consolidation, and any system that needs to combine pre-sorted data efficiently.