# Reverse Linked List - Real World Applications

## Problem Statement

Given the head of a singly linked list, reverse the list, and return the reversed list.

**Example 1:**
```
Input:  head = [1,2,3,4,5]
Output: [5,4,3,2,1]
```

**Example 2:**
```
Input:  head = [1,2]
Output: [2,1]
```

**Example 3:**
```
Input:  head = []
Output: []
```

**Constraints:**
- The number of nodes in the list is in the range [0, 5000]
- -5000 <= Node.val <= 5000

**Solution (Java):**
```java
class ListNode {
    int val;
    ListNode next;
    ListNode(int val) { this.val = val; }
}

// Approach 1: Iterative (Most common)
// Time: O(n), Space: O(1)
public ListNode reverseList(ListNode head) {
    ListNode prev = null;
    ListNode current = head;
    
    while (current != null) {
        ListNode nextTemp = current.next; // Save next node
        current.next = prev;              // Reverse link
        prev = current;                   // Move prev forward
        current = nextTemp;               // Move current forward
    }
    
    return prev; // New head
}

// Approach 2: Recursive
// Time: O(n), Space: O(n) due to call stack
public ListNode reverseListRecursive(ListNode head) {
    // Base case
    if (head == null || head.next == null) {
        return head;
    }
    
    // Recursive call
    ListNode newHead = reverseListRecursive(head.next);
    
    // Reverse the link
    head.next.next = head;
    head.next = null;
    
    return newHead;
}

// Approach 3: Using Stack
// Time: O(n), Space: O(n)
public ListNode reverseListStack(ListNode head) {
    if (head == null) return null;
    
    Stack<ListNode> stack = new Stack<>();
    ListNode current = head;
    
    // Push all nodes to stack
    while (current != null) {
        stack.push(current);
        current = current.next;
    }
    
    // Pop and rebuild list
    ListNode newHead = stack.pop();
    current = newHead;
    
    while (!stack.isEmpty()) {
        current.next = stack.pop();
        current = current.next;
    }
    
    current.next = null; // Important: terminate list
    return newHead;
}
```

---

## Core Concept

This problem is NOT just about reversing lists. It teaches:

- Pointer manipulation
- In-place data structure modification
- Iterator pattern
- State management without extra space

Any place where you need to reverse order, undo operations, or navigate backwards through sequential data - this pattern applies.

---

## Real-World Use Cases

### 1. Browser History: Back Button Navigation

**Problem:** Navigate backward through browser history

**Example:** Browser history manager (JavaScript)
```javascript
class BrowserHistory {
    constructor() {
        this.head = null;
        this.current = null;
    }
    
    visit(url) {
        const newPage = { url: url, next: null };
        
        if (!this.head) {
            this.head = newPage;
            this.current = newPage;
        } else {
            this.current.next = newPage;
            this.current = newPage;
        }
    }
    
    reverseHistory() {
        // Reverse to enable back navigation
        let prev = null;
        let current = this.head;
        
        while (current) {
            let nextTemp = current.next;
            current.next = prev;
            prev = current;
            current = nextTemp;
        }
        
        return prev; // Reversed history
    }
    
    goBack() {
        // Navigate backward through reversed list
        if (this.current && this.current.next) {
            this.current = this.current.next;
            return this.current.url;
        }
        return null;
    }
}

// Chrome, Firefox use similar structure for back button
```

**Use Case:**
- Web browser navigation
- Back button functionality
- History management

### 2. Text Editor: Undo Operations

**Problem:** Implement undo functionality by reversing operation chain

**Example:** Undo manager (Backend - Java)
```java
public class UndoManager {
    class Operation {
        String action;
        String data;
        Operation next;
        
        Operation(String action, String data) {
            this.action = action;
            this.data = data;
        }
    }
    
    private Operation head;
    
    public void recordOperation(String action, String data) {
        Operation newOp = new Operation(action, data);
        newOp.next = head;
        head = newOp;
    }
    
    public Operation reverseOperations() {
        // Reverse to undo in correct order
        Operation prev = null;
        Operation current = head;
        
        while (current != null) {
            Operation nextTemp = current.next;
            current.next = prev;
            prev = current;
            current = nextTemp;
        }
        
        return prev;
    }
    
    public void undo() {
        if (head != null) {
            System.out.println("Undoing: " + head.action);
            head = head.next;
        }
    }
}

// Microsoft Word, Google Docs use operation chains
```

**Use Case:**
- Text editors (Word, Notepad++)
- IDE undo functionality
- Photoshop history

### 3. Music Player: Reverse Playlist

**Problem:** Play playlist in reverse order

**Example:** Playlist reverser (JavaScript)
```javascript
class PlaylistManager {
    constructor() {
        this.head = null;
    }
    
    addSong(song) {
        const newNode = { song: song, next: this.head };
        this.head = newNode;
    }
    
    reversePlaylist() {
        let prev = null;
        let current = this.head;
        
        while (current) {
            let nextTemp = current.next;
            current.next = prev;
            prev = current;
            current = nextTemp;
        }
        
        this.head = prev;
        return this.head;
    }
    
    playInReverse() {
        this.reversePlaylist();
        let current = this.head;
        
        while (current) {
            console.log("Playing: " + current.song);
            current = current.next;
        }
    }
}

// Spotify, Apple Music reverse playback
```

**Use Case:**
- Music streaming apps
- Playlist management
- Queue reversal

### 4. Transaction Log: Rollback Operations

**Problem:** Reverse transaction chain for rollback

**Example:** Transaction manager (Backend - Java)
```java
public class TransactionManager {
    class Transaction {
        String id;
        String operation;
        Transaction next;
        
        Transaction(String id, String operation) {
            this.id = id;
            this.operation = operation;
        }
    }
    
    private Transaction head;
    
    public void executeTransaction(String id, String operation) {
        Transaction txn = new Transaction(id, operation);
        txn.next = head;
        head = txn;
    }
    
    public void rollback() {
        // Reverse transactions to undo in correct order
        Transaction reversed = reverseTransactions(head);
        
        Transaction current = reversed;
        while (current != null) {
            System.out.println("Rolling back: " + current.id);
            // Execute reverse operation
            current = current.next;
        }
    }
    
    private Transaction reverseTransactions(Transaction head) {
        Transaction prev = null;
        Transaction current = head;
        
        while (current != null) {
            Transaction nextTemp = current.next;
            current.next = prev;
            prev = current;
            current = nextTemp;
        }
        
        return prev;
    }
}

// Database systems use for transaction rollback
```

**Use Case:**
- Database rollbacks
- Financial transaction reversal
- ACID compliance

### 5. Social Media: Feed Reversal

**Problem:** Show posts in reverse chronological order

**Example:** Social feed manager (JavaScript)
```javascript
class SocialFeed {
    constructor() {
        this.posts = null;
    }
    
    addPost(post) {
        const newPost = { 
            content: post, 
            timestamp: Date.now(),
            next: this.posts 
        };
        this.posts = newPost;
    }
    
    showNewestFirst() {
        // Posts naturally stored newest first
        return this.posts;
    }
    
    showOldestFirst() {
        // Reverse to show oldest first
        let prev = null;
        let current = this.posts;
        
        while (current) {
            let nextTemp = current.next;
            current.next = prev;
            prev = current;
            current = nextTemp;
        }
        
        return prev;
    }
}

// Twitter, Facebook timeline ordering
```

**Use Case:**
- Social media feeds
- Timeline display
- Chronological sorting

### 6. Email Client: Thread Reversal

**Problem:** Display email threads in different orders

**Example:** Email thread manager (Backend - Java)
```java
public class EmailThreadManager {
    class Email {
        String subject;
        String body;
        long timestamp;
        Email next;
        
        Email(String subject, String body, long timestamp) {
            this.subject = subject;
            this.body = body;
            this.timestamp = timestamp;
        }
    }
    
    private Email threadHead;
    
    public void addToThread(String subject, String body) {
        Email newEmail = new Email(subject, body, System.currentTimeMillis());
        newEmail.next = threadHead;
        threadHead = newEmail;
    }
    
    public Email reverseThread() {
        // Show oldest first (reversed)
        Email prev = null;
        Email current = threadHead;
        
        while (current != null) {
            Email nextTemp = current.next;
            current.next = prev;
            prev = current;
            current = nextTemp;
        }
        
        return prev;
    }
    
    public void displayThread(boolean oldestFirst) {
        Email display = oldestFirst ? reverseThread() : threadHead;
        
        while (display != null) {
            System.out.println(display.subject + " - " + display.body);
            display = display.next;
        }
    }
}

// Gmail, Outlook thread display
```

**Use Case:**
- Email threading
- Conversation display
- Message ordering

### 7. File System: Directory Navigation

**Problem:** Navigate backward through directory path

**Example:** Directory navigator (JavaScript)
```javascript
class DirectoryNavigator {
    constructor() {
        this.currentPath = null;
    }
    
    navigate(directory) {
        const newDir = { 
            name: directory, 
            next: this.currentPath 
        };
        this.currentPath = newDir;
    }
    
    goBack() {
        if (this.currentPath) {
            this.currentPath = this.currentPath.next;
        }
    }
    
    getReversePath() {
        // Get path from root to current
        let prev = null;
        let current = this.currentPath;
        
        while (current) {
            let nextTemp = current.next;
            current.next = prev;
            prev = current;
            current = nextTemp;
        }
        
        return prev;
    }
    
    getFullPath() {
        const reversed = this.getReversePath();
        let path = "/";
        let current = reversed;
        
        while (current) {
            path += current.name + "/";
            current = current.next;
        }
        
        return path;
    }
}

// File explorers use for path navigation
```

**Use Case:**
- File system navigation
- Breadcrumb trails
- Path traversal

### 8. Game Development: Move History

**Problem:** Reverse move history for replay or undo

**Example:** Chess move tracker (Backend - Java)
```java
public class ChessMoveTracker {
    class Move {
        String piece;
        String from;
        String to;
        Move next;
        
        Move(String piece, String from, String to) {
            this.piece = piece;
            this.from = from;
            this.to = to;
        }
    }
    
    private Move moveHistory;
    
    public void recordMove(String piece, String from, String to) {
        Move newMove = new Move(piece, from, to);
        newMove.next = moveHistory;
        moveHistory = newMove;
    }
    
    public Move reverseMoves() {
        // Reverse for replay from start
        Move prev = null;
        Move current = moveHistory;
        
        while (current != null) {
            Move nextTemp = current.next;
            current.next = prev;
            prev = current;
            current = nextTemp;
        }
        
        return prev;
    }
    
    public void replayGame() {
        Move reversed = reverseMoves();
        Move current = reversed;
        
        while (current != null) {
            System.out.println(current.piece + ": " + 
                             current.from + " -> " + current.to);
            current = current.next;
        }
    }
}

// Chess.com, Lichess use for game replay
```

**Use Case:**
- Game replays
- Move undo functionality
- Game analysis

### 9. Printing: Reverse Print Queue

**Problem:** Print documents in reverse order

**Example:** Print queue manager (JavaScript)
```javascript
class PrintQueue {
    constructor() {
        this.queue = null;
    }
    
    addDocument(document) {
        const newDoc = { 
            name: document, 
            next: this.queue 
        };
        this.queue = newDoc;
    }
    
    reversePrintOrder() {
        let prev = null;
        let current = this.queue;
        
        while (current) {
            let nextTemp = current.next;
            current.next = prev;
            prev = current;
            current = nextTemp;
        }
        
        this.queue = prev;
    }
    
    printAll() {
        let current = this.queue;
        
        while (current) {
            console.log("Printing: " + current.name);
            current = current.next;
        }
    }
}

// Windows print spooler, CUPS
```

**Use Case:**
- Print queue management
- Job scheduling
- Task ordering

### 10. Blockchain: Block Chain Traversal

**Problem:** Traverse blockchain in reverse (from latest to genesis)

**Example:** Blockchain navigator (Backend - Java)
```java
public class BlockchainNavigator {
    class Block {
        int index;
        String hash;
        String data;
        Block next;
        
        Block(int index, String hash, String data) {
            this.index = index;
            this.hash = hash;
            this.data = data;
        }
    }
    
    private Block latestBlock;
    
    public void addBlock(int index, String hash, String data) {
        Block newBlock = new Block(index, hash, data);
        newBlock.next = latestBlock;
        latestBlock = newBlock;
    }
    
    public Block traverseToGenesis() {
        // Reverse to go from genesis to latest
        Block prev = null;
        Block current = latestBlock;
        
        while (current != null) {
            Block nextTemp = current.next;
            current.next = prev;
            prev = current;
            current = nextTemp;
        }
        
        return prev; // Genesis block
    }
    
    public void verifyChain() {
        Block genesis = traverseToGenesis();
        Block current = genesis;
        
        while (current != null) {
            System.out.println("Block " + current.index + ": " + current.hash);
            current = current.next;
        }
    }
}

// Bitcoin, Ethereum blockchain traversal
```

**Use Case:**
- Blockchain validation
- Transaction verification
- Block history

---

## Why This Matters in Production

### Memory Efficiency
```
In-place reversal:
- Space: O(1) - only 3 pointers (prev, current, next)
- No extra data structure needed

Using array/list:
- Space: O(n) - copy entire list
- Garbage collection overhead
```

### Performance
```
Iterative approach:
- Time: O(n) - single pass
- Space: O(1)
- Production standard

Recursive approach:
- Time: O(n)
- Space: O(n) - call stack
- Avoid in production for large lists
```

### Real-World Usage
- **Browsers:** Millions of back button clicks per day
- **Editors:** Billions of undo operations
- **Music Apps:** Playlist reversals
- **Blockchain:** Constant block traversal

---

## Interview Tip

When explaining this problem, say:

"Reverse Linked List is fundamental to systems requiring backward navigation or undo functionality. The iterative three-pointer approach (prev, current, next) reverses the list in-place with O(1) space, making it production-ready. This pattern appears in browser history (back button), text editors (undo), music players (reverse playback), transaction rollbacks, and blockchain traversal. The key is understanding pointer manipulation - we reverse each link one at a time while maintaining references to previous and next nodes."

This demonstrates understanding of both the algorithm and pointer manipulation in real systems.

---

## Key Takeaway

Reverse Linked List is the foundation of backward navigation and undo systems. The in-place reversal using three pointers (prev, current, next) is memory-efficient and used in browsers, editors, music apps, transaction systems, and blockchains - anywhere sequential data needs to be traversed or processed in reverse order.