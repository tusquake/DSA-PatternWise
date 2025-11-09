# DSA Patterns - Interview Revision Notes

Backend development mein commonly use hone wale data structure aur algorithm patterns.

## Core Patterns

### 1. Hash Map / Hash Table

**Kya hai:** Key-value pairs store karne ka fast data structure.

**Kahan use hota hai:** Caching, session management, data indexing ke liye fast lookups.

**Real-world analogy:** Dictionary jaisa - jaise dictionary mein word search karte ho toh directly meaning mil jata hai, page by page nahi dekhna padta. Hash map bhi O(1) time mein value dedeta hai.

**Backend example:** User session data store karna - user ID se quickly session info nikalna.

---

### 2. Queue

**Kya hai:** FIFO (First In First Out) - jo pehle aaya woh pehle niklega.

**Kahan use hota hai:** Task scheduling, message queues, job processing (RabbitMQ, Kafka).

**Real-world analogy:** Bank ki line jaisa - jo customer pehle aaya, uska kaam pehle hoga. Queue mein bhi tasks順序 mein process hote hain.

**Backend example:** Email sending service - jis order mein emails queue mein aaye, usi order mein send honge.

---

### 3. Stack

**Kya hai:** LIFO (Last In First Out) - jo last mein aaya woh pehle niklega.

**Kahan use hota hai:** Function call stack, undo operations, parsing expressions.

**Real-world analogy:** Plate ki stack jaisa - upar wali plate pehle uthati ho, neeche wali nahi. Stack mein bhi last addition pehle remove hota hai.

**Backend example:** Browser history - back button pe jo last page dekha tha woh pehle aata hai.

---

### 4. Linked List

**Kya hai:** Nodes ka chain jisme har node next node ko point karta hai.

**Kahan use hota hai:** LRU cache implementation, browser history.

**Real-world analogy:** Train ki bogies jaisa - har bogie next bogie se connected hai. Koi bogie add/remove karna easy hai bina puri train disturb kiye.

**Backend example:** LRU cache - recently used items ko track karna, least used ko remove karna.

---

### 5. Tree (Binary Search Tree)

**Kya hai:** Hierarchical structure jisme har node ke left mein chhote values aur right mein bade values hoti hain.

**Kahan use hota hai:** Database indexing (B-trees, B+ trees), file systems.

**Real-world analogy:** Family tree jaisa - parent se children branch out hote hain. Search karna fast hota hai kyunki har level pe half data eliminate ho jata hai.

**Backend example:** Database index - millions of records mein bhi quickly data find karna.

---

### 6. Heap

**Kya hai:** Complete binary tree jisme parent always child se bada (max heap) ya chhota (min heap) hota hai.

**Kahan use hota hai:** Priority queues for job scheduling, finding top K elements.

**Real-world analogy:** Hospital emergency room jaisa - sabse critical patient ko pehle treat karte hain, jo pehle aaya use nahi. Priority matter karti hai.

**Backend example:** Job scheduler - high priority tasks pehle execute karna.

---

### 7. Graph

**Kya hai:** Nodes (vertices) aur unke beech connections (edges) ka network.

**Kahan use hota hai:** Social networks, recommendation systems, routing algorithms.

**Real-world analogy:** City ka road map jaisa - cities nodes hain aur roads edges hain. Ek city se dusri city tak kai routes ho sakte hain.

**Backend example:** Social network - user connections track karna, friend suggestions dena.

---

### 8. Trie

**Kya hai:** Tree structure jisme har node ek character represent karta hai, words ko prefix-wise store karta hai.

**Kahan use hota hai:** Autocomplete, IP routing, prefix matching.

**Real-world analogy:** Phone directory jaisa - jaise "S" letter ke andar "Sa", "Sh", "Si" sab grouped hote hain. Search karte time sirf relevant branch explore karni padti hai.

**Backend example:** Search box autocomplete - "goo" type karo toh "google", "good", "goose" suggestions aayenge.

---

### 9. Sliding Window

**Kya hai:** Fixed size window jo array/string pe slide karta hai aur subarray/substring analyze karta hai.

**Kahan use hota hai:** Rate limiting, moving averages, time-based analytics.

**Real-world analogy:** Train ki window se bahar dekhna jaisa - jaise jaise train aage badhti hai, window se scene change hota hai. Purane part hat jata hai, naya part add hota hai.

**Backend example:** API rate limiting - last 1 minute mein kitne requests aaye check karna.

---

### 10. Two Pointers

**Kya hai:** Do pointers use karke array/string traverse karna, usually opposite ends se ya different speeds se.

**Kahan use hota hai:** String manipulation, array processing, memory optimization.

**Real-world analogy:** Do logon ka library mein book search karna jaisa - ek left end se shuru kare aur ek right end se, beech mein mil jayenge. Faster ho jata hai.

**Backend example:** Finding pair of values in sorted array that sum to target.

---

### 11. BFS/DFS

**Kya hai:** Graph traversal techniques - BFS level by level, DFS depth first explore karta hai.

**Kahan use hota hai:** Crawling systems, dependency resolution, graph traversal.

**Real-world analogy:** 
- BFS: Pani ka failna jaisa - sabse pehle neighbors wet hote hain, phir unke neighbors
- DFS: Maze solve karna jaisa - ek path end tak follow karo, phir backtrack karke dusra try karo

**Backend example:** Web crawler - BFS se level by level pages crawl karna.

---

### 12. Binary Search

**Kya hai:** Sorted data mein middle element check karke half data eliminate karte hue search karna.

**Kahan use hota hai:** Searching sorted data, database queries, pagination.

**Real-world analogy:** Dictionary mein word search karna jaisa - beech kholo, agar word pehle hai toh left half mein dhundo, nahi toh right half mein. Har step pe half pages eliminate ho jate hain.

**Backend example:** Sorted log files mein specific timestamp ka entry find karna.

---

### 13. Dynamic Programming

**Kya hai:** Complex problem ko chhote subproblems mein todna aur results cache karna taaki repeat calculations avoid ho.

**Kahan use hota hai:** Resource allocation, optimization problems, caching strategies.

**Real-world analogy:** Recipe banate time ingredients ki list maintain karna jaisa - agar aaj cake banaya aur kal bhi banana hai toh dubara ingredients list nahi banani padegi, save hai.

**Backend example:** Price optimization - discount calculate karte time previous calculations reuse karna.

---

### 14. Sorting Algorithms

**Kya hai:** Data ko specific order mein arrange karna (ascending/descending).

**Kahan use hota hai:** Data processing, ranking systems, log analysis.

**Real-world analogy:** Playing cards ko sort karna jaisa - sabko value ke hisaab se arrange karna taaki game khelna easy ho jaye.

**Backend example:** Leaderboard system - users ko score ke hisaab se rank karna.

---

### 15. Bloom Filter

**Kya hai:** Probabilistic data structure jo efficiently check karta hai ki element set mein exist karta hai ya nahi.

**Kahan use hota hai:** Fast membership testing, caching, deduplication.

**Real-world analogy:** Security guard ka rough check jaisa - "tumhara naam list mein nahi hai" definitely sahi bata dega, but "list mein hai" ka maybe galat bhi ho sakta hai (false positive possible).

**Backend example:** Check karna ki username already taken hai ya nahi - Bloom filter fast preliminary check deta hai.

---

## Real-World Use Cases

### Caching
**Pattern:** Hash Map, LRU Cache (Linked List + Hash Map)

**Kya hota hai:** Frequently accessed data ko memory mein store karna taaki database call avoid ho.

**Example:** User profile data cache karna - bar bar database query nahi karni padti.

---

### Rate Limiting
**Pattern:** Sliding Window, Token Bucket

**Kya hota hai:** User ko limited number of requests allow karna per time window.

**Example:** API mein 100 requests per minute ki limit - isse server overload nahi hota.

---

### Load Balancing
**Pattern:** Consistent Hashing, Round Robin

**Kya hota hai:** Multiple servers ke beech traffic distribute karna.

**Example:** 3 servers hain toh har request ko turn by turn different server pe bhejenge.

---

### Job Queues
**Pattern:** Priority Queue, FIFO Queue

**Kya hota hai:** Background tasks ko queue mein dalna aur priority ya order se process karna.

**Example:** Email notifications ko queue mein dalna - important emails pehle send hongi.

---

### Search
**Pattern:** Trie, Binary Search, Inverted Index

**Kya hota hai:** Large dataset mein efficiently search karna.

**Example:** E-commerce site pe product search - Trie se autocomplete, inverted index se full-text search.

---

## Interview Tips

1. **Pattern pehchano:** Problem statement padho aur identify karo ki kaun sa pattern fit hoga

2. **Time complexity discuss karo:** Har approach ka Big O notation mention karo

3. **Trade-offs explain karo:** Space vs Time, Simplicity vs Performance

4. **Real example do:** Backend context mein practical use case batao

5. **Edge cases cover karo:** Empty input, large dataset, concurrent access

6. **Scale pe socho:** Million users ke liye solution scalable hai ya nahi

---

## Key Takeaway

Backend development mein ye patterns repeatedly aate hain. Har pattern ka:
- Core concept samajho
- Real-world analogy se relate karo
- Practical use cases yaad rakho
- Trade-offs understand karo

Interview mein problem solving approach clear honi chahiye - brute force se start karo, optimize karo, best solution tak pahuncho.
