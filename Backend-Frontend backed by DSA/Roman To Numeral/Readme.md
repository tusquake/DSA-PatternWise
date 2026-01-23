# Roman to Integer - Real World Applications

## Problem Statement

Roman numerals are represented by seven different symbols: I, V, X, L, C, D and M.

```
Symbol       Value
I             1
V             5
X             10
L             50
C             100
D             500
M             1000
```

Roman numerals are usually written largest to smallest from left to right. However, the numeral for four is not IIII. Instead, the number four is written as IV. The same principle applies to other subtractive cases:
- I can be placed before V (5) and X (10) to make 4 and 9
- X can be placed before L (50) and C (100) to make 40 and 90
- C can be placed before D (500) and M (1000) to make 400 and 900

Given a roman numeral, convert it to an integer.

**Example 1:**
```
Input:  s = "III"
Output: 3
Explanation: III = 3
```

**Example 2:**
```
Input:  s = "LVIII"
Output: 58
Explanation: L = 50, V= 5, III = 3
```

**Example 3:**
```
Input:  s = "MCMXCIV"
Output: 1994
Explanation: M = 1000, CM = 900, XC = 90, IV = 4
```

**Constraints:**
- 1 <= s.length <= 15
- s contains only the characters ('I', 'V', 'X', 'L', 'C', 'D', 'M')
- It is guaranteed that s is a valid roman numeral in the range [1, 3999]

**Solution (Java):**
```java
// Approach 1: Left-to-right with lookahead
// Time: O(n), Space: O(1)
public int romanToInt(String s) {
    Map<Character, Integer> values = new HashMap<>();
    values.put('I', 1);
    values.put('V', 5);
    values.put('X', 10);
    values.put('L', 50);
    values.put('C', 100);
    values.put('D', 500);
    values.put('M', 1000);
    
    int result = 0;
    
    for (int i = 0; i < s.length(); i++) {
        int current = values.get(s.charAt(i));
        
        // Look ahead: if next is larger, subtract current
        if (i + 1 < s.length() && current < values.get(s.charAt(i + 1))) {
            result -= current;
        } else {
            result += current;
        }
    }
    
    return result;
}

// Approach 2: Right-to-left (cleaner logic)
// Time: O(n), Space: O(1)
public int romanToIntRightToLeft(String s) {
    Map<Character, Integer> values = new HashMap<>();
    values.put('I', 1);
    values.put('V', 5);
    values.put('X', 10);
    values.put('L', 50);
    values.put('C', 100);
    values.put('D', 500);
    values.put('M', 1000);
    
    int result = 0;
    int prevValue = 0;
    
    // Process right to left
    for (int i = s.length() - 1; i >= 0; i--) {
        int current = values.get(s.charAt(i));
        
        // If current < previous, subtract (like IV)
        if (current < prevValue) {
            result -= current;
        } else {
            result += current;
        }
        
        prevValue = current;
    }
    
    return result;
}

// Approach 3: Using switch statement (faster)
// Time: O(n), Space: O(1)
public int romanToIntSwitch(String s) {
    int result = 0;
    int prevValue = 0;
    
    for (int i = s.length() - 1; i >= 0; i--) {
        int current = getValue(s.charAt(i));
        
        if (current < prevValue) {
            result -= current;
        } else {
            result += current;
        }
        
        prevValue = current;
    }
    
    return result;
}

private int getValue(char c) {
    switch(c) {
        case 'I': return 1;
        case 'V': return 5;
        case 'X': return 10;
        case 'L': return 50;
        case 'C': return 100;
        case 'D': return 500;
        case 'M': return 1000;
        default: return 0;
    }
}
```

---

## Core Concept

This problem is NOT just about Roman numerals. It teaches:

- String parsing with context awareness
- Lookahead/lookback patterns
- Character mapping and translation
- Subtraction rule handling

Any place where you need to parse formatted text, convert notation systems, process legacy formats, or handle context-dependent rules - this pattern applies.

---

## Real-World Use Cases

### 1. Document Processing: Historical Document Parsing

**Problem:** Extract dates and numbers from historical documents with Roman numerals

**Example:** Historical document parser (Backend - Java)
```java
public class HistoricalDocumentParser {
    
    public int parseRomanNumeral(String roman) {
        Map<Character, Integer> values = new HashMap<>();
        values.put('I', 1);
        values.put('V', 5);
        values.put('X', 10);
        values.put('L', 50);
        values.put('C', 100);
        values.put('D', 500);
        values.put('M', 1000);
        
        int result = 0;
        int prevValue = 0;
        
        for (int i = roman.length() - 1; i >= 0; i--) {
            int current = values.get(roman.charAt(i));
            
            if (current < prevValue) {
                result -= current;
            } else {
                result += current;
            }
            
            prevValue = current;
        }
        
        return result;
    }
    
    public DocumentMetadata extractMetadata(String document) {
        // Extract patterns like "Volume IV", "Chapter XII", "Year MDCCCXCIX"
        Pattern volumePattern = Pattern.compile("Volume\\s+([IVXLCDM]+)");
        Pattern chapterPattern = Pattern.compile("Chapter\\s+([IVXLCDM]+)");
        Pattern yearPattern = Pattern.compile("Year\\s+([IVXLCDM]+)");
        
        Matcher volMatcher = volumePattern.matcher(document);
        Matcher chapMatcher = chapterPattern.matcher(document);
        Matcher yearMatcher = yearPattern.matcher(document);
        
        Integer volume = volMatcher.find() ? parseRomanNumeral(volMatcher.group(1)) : null;
        Integer chapter = chapMatcher.find() ? parseRomanNumeral(chapMatcher.group(1)) : null;
        Integer year = yearMatcher.find() ? parseRomanNumeral(yearMatcher.group(1)) : null;
        
        return new DocumentMetadata(volume, chapter, year);
    }
}

// Used by: Libraries, museums, digital archives
// Google Books, Archive.org document processing
```

**Use Case:**
- Digital library systems
- Historical document digitization
- Academic research databases

### 2. Movie/TV Industry: Title Sequence Parsing

**Problem:** Parse movie sequels and series numbers from titles

**Example:** Title parser (JavaScript)
```javascript
class MovieTitleParser {
    
    romanToInt(s) {
        const values = {
            'I': 1, 'V': 5, 'X': 10, 'L': 50,
            'C': 100, 'D': 500, 'M': 1000
        };
        
        let result = 0;
        let prevValue = 0;
        
        for (let i = s.length - 1; i >= 0; i--) {
            const current = values[s[i]];
            
            if (current < prevValue) {
                result -= current;
            } else {
                result += current;
            }
            
            prevValue = current;
        }
        
        return result;
    }
    
    parseMovieTitle(title) {
        // Examples: "Rocky IV", "Super Bowl LVIII", "Final Fantasy VII"
        const romanPattern = /\b([IVXLCDM]+)\b/g;
        const matches = title.match(romanPattern);
        
        if (matches) {
            const lastRoman = matches[matches.length - 1];
            const sequelNumber = this.romanToInt(lastRoman);
            
            return {
                title: title,
                sequelNumber: sequelNumber,
                isSequel: sequelNumber > 1,
                displayName: title,
                sortOrder: sequelNumber
            };
        }
        
        return {
            title: title,
            sequelNumber: 1,
            isSequel: false
        };
    }
    
    sortMovieSeries(movies) {
        return movies
            .map(m => this.parseMovieTitle(m))
            .sort((a, b) => a.sortOrder - b.sortOrder);
    }
}

// Netflix, IMDb movie cataloging
// Streaming platform content organization
```

**Use Case:**
- Streaming platforms (Netflix, Disney+)
- Movie databases (IMDb, TMDB)
- Content management systems

### 3. Publishing: Book Edition Numbering

**Problem:** Parse edition numbers from book titles and metadata

**Example:** Book edition parser (Backend - Java)
```java
public class BookEditionParser {
    
    public int parseEdition(String editionText) {
        // Extract Roman numeral from text like "Third Edition" or "Edition III"
        Pattern pattern = Pattern.compile("([IVXLCDM]+)");
        Matcher matcher = pattern.matcher(editionText.toUpperCase());
        
        if (matcher.find()) {
            return romanToInt(matcher.group(1));
        }
        
        return 1; // Default first edition
    }
    
    private int romanToInt(String s) {
        Map<Character, Integer> values = new HashMap<>();
        values.put('I', 1);
        values.put('V', 5);
        values.put('X', 10);
        values.put('L', 50);
        values.put('C', 100);
        values.put('D', 500);
        values.put('M', 1000);
        
        int result = 0;
        int prevValue = 0;
        
        for (int i = s.length() - 1; i >= 0; i--) {
            int current = values.get(s.charAt(i));
            
            if (current < prevValue) {
                result -= current;
            } else {
                result += current;
            }
            
            prevValue = current;
        }
        
        return result;
    }
    
    public BookMetadata analyzeBook(String title, String edition) {
        int editionNumber = parseEdition(edition);
        
        return new BookMetadata(
            title,
            editionNumber,
            editionNumber > 1 ? "UPDATED_EDITION" : "FIRST_EDITION",
            "Edition " + editionNumber
        );
    }
}

// Amazon Books, Google Books cataloging
// Library management systems
```

**Use Case:**
- Library management systems
- Online bookstores
- Academic publishing platforms

### 4. Event Management: Sports Event Numbering

**Problem:** Parse event numbers from sports championships

**Example:** Event parser (JavaScript)
```javascript
class SportsEventParser {
    
    romanToInt(s) {
        const values = {
            'I': 1, 'V': 5, 'X': 10, 'L': 50,
            'C': 100, 'D': 500, 'M': 1000
        };
        
        let result = 0;
        let prevValue = 0;
        
        for (let i = s.length - 1; i >= 0; i--) {
            const current = values[s[i]];
            
            if (current < prevValue) {
                result -= current;
            } else {
                result += current;
            }
            
            prevValue = current;
        }
        
        return result;
    }
    
    parseEvent(eventName) {
        // Examples: "Super Bowl LVIII", "Olympics XXXIII", "World Cup XXII"
        const match = eventName.match(/([IVXLCDM]+)/);
        
        if (match) {
            const roman = match[1];
            const edition = this.romanToInt(roman);
            
            return {
                name: eventName,
                edition: edition,
                year: this.estimateYear(eventName, edition),
                displayNumber: `#${edition}`,
                isAnniversary: edition % 10 === 0 || edition % 25 === 0
            };
        }
        
        return null;
    }
    
    estimateYear(name, edition) {
        // Super Bowl started in 1967
        if (name.includes("Super Bowl")) {
            return 1966 + edition;
        }
        return null;
    }
}

// ESPN, NFL event management
// Olympics organizing committee systems
```

**Use Case:**
- Sports broadcasting systems
- Event ticketing platforms
- Championship tracking

### 5. Legal Documents: Statute and Section Parsing

**Problem:** Parse legal citations with Roman numerals

**Example:** Legal citation parser (Backend - Java)
```java
public class LegalCitationParser {
    
    public CitationReference parseCitation(String citation) {
        // Example: "Title IX", "Article IV Section 2", "Amendment XIV"
        Pattern titlePattern = Pattern.compile("Title\\s+([IVXLCDM]+)");
        Pattern articlePattern = Pattern.compile("Article\\s+([IVXLCDM]+)");
        Pattern sectionPattern = Pattern.compile("Section\\s+([IVXLCDM]+)");
        
        Matcher titleMatcher = titlePattern.matcher(citation);
        Matcher articleMatcher = articlePattern.matcher(citation);
        Matcher sectionMatcher = sectionPattern.matcher(citation);
        
        Integer titleNum = titleMatcher.find() ? 
            romanToInt(titleMatcher.group(1)) : null;
        Integer articleNum = articleMatcher.find() ? 
            romanToInt(articleMatcher.group(1)) : null;
        Integer sectionNum = sectionMatcher.find() ? 
            romanToInt(sectionMatcher.group(1)) : null;
        
        return new CitationReference(titleNum, articleNum, sectionNum);
    }
    
    private int romanToInt(String s) {
        Map<Character, Integer> values = new HashMap<>();
        values.put('I', 1);
        values.put('V', 5);
        values.put('X', 10);
        values.put('L', 50);
        values.put('C', 100);
        values.put('D', 500);
        values.put('M', 1000);
        
        int result = 0;
        int prevValue = 0;
        
        for (int i = s.length() - 1; i >= 0; i--) {
            int current = values.get(s.charAt(i));
            
            if (current < prevValue) {
                result -= current;
            } else {
                result += current;
            }
            
            prevValue = current;
        }
        
        return result;
    }
}

// LexisNexis, Westlaw legal databases
// Court document management systems
```

**Use Case:**
- Legal research platforms
- Court filing systems
- Legislative databases

### 6. Gaming: Level and Achievement Parsing

**Problem:** Parse game level numbers with Roman numerals

**Example:** Game level parser (JavaScript)
```javascript
class GameLevelParser {
    
    romanToInt(s) {
        const values = {
            'I': 1, 'V': 5, 'X': 10, 'L': 50,
            'C': 100, 'D': 500, 'M': 1000
        };
        
        let result = 0;
        let prevValue = 0;
        
        for (let i = s.length - 1; i >= 0; i--) {
            const current = values[s[i]];
            
            if (current < prevValue) {
                result -= current;
            } else {
                result += current;
            }
            
            prevValue = current;
        }
        
        return result;
    }
    
    parseLevel(levelName) {
        // Examples: "World IV-3", "Boss Battle IX", "Chapter XII"
        const match = levelName.match(/([IVXLCDM]+)/);
        
        if (match) {
            const levelNumber = this.romanToInt(match[1]);
            
            return {
                displayName: levelName,
                numericLevel: levelNumber,
                difficulty: this.calculateDifficulty(levelNumber),
                unlocked: false,
                estimatedTime: levelNumber * 5 + " minutes"
            };
        }
        
        return null;
    }
    
    calculateDifficulty(level) {
        if (level <= 3) return "EASY";
        if (level <= 7) return "MEDIUM";
        if (level <= 12) return "HARD";
        return "EXTREME";
    }
}

// Final Fantasy, Resident Evil game series
// RPG level progression systems
```

**Use Case:**
- Video game progression systems
- Achievement tracking
- Game content organization

---

## Why This Matters in Production

### The Subtraction Rule
```
Key insight: 
- If current < next: subtract current (IV = 4, IX = 9)
- Otherwise: add current

Right-to-left is cleaner:
- Compare with previous instead of next
- No need for lookahead

Example: MCMXCIV (1994)
M=1000, CM=900 (C < M, subtract), XC=90 (X < C, subtract), IV=4
```

### Performance Comparison
```
All approaches: O(n) time, O(1) space

HashMap: Flexible, handles any Roman numeral
Switch: Slightly faster (direct jump)
Array: Fastest (direct index) but less readable

For production: Use HashMap (clarity > micro-optimization)
```

### Common Patterns
```
I, II, III = 1, 2, 3
IV = 4 (not IIII)
IX = 9 (not VIIII)
XL = 40, XC = 90
CD = 400, CM = 900
```

---

## Interview Tip

When explaining this problem, say:

"Roman to Integer teaches context-aware string parsing where the value of a character depends on its position relative to others. The key insight is the subtraction rule - when a smaller value appears before a larger one, we subtract instead of add. Processing right-to-left simplifies this by comparing each character with the previous rather than lookahead. This pattern is essential in historical document processing for digitizing archives, movie/TV platforms for parsing sequel numbers, publishing systems for edition tracking, sports broadcasting for event numbering, legal databases for statute parsing, and gaming systems for level progression. The O(n) time, O(1) space solution handles all valid Roman numerals up to 3999."

This demonstrates understanding of context-dependent parsing and practical text processing.

---

## Key Takeaway

Roman to Integer is a blueprint for parsing notation systems with context-dependent rules. The right-to-left scanning pattern with subtraction rule handling is used in document digitization, content management, legal databases, sports broadcasting, and gaming systems - any domain requiring conversion between traditional notation and modern numeric systems.