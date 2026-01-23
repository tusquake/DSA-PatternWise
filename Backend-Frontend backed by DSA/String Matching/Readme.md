# String Matching (strStr) - Real World Applications

## Problem Statement

Implement `strStr()` which returns the index of the first occurrence of `needle` in `haystack`, or `-1` if `needle` is not part of `haystack`.

**Clarification:**
What should we return when `needle` is an empty string? For the purpose of this problem, we shall return 0 when `needle` is an empty string. This is consistent with C's `strstr()` and Java's `indexOf()`.

**Example 1:**
```
Input:  haystack = "hello", needle = "ll"
Output: 2
```

**Example 2:**
```
Input:  haystack = "aaaaa", needle = "bba"
Output: -1
```

**Example 3:**
```
Input:  haystack = "mississippi", needle = "issip"
Output: 4
```

**Constraints:**
- 0 <= haystack.length, needle.length <= 5 * 10^4
- haystack and needle consist of only lowercase English characters

**Solution (Java):**
```java
// Approach 1: Brute Force (Simple Substring)
// Time: O(n*m), Space: O(1)
public int strStr(String haystack, String needle) {
    if (needle.isEmpty()) return 0;
    
    for (int i = 0, j = needle.length(); j <= haystack.length(); i++, j++) {
        if (haystack.substring(i, j).equals(needle)) {
            return i;
        }
    }
    
    return -1;
}

// Approach 2: Character-by-Character Comparison (More Efficient)
// Time: O(n*m), Space: O(1)
public int strStrOptimized(String haystack, String needle) {
    if (needle.isEmpty()) return 0;
    
    int n = haystack.length();
    int m = needle.length();
    
    if (m > n) return -1;
    
    for (int i = 0; i <= n - m; i++) {
        int j = 0;
        
        // Check if needle matches starting at position i
        while (j < m && haystack.charAt(i + j) == needle.charAt(j)) {
            j++;
        }
        
        if (j == m) {
            return i; // Found match
        }
    }
    
    return -1;
}

// Approach 3: Two Pointer
// Time: O(n*m), Space: O(1)
public int strStrTwoPointer(String haystack, String needle) {
    if (needle.isEmpty()) return 0;
    
    int n = haystack.length();
    int m = needle.length();
    
    for (int i = 0; i <= n - m; i++) {
        boolean found = true;
        
        for (int j = 0; j < m; j++) {
            if (haystack.charAt(i + j) != needle.charAt(j)) {
                found = false;
                break;
            }
        }
        
        if (found) return i;
    }
    
    return -1;
}

// Approach 4: KMP Algorithm (Advanced - O(n+m))
// Time: O(n+m), Space: O(m)
public int strStrKMP(String haystack, String needle) {
    if (needle.isEmpty()) return 0;
    
    int n = haystack.length();
    int m = needle.length();
    
    // Build LPS (Longest Prefix Suffix) array
    int[] lps = buildLPS(needle);
    
    int i = 0; // haystack pointer
    int j = 0; // needle pointer
    
    while (i < n) {
        if (haystack.charAt(i) == needle.charAt(j)) {
            i++;
            j++;
        }
        
        if (j == m) {
            return i - j; // Found match
        } else if (i < n && haystack.charAt(i) != needle.charAt(j)) {
            if (j != 0) {
                j = lps[j - 1];
            } else {
                i++;
            }
        }
    }
    
    return -1;
}

private int[] buildLPS(String pattern) {
    int m = pattern.length();
    int[] lps = new int[m];
    int len = 0;
    int i = 1;
    
    while (i < m) {
        if (pattern.charAt(i) == pattern.charAt(len)) {
            len++;
            lps[i] = len;
            i++;
        } else {
            if (len != 0) {
                len = lps[len - 1];
            } else {
                lps[i] = 0;
                i++;
            }
        }
    }
    
    return lps;
}
```

---

## Core Concept

This problem is NOT just about string matching. It teaches:

- Pattern searching in text
- Sliding window technique
- Prefix-suffix relationships (KMP)
- Optimization through preprocessing

Any place where you need to find text patterns, detect keywords, search logs, validate formats, or filter content - this pattern applies.

---

## Real-World Use Cases

### 1. Web Security: URL Filtering and Threat Detection

**Problem:** Detect malicious patterns in URLs and block phishing attempts

**Example:** Security filter (Backend - Java)
```java
public class URLSecurityFilter {
    
    private static final String[] SUSPICIOUS_PATTERNS = {
        "javascript:", "data:", "vbscript:", "<script", 
        "../", "eval(", "base64", "onclick="
    };
    
    private static final String[] PHISHING_KEYWORDS = {
        "confirm-account", "verify-identity", "urgent-action",
        "suspended-account", "unusual-activity", "click-here"
    };
    
    public int findPattern(String url, String pattern) {
        // Simple strStr implementation
        for (int i = 0, j = pattern.length(); j <= url.length(); i++, j++) {
            if (url.substring(i, j).equalsIgnoreCase(pattern)) {
                return i;
            }
        }
        return -1;
    }
    
    public SecurityResult analyzeURL(String url) {
        String lowerUrl = url.toLowerCase();
        List<String> threats = new ArrayList<>();
        
        // Check for XSS patterns
        for (String pattern : SUSPICIOUS_PATTERNS) {
            if (findPattern(lowerUrl, pattern) != -1) {
                threats.add("XSS_RISK: " + pattern);
            }
        }
        
        // Check for phishing indicators
        for (String keyword : PHISHING_KEYWORDS) {
            if (findPattern(lowerUrl, keyword) != -1) {
                threats.add("PHISHING_RISK: " + keyword);
            }
        }
        
        return new SecurityResult(
            threats.isEmpty(),
            threats,
            calculateRiskScore(threats.size())
        );
    }
    
    private String calculateRiskScore(int threatCount) {
        if (threatCount == 0) return "SAFE";
        if (threatCount <= 2) return "LOW";
        if (threatCount <= 4) return "MEDIUM";
        return "HIGH";
    }
}

// Used by: Cloudflare, AWS WAF, web browsers
// Chrome Safe Browsing, Firefox security filters
```

**Use Case:**
- Web application firewalls (WAF)
- Browser security extensions
- Email spam filters
- Network intrusion detection systems

### 2. Search Engines: Keyword Highlighting and Snippet Generation

**Problem:** Find and highlight search terms in search results

**Example:** Search result highlighter (JavaScript)
```javascript
class SearchHighlighter {
    
    findPattern(text, pattern) {
        const lowerText = text.toLowerCase();
        const lowerPattern = pattern.toLowerCase();
        
        for (let i = 0, j = pattern.length; j <= text.length; i++, j++) {
            if (lowerText.substring(i, j) === lowerPattern) {
                return i;
            }
        }
        return -1;
    }
    
    findAllOccurrences(text, pattern) {
        const positions = [];
        const lowerText = text.toLowerCase();
        const lowerPattern = pattern.toLowerCase();
        
        for (let i = 0; i <= text.length - pattern.length; i++) {
            if (lowerText.substring(i, i + pattern.length) === lowerPattern) {
                positions.push(i);
            }
        }
        
        return positions;
    }
    
    generateSnippet(text, searchTerm, contextLength = 50) {
        const position = this.findPattern(text, searchTerm);
        
        if (position === -1) {
            return text.substring(0, 100) + "...";
        }
        
        // Generate snippet around the found term
        const start = Math.max(0, position - contextLength);
        const end = Math.min(text.length, position + searchTerm.length + contextLength);
        
        let snippet = text.substring(start, end);
        
        if (start > 0) snippet = "..." + snippet;
        if (end < text.length) snippet = snippet + "...";
        
        return snippet;
    }
    
    highlightMatches(text, searchTerms) {
        let result = text;
        
        for (const term of searchTerms) {
            const positions = this.findAllOccurrences(result, term);
            
            // Highlight from right to left to maintain positions
            for (let i = positions.length - 1; i >= 0; i--) {
                const pos = positions[i];
                const original = result.substring(pos, pos + term.length);
                result = result.substring(0, pos) +
                        `<mark>${original}</mark>` +
                        result.substring(pos + term.length);
            }
        }
        
        return result;
    }
}

// Google Search result snippets
// Elasticsearch highlighting
```

**Use Case:**
- Search engines (Google, Bing)
- E-commerce site search
- Document search systems
- Code search tools (GitHub)

### 3. Content Moderation: Profanity and Hate Speech Detection

**Problem:** Detect and filter inappropriate content in user-generated text

**Example:** Content filter (Backend - Java)
```java
public class ContentModerationFilter {
    
    private Set<String> profanityList;
    private Set<String> hateSpeechPatterns;
    private Map<String, String> censorMap;
    
    public ContentModerationFilter() {
        // Load from database or config
        this.profanityList = loadProfanityList();
        this.hateSpeechPatterns = loadHateSpeechPatterns();
        this.censorMap = new HashMap<>();
    }
    
    public int findPattern(String text, String pattern) {
        String lowerText = text.toLowerCase();
        String lowerPattern = pattern.toLowerCase();
        
        for (int i = 0, j = pattern.length(); j <= text.length(); i++, j++) {
            if (lowerText.substring(i, j).equals(lowerPattern)) {
                return i;
            }
        }
        return -1;
    }
    
    public ModerationResult moderateContent(String content) {
        String lowerContent = content.toLowerCase();
        List<String> violations = new ArrayList<>();
        int violationCount = 0;
        
        // Check for profanity
        for (String word : profanityList) {
            if (findPattern(lowerContent, word) != -1) {
                violations.add("PROFANITY: " + word);
                violationCount++;
            }
        }
        
        // Check for hate speech patterns
        for (String pattern : hateSpeechPatterns) {
            if (findPattern(lowerContent, pattern) != -1) {
                violations.add("HATE_SPEECH: " + pattern);
                violationCount++;
            }
        }
        
        return new ModerationResult(
            violationCount == 0,
            violations,
            violationCount >= 3 ? "AUTO_REJECT" : 
            violationCount >= 1 ? "MANUAL_REVIEW" : "APPROVED",
            censorContent(content, violations)
        );
    }
    
    private String censorContent(String content, List<String> violations) {
        String result = content;
        
        for (String violation : violations) {
            String word = violation.split(": ")[1];
            int pos = findPattern(result.toLowerCase(), word);
            
            if (pos != -1) {
                String censored = "*".repeat(word.length());
                result = result.substring(0, pos) + 
                        censored + 
                        result.substring(pos + word.length());
            }
        }
        
        return result;
    }
}

// Facebook, Twitter, Reddit content moderation
// Gaming chat filters (Discord, Twitch)
```

**Use Case:**
- Social media platforms
- Gaming chat systems
- Comment sections
- User review platforms

### 4. Log Analysis: Error Pattern Detection

**Problem:** Search through application logs to find error patterns and anomalies

**Example:** Log analyzer (JavaScript)
```javascript
class LogAnalyzer {
    
    findPattern(logLine, pattern) {
        for (let i = 0, j = pattern.length; j <= logLine.length; i++, j++) {
            if (logLine.substring(i, j) === pattern) {
                return i;
            }
        }
        return -1;
    }
    
    analyzeLog(logContent) {
        const lines = logContent.split('\n');
        const errorPatterns = [
            'ERROR', 'FATAL', 'Exception', 
            'OutOfMemory', 'NullPointer', 'Connection refused'
        ];
        
        const results = {
            totalLines: lines.length,
            errors: [],
            errorsByType: {},
            criticalErrors: []
        };
        
        lines.forEach((line, index) => {
            for (const pattern of errorPatterns) {
                const position = this.findPattern(line, pattern);
                
                if (position !== -1) {
                    const errorInfo = {
                        lineNumber: index + 1,
                        line: line,
                        errorType: pattern,
                        position: position
                    };
                    
                    results.errors.push(errorInfo);
                    
                    // Count by type
                    results.errorsByType[pattern] = 
                        (results.errorsByType[pattern] || 0) + 1;
                    
                    // Flag critical errors
                    if (pattern === 'FATAL' || pattern === 'OutOfMemory') {
                        results.criticalErrors.push(errorInfo);
                    }
                }
            }
        });
        
        return results;
    }
    
    searchLogs(logs, searchTerm, timeRange) {
        const matches = [];
        const lines = logs.split('\n');
        
        lines.forEach((line, index) => {
            if (this.findPattern(line, searchTerm) !== -1) {
                matches.push({
                    lineNumber: index + 1,
                    content: line,
                    timestamp: this.extractTimestamp(line)
                });
            }
        });
        
        return matches;
    }
    
    extractTimestamp(line) {
        // Simple timestamp extraction
        const match = line.match(/\d{4}-\d{2}-\d{2} \d{2}:\d{2}:\d{2}/);
        return match ? match[0] : null;
    }
}

// Splunk, ELK Stack (Elasticsearch, Logstash, Kibana)
// AWS CloudWatch Logs, Datadog
```

**Use Case:**
- Application monitoring (Splunk, ELK)
- Cloud platform logging (AWS CloudWatch)
- System administration
- DevOps troubleshooting

### 5. Email Systems: Spam Detection and Email Parsing

**Problem:** Detect spam patterns and parse email headers

**Example:** Email spam detector (Backend - Java)
```java
public class EmailSpamDetector {
    
    private static final String[] SPAM_KEYWORDS = {
        "click here now", "limited time offer", "act now",
        "100% free", "earn money fast", "weight loss",
        "viagra", "casino", "lottery winner"
    };
    
    private static final String[] SUSPICIOUS_HEADERS = {
        "X-Spam", "X-Mailer: ", "Received: from"
    };
    
    public int findPattern(String text, String pattern) {
        String lowerText = text.toLowerCase();
        String lowerPattern = pattern.toLowerCase();
        
        for (int i = 0, j = pattern.length(); j <= text.length(); i++, j++) {
            if (lowerText.substring(i, j).equals(lowerPattern)) {
                return i;
            }
        }
        return -1;
    }
    
    public SpamAnalysisResult analyzeEmail(String subject, String body, String headers) {
        int spamScore = 0;
        List<String> indicators = new ArrayList<>();
        
        String fullContent = subject + " " + body;
        
        // Check for spam keywords
        for (String keyword : SPAM_KEYWORDS) {
            if (findPattern(fullContent, keyword) != -1) {
                spamScore += 10;
                indicators.add("Spam keyword: " + keyword);
            }
        }
        
        // Check for suspicious headers
        for (String header : SUSPICIOUS_HEADERS) {
            if (findPattern(headers, header) != -1) {
                spamScore += 5;
                indicators.add("Suspicious header: " + header);
            }
        }
        
        // Check for excessive capital letters
        if (hasExcessiveCaps(subject)) {
            spamScore += 15;
            indicators.add("Excessive capitalization in subject");
        }
        
        return new SpamAnalysisResult(
            spamScore >= 30 ? "SPAM" : spamScore >= 15 ? "SUSPICIOUS" : "HAM",
            spamScore,
            indicators
        );
    }
    
    private boolean hasExcessiveCaps(String text) {
        if (text.length() < 5) return false;
        
        long capsCount = text.chars().filter(Character::isUpperCase).count();
        return (double) capsCount / text.length() > 0.6;
    }
    
    public EmailMetadata parseEmailHeaders(String headers) {
        // Extract sender
        String fromPattern = "From: ";
        int fromPos = findPattern(headers, fromPattern);
        String sender = fromPos != -1 ? 
            extractUntilNewline(headers, fromPos + fromPattern.length()) : "Unknown";
        
        // Extract subject
        String subjectPattern = "Subject: ";
        int subjectPos = findPattern(headers, subjectPattern);
        String subject = subjectPos != -1 ? 
            extractUntilNewline(headers, subjectPos + subjectPattern.length()) : "No Subject";
        
        return new EmailMetadata(sender, subject);
    }
    
    private String extractUntilNewline(String text, int start) {
        int end = text.indexOf('\n', start);
        return end != -1 ? text.substring(start, end).trim() : text.substring(start).trim();
    }
}

// Gmail spam filter, Microsoft Outlook
// SpamAssassin, Postfix mail server
```

**Use Case:**
- Email service providers (Gmail, Outlook)
- Corporate email security
- Anti-spam solutions
- Email marketing compliance

### 6. Code Editors: Find and Replace Functionality

**Problem:** Search for patterns in code and enable find/replace operations

**Example:** Code editor find functionality (JavaScript)
```javascript
class CodeEditorSearch {
    
    findPattern(text, pattern, caseSensitive = true) {
        const searchText = caseSensitive ? text : text.toLowerCase();
        const searchPattern = caseSensitive ? pattern : pattern.toLowerCase();
        
        for (let i = 0, j = pattern.length; j <= text.length; i++, j++) {
            if (searchText.substring(i, j) === searchPattern) {
                return i;
            }
        }
        return -1;
    }
    
    findAll(code, searchTerm, options = {}) {
        const { caseSensitive = true, wholeWord = false, regex = false } = options;
        
        if (regex) {
            return this.findWithRegex(code, searchTerm);
        }
        
        const matches = [];
        const lines = code.split('\n');
        
        lines.forEach((line, lineNumber) => {
            let searchLine = caseSensitive ? line : line.toLowerCase();
            let searchPattern = caseSensitive ? searchTerm : searchTerm.toLowerCase();
            
            let startPos = 0;
            while (startPos < line.length) {
                const position = this.findPattern(
                    searchLine.substring(startPos), 
                    searchPattern
                );
                
                if (position === -1) break;
                
                const actualPos = startPos + position;
                
                // Check whole word matching
                if (wholeWord && !this.isWholeWordMatch(line, actualPos, searchTerm.length)) {
                    startPos = actualPos + 1;
                    continue;
                }
                
                matches.push({
                    line: lineNumber + 1,
                    column: actualPos + 1,
                    length: searchTerm.length,
                    text: line.substring(actualPos, actualPos + searchTerm.length),
                    context: line.trim()
                });
                
                startPos = actualPos + 1;
            }
        });
        
        return matches;
    }
    
    isWholeWordMatch(text, position, length) {
        const before = position > 0 ? text[position - 1] : ' ';
        const after = position + length < text.length ? text[position + length] : ' ';
        
        return !this.isWordChar(before) && !this.isWordChar(after);
    }
    
    isWordChar(char) {
        return /[a-zA-Z0-9_]/.test(char);
    }
    
    replaceAll(code, searchTerm, replaceTerm, options = {}) {
        const matches = this.findAll(code, searchTerm, options);
        const lines = code.split('\n');
        
        // Replace from end to start to maintain positions
        for (let i = matches.length - 1; i >= 0; i--) {
            const match = matches[i];
            const lineIndex = match.line - 1;
            const line = lines[lineIndex];
            
            lines[lineIndex] = 
                line.substring(0, match.column - 1) +
                replaceTerm +
                line.substring(match.column - 1 + match.length);
        }
        
        return {
            code: lines.join('\n'),
            replacementCount: matches.length
        };
    }
}

// VS Code, Sublime Text, IntelliJ IDEA
// GitHub code search
```

**Use Case:**
- Code editors (VS Code, Sublime)
- IDEs (IntelliJ, Eclipse)
- Code review tools
- Version control systems

---

## Why This Matters in Production

### Algorithm Comparison
```
Brute Force (Naive):
- Time: O(n*m) where n=haystack, m=needle
- Space: O(1)
- Simple, works for small inputs

KMP Algorithm:
- Time: O(n+m) - optimal
- Space: O(m) for LPS array
- Better for long texts or repeated searches

Boyer-Moore:
- Time: O(n/m) best case, O(n*m) worst
- Space: O(m)
- Fastest in practice for large alphabets

Rabin-Karp:
- Time: O(n+m) average, O(n*m) worst
- Space: O(1)
- Good for multiple pattern search
```

### Performance Optimization
```
For production systems:

1. Small patterns (<10 chars): Brute force is fine
2. Large text, repeated search: KMP or Boyer-Moore
3. Multiple patterns: Use Aho-Corasick (trie-based)
4. Case-insensitive: Lowercase both before comparing
5. Unicode support: Use proper string libraries
```

### Common Pitfalls
```
1. Case sensitivity: "Hello" != "hello"
2. Empty strings: Define behavior clearly
3. Unicode characters: Use proper char counting
4. Performance: Don't create substrings in loops
5. Off-by-one: j <= haystack.length() (not <)
```

---

## Interview Tip

When explaining this problem, say:

"String matching (strStr) is fundamental to text processing and pattern searching. The key insight is sliding a window of needle's length across the haystack and comparing characters. While the naive O(n*m) approach works well for most cases, advanced algorithms like KMP achieve O(n+m) through preprocessing. This pattern is critical in web security for URL filtering and XSS detection, search engines for keyword highlighting and snippet generation, content moderation for profanity filtering, log analysis for error detection, email systems for spam detection, and code editors for find/replace functionality. The simplicity-performance tradeoff makes the naive approach preferred in practice unless dealing with very large texts or repeated searches."

This demonstrates understanding of pattern matching and practical text processing applications.

---

## Key Takeaway

String matching (strStr) is the foundation of text search and pattern detection. The sliding window technique with character comparison is used in security systems, search engines, content filters, log analyzers, email processing, and code editors - any system requiring text pattern matching, keyword detection, or content validation. While advanced algorithms exist, the simple O(n*m) solution handles most real-world scenarios effectively.