# Valid Anagram - Real World Applications

## Problem Statement

Given two strings s and t, return true if t is an anagram of s, and false otherwise. An anagram is a word formed by rearranging the letters of another word, using all the original letters exactly once.

**Example 1:**
```
Input:  s = "anagram", t = "nagaram"
Output: true
```

**Example 2:**
```
Input:  s = "rat", t = "car"
Output: false
```

**Example 3:**
```
Input:  s = "listen", t = "silent"
Output: true
```

**Constraints:**
- 1 <= s.length, t.length <= 5 * 10^4
- s and t consist of lowercase English letters

**Solution (Java):**
```java
// Approach 1: Using HashMap
public boolean isAnagram(String s, String t) {
    if (s.length() != t.length()) {
        return false;
    }
    
    Map<Character, Integer> countMap = new HashMap<>();
    
    // Count characters in first string
    for (char c : s.toCharArray()) {
        countMap.put(c, countMap.getOrDefault(c, 0) + 1);
    }
    
    // Subtract characters from second string
    for (char c : t.toCharArray()) {
        if (!countMap.containsKey(c)) {
            return false;
        }
        countMap.put(c, countMap.get(c) - 1);
        if (countMap.get(c) < 0) {
            return false;
        }
    }
    
    return true;
}

// Approach 2: Using Array (faster for lowercase letters only)
public boolean isAnagramArray(String s, String t) {
    if (s.length() != t.length()) {
        return false;
    }
    
    int[] count = new int[26]; // for 'a' to 'z'
    
    for (int i = 0; i < s.length(); i++) {
        count[s.charAt(i) - 'a']++;
        count[t.charAt(i) - 'a']--;
    }
    
    for (int c : count) {
        if (c != 0) {
            return false;
        }
    }
    
    return true;
}

// Approach 3: Using Sorting
public boolean isAnagramSort(String s, String t) {
    if (s.length() != t.length()) {
        return false;
    }
    
    char[] sArr = s.toCharArray();
    char[] tArr = t.toCharArray();
    
    Arrays.sort(sArr);
    Arrays.sort(tArr);
    
    return Arrays.equals(sArr, tArr);
}
```

---

## Core Concept

This problem is NOT just about anagrams. It teaches:

- Character frequency counting
- Hash map for O(1) lookups
- Array as hash table for limited character sets
- String comparison techniques

Any place where you need to compare character composition, detect duplicates, or validate data similarity - this pattern applies.

---

## Real-World Use Cases

### 1. Plagiarism Detection

**Problem:** Detect if two documents have same content with words rearranged

**Example:** Content similarity checker (Backend - Java)
```java
public class PlagiarismDetector {
    
    public boolean isSimilarContent(String doc1, String doc2) {
        // Normalize: remove spaces, lowercase
        String normalized1 = normalize(doc1);
        String normalized2 = normalize(doc2);
        
        // Check if same characters (potential plagiarism)
        return isAnagram(normalized1, normalized2);
    }
    
    private String normalize(String text) {
        return text.toLowerCase()
                   .replaceAll("[^a-z]", "");
    }
    
    public double getSimilarityScore(String doc1, String doc2) {
        Map<Character, Integer> freq1 = getFrequency(doc1);
        Map<Character, Integer> freq2 = getFrequency(doc2);
        
        // Calculate similarity based on character overlap
        return calculateOverlap(freq1, freq2);
    }
}

// Detects if content is just rearranged
// Used by: Turnitin, Copyscape, Grammarly
```

**Use Case:**
- Academic plagiarism detection
- Content copyright checking
- Duplicate article detection

### 2. Spell Checker / Auto-Correct

**Problem:** Find words with same letters but different arrangement

**Example:** Word suggestion system (JavaScript)
```javascript
class SpellChecker {
    constructor(dictionary) {
        this.dictionary = dictionary;
        // Group words by sorted letters
        this.anagramMap = new Map();
        
        for (const word of dictionary) {
            const key = this.getSortedKey(word);
            if (!this.anagramMap.has(key)) {
                this.anagramMap.set(key, []);
            }
            this.anagramMap.get(key).push(word);
        }
    }
    
    getSuggestions(misspelledWord) {
        const key = this.getSortedKey(misspelledWord);
        return this.anagramMap.get(key) || [];
    }
    
    getSortedKey(word) {
        return word.toLowerCase().split('').sort().join('');
    }
}

// Misspelled: "teh" → Suggests: "the", "het"
// Misspelled: "recieve" → Suggests: "receive"
```

**Use Case:**
- Microsoft Word spell check
- Google Docs autocorrect
- Mobile keyboard suggestions

### 3. Word Games (Scrabble, Wordle, Anagrams)

**Problem:** Find valid words from given letters

**Example:** Scrabble word validator (JavaScript)
```javascript
class ScrabbleValidator {
    
    canFormWord(availableLetters, word) {
        const letterCount = new Map();
        
        // Count available letters
        for (const letter of availableLetters) {
            letterCount.set(letter, (letterCount.get(letter) || 0) + 1);
        }
        
        // Check if word can be formed
        for (const letter of word) {
            if (!letterCount.has(letter) || letterCount.get(letter) === 0) {
                return false;
            }
            letterCount.set(letter, letterCount.get(letter) - 1);
        }
        
        return true;
    }
    
    findAllValidWords(availableLetters, dictionary) {
        const validWords = [];
        
        for (const word of dictionary) {
            if (this.canFormWord(availableLetters, word)) {
                validWords.push(word);
            }
        }
        
        return validWords;
    }
}

// Letters: "CATDOG" → Can form: "CAT", "DOG", "COD", "ACT"
```

**Use Case:**
- Scrabble word validation
- Words with Friends
- Wordle helpers

### 4. Duplicate Detection in Databases

**Problem:** Find duplicate entries with slightly different text

**Example:** Customer deduplication (Backend - Java)
```java
public class CustomerDeduplicator {
    
    public boolean isPotentialDuplicate(Customer c1, Customer c2) {
        // Check if names are anagrams (typos, rearrangements)
        String name1 = normalize(c1.getName());
        String name2 = normalize(c2.getName());
        
        return isAnagram(name1, name2);
    }
    
    public List<List<Customer>> findDuplicates(List<Customer> customers) {
        Map<String, List<Customer>> groups = new HashMap<>();
        
        for (Customer customer : customers) {
            String key = getSortedName(customer.getName());
            groups.computeIfAbsent(key, k -> new ArrayList<>())
                  .add(customer);
        }
        
        // Return groups with duplicates
        return groups.values().stream()
                     .filter(list -> list.size() > 1)
                     .collect(Collectors.toList());
    }
    
    private String getSortedName(String name) {
        char[] chars = normalize(name).toCharArray();
        Arrays.sort(chars);
        return new String(chars);
    }
}

// "John Smith" vs "Smith John" → Detected as potential duplicate
// "Maria Garcia" vs "Garcia Maria" → Flagged for review
```

**Use Case:**
- CRM duplicate detection
- Email list deduplication
- Customer data cleanup

### 5. Search Optimization

**Problem:** Find documents with same keywords regardless of order

**Example:** Search index optimizer (Backend - Java)
```java
public class SearchIndexer {
    
    public String generateSearchKey(String query) {
        // Normalize and sort words for indexing
        String[] words = query.toLowerCase().split("\\s+");
        Arrays.sort(words);
        return String.join(" ", words);
    }
    
    public List<Document> findSimilarSearches(String query, 
                                              List<SearchLog> logs) {
        String queryKey = generateSearchKey(query);
        List<Document> results = new ArrayList<>();
        
        for (SearchLog log : logs) {
            String logKey = generateSearchKey(log.getQuery());
            if (queryKey.equals(logKey)) {
                // Same words, different order
                results.addAll(log.getResults());
            }
        }
        
        return results;
    }
}

// "red shoes" and "shoes red" → Same search results
// "laptop bag" and "bag laptop" → Grouped together
```

**Use Case:**
- E-commerce search
- Google search optimization
- Query suggestion systems

### 6. Social Media: Hashtag Deduplication

**Problem:** Detect duplicate hashtags with different casing/order

**Example:** Hashtag normalizer (JavaScript)
```javascript
class HashtagManager {
    
    normalizeHashtag(hashtag) {
        // Remove #, lowercase, sort characters
        return hashtag.replace('#', '')
                     .toLowerCase()
                     .split('')
                     .sort()
                     .join('');
    }
    
    findDuplicateHashtags(hashtags) {
        const groups = new Map();
        
        for (const tag of hashtags) {
            const normalized = this.normalizeHashtag(tag);
            if (!groups.has(normalized)) {
                groups.set(normalized, []);
            }
            groups.get(normalized).push(tag);
        }
        
        // Return groups with duplicates
        return Array.from(groups.values())
                    .filter(group => group.size > 1);
    }
}

// #coding, #Coding, #CODING → All same
// #javascript, #JavaScript → Grouped together
```

**Use Case:**
- Twitter/Instagram hashtag management
- Trend analysis
- Social media analytics

### 7. Translation Validation

**Problem:** Verify if translated text has same character composition

**Example:** Translation checker (JavaScript)
```javascript
class TranslationValidator {
    
    hasConsistentCharacters(original, translated) {
        // For languages using same alphabet
        // Check if character composition is preserved
        
        const origChars = this.getCharFrequency(original);
        const transChars = this.getCharFrequency(translated);
        
        return this.compareFrequencies(origChars, transChars);
    }
    
    getCharFrequency(text) {
        const freq = new Map();
        for (const char of text.toLowerCase()) {
            if (char.match(/[a-z]/)) {
                freq.set(char, (freq.get(char) || 0) + 1);
            }
        }
        return freq;
    }
}

// Useful for detecting translation errors
// Where character set should remain same
```

**Use Case:**
- Translation quality check
- Localization validation
- Subtitle verification

### 8. Password Strength: Character Diversity

**Problem:** Check if password uses diverse characters

**Example:** Password analyzer (Backend - Java)
```java
public class PasswordAnalyzer {
    
    public boolean hasDiverseCharacters(String password) {
        Set<Character> uniqueChars = new HashSet<>();
        
        for (char c : password.toCharArray()) {
            uniqueChars.add(Character.toLowerCase(c));
        }
        
        // Check diversity
        return uniqueChars.size() >= password.length() * 0.6;
    }
    
    public boolean isWeakPattern(String password, 
                                 List<String> commonPatterns) {
        String sortedPassword = sortString(password);
        
        for (String pattern : commonPatterns) {
            if (sortedPassword.equals(sortString(pattern))) {
                return true; // Weak: just rearranged common pattern
            }
        }
        
        return false;
    }
    
    private String sortString(String s) {
        char[] chars = s.toLowerCase().toCharArray();
        Arrays.sort(chars);
        return new String(chars);
    }
}

// "password123" rearranged is still weak
// Detects patterns regardless of character order
```

**Use Case:**
- Password strength validation
- Security policy enforcement
- Authentication systems

---

## Why This Matters in Production

### Performance Comparison
```java
// Approach 1: HashMap - O(n) time, O(n) space
// Works for any characters (Unicode)

// Approach 2: Array - O(n) time, O(1) space
// Only for limited character set (a-z)
// 2-3x faster than HashMap

// Approach 3: Sorting - O(n log n) time, O(1) space
// Simple but slower for large strings
```

### Real Numbers
- **Google Docs:** Checks millions of words/day for spelling
- **Turnitin:** Processes thousands of documents for plagiarism
- **Scrabble Apps:** Validates millions of words/day
- **CRM Systems:** Deduplicates millions of customer records

### Memory Efficiency
```
For lowercase letters only:
HashMap: ~1KB per comparison
Array: 26 integers = 104 bytes
Saving: ~90% memory
```

---

## Interview Tip

When explaining this problem, say:

"Valid Anagram teaches character frequency counting, which is fundamental to many real-world applications. It's used in spell checkers for word suggestions, plagiarism detection for content similarity, duplicate detection in databases, and word games like Scrabble. The pattern extends beyond anagrams - anytime you need to compare character composition regardless of order, this approach applies. For lowercase letters, using an array is faster than HashMap. For Unicode support, HashMap is necessary."

This demonstrates understanding of both the algorithm and practical trade-offs.

---

## Key Takeaway

Valid Anagram is a blueprint for character frequency analysis. The core pattern - counting character occurrences and comparing - applies to spell checking, plagiarism detection, duplicate removal, search optimization, and any domain requiring composition-based comparison regardless of order.