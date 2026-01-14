# DSA Patterns - Interview Revision Notes

Common data structure and algorithm patterns used in backend development.

## Core Patterns

### 1. Hash Map / Hash Table

**What is it:** A fast data structure to store key-value pairs.

**Where it's used:** Fast lookups for caching, session management, and data indexing.

**Real-world analogy:** Like a dictionary - when you search for a word, you get the meaning directly without flipping through pages. Hash maps also return values in O(1) time.

**Backend example:** Storing user session data - quickly retrieving session info using user ID.

---

### 2. Queue

**What is it:** FIFO (First In First Out) - whoever enters first, exits first.

**Where it's used:** Task scheduling, message queues, job processing (RabbitMQ, Kafka).

**Real-world analogy:** Like a bank line - the customer who arrived first gets served first. Tasks in a queue are also processed in order.

**Backend example:** Email sending service - emails are sent in the same order they were queued.

---

### 3. Stack

**What is it:** LIFO (Last In First Out) - whoever enters last, exits first.

**Where it's used:** Function call stack, undo operations, parsing expressions.

**Real-world analogy:** Like a stack of plates - you pick the top plate first, not the bottom one. The last addition is removed first.

**Backend example:** Browser history - pressing the back button shows the last page you visited.

---

### 4. Linked List

**What is it:** A chain of nodes where each node points to the next node.

**Where it's used:** LRU cache implementation, browser history.

**Real-world analogy:** Like train coaches - each coach is connected to the next one. You can easily add or remove coaches without disturbing the entire train.

**Backend example:** LRU cache - tracking recently used items and removing the least used ones.

---

### 5. Tree (Binary Search Tree)

**What is it:** A hierarchical structure where each node has smaller values on the left and larger values on the right.

**Where it's used:** Database indexing (B-trees, B+ trees), file systems.

**Real-world analogy:** Like a family tree - children branch out from parents. Searching is fast because at each level, half the data gets eliminated.

**Backend example:** Database index - quickly finding data even in millions of records.

---

### 6. Heap

**What is it:** A complete binary tree where the parent is always larger (max heap) or smaller (min heap) than its children.

**Where it's used:** Priority queues for job scheduling, finding top K elements.

**Real-world analogy:** Like a hospital emergency room - the most critical patient gets treated first, not the one who arrived first. Priority matters.

**Backend example:** Job scheduler - executing high priority tasks first.

---

### 7. Graph

**What is it:** A network of nodes (vertices) and connections (edges) between them.

**Where it's used:** Social networks, recommendation systems, routing algorithms.

**Real-world analogy:** Like a city road map - cities are nodes and roads are edges. Multiple routes can exist between two cities.

**Backend example:** Social network - tracking user connections and suggesting friends.

---

### 8. Trie

**What is it:** A tree structure where each node represents a character, storing words prefix-wise.

**Where it's used:** Autocomplete, IP routing, prefix matching.

**Real-world analogy:** Like a phone directory - under letter "S", you have "Sa", "Sh", "Si" all grouped. While searching, you only explore the relevant branch.

**Backend example:** Search box autocomplete - typing "goo" suggests "google", "good", "goose".

---

### 9. Sliding Window

**What is it:** A fixed-size window that slides over an array/string to analyze subarrays/substrings.

**Where it's used:** Rate limiting, moving averages, time-based analytics.

**Real-world analogy:** Like looking out of a train window - as the train moves forward, the scene changes. The old part disappears, and a new part appears.

**Backend example:** API rate limiting - checking how many requests came in the last 1 minute.

---

### 10. Two Pointers

**What is it:** Using two pointers to traverse an array/string, usually from opposite ends or at different speeds.

**Where it's used:** String manipulation, array processing, memory optimization.

**Real-world analogy:** Like two people searching for a book in a library - one starts from the left end, one from the right end, they meet in the middle. It's faster.

**Backend example:** Finding a pair of values in a sorted array that sum to a target.

---

### 11. BFS/DFS

**What is it:** Graph traversal techniques - BFS explores level by level, DFS explores depth first.

**Where it's used:** Crawling systems, dependency resolution, graph traversal.

**Real-world analogy:** 
- BFS: Like water spreading - neighbors get wet first, then their neighbors
- DFS: Like solving a maze - follow one path to the end, then backtrack and try another

**Backend example:** Web crawler - crawling pages level by level using BFS.

---

### 12. Binary Search

**What is it:** Searching in sorted data by checking the middle element and eliminating half the data.

**Where it's used:** Searching sorted data, database queries, pagination.

**Real-world analogy:** Like searching for a word in a dictionary - open to the middle, if the word is earlier then search the left half, otherwise the right half. Half the pages get eliminated at each step.

**Backend example:** Finding a specific timestamp entry in sorted log files.

---

### 13. Dynamic Programming

**What is it:** Breaking a complex problem into smaller subproblems and caching results to avoid repeat calculations.

**Where it's used:** Resource allocation, optimization problems, caching strategies.

**Real-world analogy:** Like maintaining an ingredients list while cooking - if you made a cake today and need to make one tomorrow, you don't have to create the ingredients list again, it's saved.

**Backend example:** Price optimization - reusing previous calculations when calculating discounts.

---

### 14. Sorting Algorithms

**What is it:** Arranging data in a specific order (ascending/descending).

**Where it's used:** Data processing, ranking systems, log analysis.

**Real-world analogy:** Like sorting playing cards - arranging all cards by value so the game is easier to play.

**Backend example:** Leaderboard system - ranking users by their scores.

---

### 15. Bloom Filter

**What is it:** A probabilistic data structure that efficiently checks if an element exists in a set.

**Where it's used:** Fast membership testing, caching, deduplication.

**Real-world analogy:** Like a security guard's rough check - "your name is NOT on the list" is definitely correct, but "it IS on the list" might be wrong (false positives are possible).

**Backend example:** Checking if a username is already taken - Bloom filter provides a fast preliminary check.

---

## Real-World Use Cases

### Caching
**Pattern:** Hash Map, LRU Cache (Linked List + Hash Map)

**What happens:** Frequently accessed data is stored in memory to avoid database calls.

**Example:** Caching user profile data - no need to query the database repeatedly.

---

### Rate Limiting
**Pattern:** Sliding Window, Token Bucket

**What happens:** Allowing users a limited number of requests per time window.

**Example:** API limit of 100 requests per minute - prevents server overload.

---

### Load Balancing
**Pattern:** Consistent Hashing, Round Robin

**What happens:** Distributing traffic across multiple servers.

**Example:** If there are 3 servers, each request goes to a different server in turns.

---

### Job Queues
**Pattern:** Priority Queue, FIFO Queue

**What happens:** Placing background tasks in a queue and processing them by priority or order.

**Example:** Queuing email notifications - important emails are sent first.

---

### Search
**Pattern:** Trie, Binary Search, Inverted Index

**What happens:** Efficiently searching in large datasets.

**Example:** Product search on e-commerce sites - Trie for autocomplete, inverted index for full-text search.

---

## Interview Tips

1. **Identify the pattern:** Read the problem statement and identify which pattern fits

2. **Discuss time complexity:** Mention the Big O notation for each approach

3. **Explain trade-offs:** Space vs Time, Simplicity vs Performance

4. **Give real examples:** Provide practical use cases in a backend context

5. **Cover edge cases:** Empty input, large datasets, concurrent access

6. **Think about scale:** Is the solution scalable for millions of users?

---

## Key Takeaway

These patterns appear repeatedly in backend development. For each pattern:
- Understand the core concept
- Relate it to real-world analogies
- Remember practical use cases
- Understand the trade-offs
