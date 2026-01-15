# Contains Duplicate - Real World Applications

## Problem Statement

Given an integer array nums, return true if any value appears at least twice in the array, and return false if every element is distinct.

**Example 1:**
```
Input:  nums = [1,2,3,1]
Output: true
```

**Example 2:**
```
Input:  nums = [1,2,3,4]
Output: false
```

**Example 3:**
```
Input:  nums = [1,1,1,3,3,4,3,2,4,2]
Output: true
```

**Constraints:**
- 1 <= nums.length <= 10^5
- -10^9 <= nums[i] <= 10^9

**Solution (Java):**
```java
// Approach 1: Using HashSet - Most efficient
// Time: O(n), Space: O(n)
public boolean containsDuplicate(int[] nums) {
    Set<Integer> seen = new HashSet<>();
    
    for (int num : nums) {
        if (seen.contains(num)) {
            return true; // Duplicate found
        }
        seen.add(num);
    }
    
    return false; // No duplicates
}

// Approach 2: Using Sorting
// Time: O(n log n), Space: O(1)
public boolean containsDuplicateSort(int[] nums) {
    Arrays.sort(nums);
    
    for (int i = 1; i < nums.length; i++) {
        if (nums[i] == nums[i - 1]) {
            return true; // Adjacent duplicates found
        }
    }
    
    return false;
}

// Approach 3: Using HashMap (with frequency count)
// Time: O(n), Space: O(n)
public boolean containsDuplicateMap(int[] nums) {
    Map<Integer, Integer> frequency = new HashMap<>();
    
    for (int num : nums) {
        frequency.put(num, frequency.getOrDefault(num, 0) + 1);
        if (frequency.get(num) > 1) {
            return true;
        }
    }
    
    return false;
}
```

---

## Core Concept

This problem is NOT just about duplicates. It teaches:

- HashSet for O(1) membership checking
- Trading space for time efficiency
- Early termination optimization
- Data validation patterns

Any place where you need to validate uniqueness, prevent duplicates, or ensure data integrity - this pattern applies.

---

## Real-World Use Cases

### 1. User Registration: Duplicate Username/Email Detection

**Problem:** Check if username or email already exists before registration

**Example:** Registration validator (Backend - Java)
```java
public class UserRegistrationService {
    private Set<String> existingUsernames = new HashSet<>();
    private Set<String> existingEmails = new HashSet<>();
    
    public boolean isUsernameAvailable(String username) {
        return !existingUsernames.contains(username);
    }
    
    public boolean isEmailAvailable(String email) {
        return !existingEmails.contains(email);
    }
    
    public boolean registerUser(String username, String email) {
        // Check for duplicates
        if (existingUsernames.contains(username)) {
            throw new Exception("Username already taken");
        }
        
        if (existingEmails.contains(email)) {
            throw new Exception("Email already registered");
        }
        
        // Register user
        existingUsernames.add(username);
        existingEmails.add(email);
        return true;
    }
}

// Instant feedback: "Username already taken"
// Used by: Facebook, Twitter, Gmail, etc.
```

**Use Case:**
- Social media registration
- Email account creation
- Username validation

### 2. E-Commerce: Shopping Cart Duplicate Prevention

**Problem:** Prevent adding same product twice to cart

**Example:** Shopping cart manager (JavaScript)
```javascript
class ShoppingCart {
    constructor() {
        this.items = [];
        this.productIds = new Set();
    }
    
    addItem(product) {
        // Check for duplicate
        if (this.productIds.has(product.id)) {
            // Update quantity instead of adding duplicate
            this.updateQuantity(product.id, 1);
            return false;
        }
        
        this.items.push(product);
        this.productIds.add(product.id);
        return true;
    }
    
    hasDuplicates() {
        const ids = this.items.map(item => item.id);
        const uniqueIds = new Set(ids);
        return ids.length !== uniqueIds.size;
    }
}

// Prevents duplicate products in cart
// Amazon, Flipkart use similar logic
```

**Use Case:**
- Shopping cart management
- Order validation
- Wishlist deduplication

### 3. Event Management: Duplicate Ticket Prevention

**Problem:** Ensure no duplicate ticket sales

**Example:** Ticket booking system (Backend - Java)
```java
public class TicketBookingSystem {
    private Set<String> bookedSeats = new HashSet<>();
    
    public boolean bookSeat(String seatNumber) {
        // Check if seat already booked
        if (bookedSeats.contains(seatNumber)) {
            System.out.println("Seat " + seatNumber + " already booked");
            return false;
        }
        
        // Book the seat
        bookedSeats.add(seatNumber);
        System.out.println("Seat " + seatNumber + " booked successfully");
        return true;
    }
    
    public boolean hasDuplicateBookings(List<String> bookingRequests) {
        Set<String> seen = new HashSet<>();
        
        for (String seat : bookingRequests) {
            if (seen.contains(seat)) {
                return true; // Duplicate booking attempt
            }
            seen.add(seat);
        }
        
        return false;
    }
}

// Prevents double-booking
// Used by: BookMyShow, Ticketmaster, airlines
```

**Use Case:**
- Movie ticket booking
- Flight seat reservation
- Concert ticket sales

### 4. File Upload: Duplicate File Detection

**Problem:** Prevent uploading same file twice

**Example:** File upload handler (JavaScript)
```javascript
class FileUploadManager {
    constructor() {
        this.uploadedFiles = new Set();
    }
    
    canUpload(file) {
        const fileHash = this.generateFileHash(file);
        
        if (this.uploadedFiles.has(fileHash)) {
            console.log("File already uploaded");
            return false;
        }
        
        return true;
    }
    
    uploadFile(file) {
        const fileHash = this.generateFileHash(file);
        
        if (this.uploadedFiles.has(fileHash)) {
            throw new Error("Duplicate file - already uploaded");
        }
        
        // Upload file
        this.uploadedFiles.add(fileHash);
        return { success: true, hash: fileHash };
    }
    
    generateFileHash(file) {
        // Simplified - in real world use MD5/SHA256
        return `${file.name}_${file.size}_${file.lastModified}`;
    }
}

// Google Drive, Dropbox detect duplicate uploads
```

**Use Case:**
- Cloud storage (Google Drive, Dropbox)
- Document management systems
- Photo backup services

### 5. Database: Unique Constraint Validation

**Problem:** Validate unique constraints before INSERT

**Example:** Database validator (Backend - Java)
```java
public class DatabaseValidator {
    
    public boolean hasUniqueValues(List<Integer> columnValues) {
        Set<Integer> seen = new HashSet<>();
        
        for (Integer value : columnValues) {
            if (seen.contains(value)) {
                return false; // Duplicate found
            }
            seen.add(value);
        }
        
        return true; // All unique
    }
    
    public void validateBeforeInsert(List<Record> records) {
        Set<String> primaryKeys = new HashSet<>();
        
        for (Record record : records) {
            if (primaryKeys.contains(record.getId())) {
                throw new SQLException("Duplicate primary key: " + record.getId());
            }
            primaryKeys.add(record.getId());
        }
    }
}

// Validates data before database insert
// Prevents constraint violation errors
```

**Use Case:**
- SQL unique constraints
- Primary key validation
- Data import validation

### 6. Attendance System: Duplicate Entry Prevention

**Problem:** Prevent marking attendance twice

**Example:** Attendance tracker (JavaScript)
```javascript
class AttendanceSystem {
    constructor() {
        this.presentToday = new Set();
    }
    
    markAttendance(employeeId) {
        if (this.presentToday.has(employeeId)) {
            console.log("Attendance already marked for employee " + employeeId);
            return false;
        }
        
        this.presentToday.add(employeeId);
        console.log("Attendance marked successfully");
        return true;
    }
    
    hasDuplicateEntries(attendanceList) {
        const seen = new Set();
        
        for (const employeeId of attendanceList) {
            if (seen.has(employeeId)) {
                return true; // Duplicate attendance
            }
            seen.add(employeeId);
        }
        
        return false;
    }
    
    resetDaily() {
        this.presentToday.clear();
    }
}

// Prevents attendance fraud
// Used in schools, offices, factories
```

**Use Case:**
- Employee attendance systems
- School attendance
- Visitor check-in systems

### 7. Social Media: Duplicate Post Prevention

**Problem:** Prevent posting same content multiple times

**Example:** Post validator (Backend - Java)
```java
public class PostValidator {
    private Set<String> recentPosts = new HashSet<>();
    
    public boolean isDuplicatePost(String postContent) {
        String postHash = generateHash(postContent);
        return recentPosts.contains(postHash);
    }
    
    public boolean canPost(String content, String userId) {
        String postKey = userId + ":" + generateHash(content);
        
        if (recentPosts.contains(postKey)) {
            System.out.println("Duplicate post detected - already published");
            return false;
        }
        
        recentPosts.add(postKey);
        return true;
    }
    
    private String generateHash(String content) {
        // Simplified - use proper hash function in production
        return String.valueOf(content.hashCode());
    }
}

// Prevents spam and duplicate posts
// Twitter, Facebook use similar detection
```

**Use Case:**
- Social media platforms
- Blog publishing
- Forum post validation

### 8. Payment: Duplicate Transaction Prevention

**Problem:** Prevent processing same payment twice (idempotency)

**Example:** Payment processor (Backend - Java)
```java
public class PaymentProcessor {
    private Set<String> processedTransactions = new HashSet<>();
    
    public PaymentResult processPayment(String transactionId, 
                                       double amount) {
        // Check for duplicate transaction
        if (processedTransactions.contains(transactionId)) {
            return new PaymentResult(
                false, 
                "Duplicate transaction - already processed"
            );
        }
        
        // Process payment
        boolean success = chargeCard(amount);
        
        if (success) {
            processedTransactions.add(transactionId);
        }
        
        return new PaymentResult(success, "Payment processed");
    }
    
    public boolean hasDuplicateTransactions(List<String> transactions) {
        Set<String> seen = new HashSet<>();
        
        for (String txnId : transactions) {
            if (seen.contains(txnId)) {
                return true; // Duplicate transaction
            }
            seen.add(txnId);
        }
        
        return false;
    }
}

// Prevents double-charging customers
// Stripe, PayPal, Razorpay use this
```

**Use Case:**
- Payment gateways
- Financial transactions
- Billing systems

### 9. URL Shortener: Duplicate URL Detection

**Problem:** Check if URL already shortened before creating new short link

**Example:** URL shortener (JavaScript)
```javascript
class URLShortener {
    constructor() {
        this.urlMap = new Map(); // short -> long
        this.existingURLs = new Set(); // all long URLs
    }
    
    shortenURL(longURL) {
        // Check if already shortened
        if (this.existingURLs.has(longURL)) {
            console.log("URL already shortened");
            return this.findExistingShortURL(longURL);
        }
        
        const shortURL = this.generateShortURL();
        this.urlMap.set(shortURL, longURL);
        this.existingURLs.add(longURL);
        
        return shortURL;
    }
    
    hasDuplicateURLs(urls) {
        const seen = new Set();
        
        for (const url of urls) {
            if (seen.has(url)) {
                return true;
            }
            seen.add(url);
        }
        
        return false;
    }
}

// bit.ly, tinyurl.com use similar logic
```

**Use Case:**
- URL shortening services
- Link tracking
- Marketing campaigns

### 10. Playlist: Duplicate Song Detection

**Problem:** Prevent adding same song twice to playlist

**Example:** Music playlist manager (JavaScript)
```javascript
class PlaylistManager {
    constructor() {
        this.songs = [];
        this.songIds = new Set();
    }
    
    addSong(song) {
        if (this.songIds.has(song.id)) {
            console.log("Song already in playlist");
            return false;
        }
        
        this.songs.push(song);
        this.songIds.add(song.id);
        return true;
    }
    
    hasDuplicateSongs() {
        const ids = this.songs.map(s => s.id);
        const uniqueIds = new Set(ids);
        return ids.length !== uniqueIds.size;
    }
}

// Spotify, Apple Music prevent duplicate songs
```

**Use Case:**
- Music streaming apps
- Playlist management
- Video queue systems

---

## Why This Matters in Production

### Performance Comparison
```java
// Approach 1: HashSet - O(n) time, O(n) space
// Best for most cases
Set<Integer> seen = new HashSet<>();
for (int num : nums) {
    if (seen.contains(num)) return true;
    seen.add(num);
}

// Approach 2: Sorting - O(n log n) time, O(1) space
// Better when memory is limited
Arrays.sort(nums);
for (int i = 1; i < nums.length; i++) {
    if (nums[i] == nums[i-1]) return true;
}
```

### Real-World Performance
- **Facebook:** Checks millions of usernames/day for duplicates
- **Amazon:** Validates millions of cart additions
- **Stripe:** Prevents billions of duplicate transactions
- **Netflix:** Ensures no duplicate content in queues

### Early Termination Benefit
```
Dataset: 1 million items, duplicate at position 100

Without early termination: Check all 1 million
With early termination: Stop at 100

99.99% performance improvement!
```

---

## Interview Tip

When explaining this problem, say:

"Contains Duplicate is fundamental to data validation across all systems. While simple, it appears everywhere - user registration to prevent duplicate usernames, e-commerce to prevent duplicate cart items, payment systems to prevent double-charging, and file uploads to detect duplicates. The HashSet approach gives O(n) time with O(1) lookup, making it ideal for real-time validation. The early termination optimization is crucial - we return immediately when finding a duplicate rather than checking everything. This pattern is used billions of times daily across systems like Facebook, Amazon, Stripe, and Google."

This demonstrates understanding of both the algorithm and its ubiquitous real-world importance.

---

## Key Takeaway

Contains Duplicate is the foundation of data validation in production systems. The HashSet pattern with early termination enables instant duplicate detection across user registration, transaction processing, file uploads, and any system requiring uniqueness constraints. Simple but used billions of times daily in every major application.