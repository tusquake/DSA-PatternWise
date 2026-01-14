# Generate Parentheses - Real World Applications

## Problem Statement

Given n pairs of parentheses, generate all combinations of well-formed parentheses.

**Example 1:**
```
Input:  n = 3
Output: ["((()))","(()())","(())()","()(())","()()()"]
```

**Example 2:**
```
Input:  n = 1
Output: ["()"]
```

**Example 3:**
```
Input:  n = 2
Output: ["(())","()()"]
```

**Constraints:**
- 1 <= n <= 8
- All parentheses must be properly closed
- Each opening bracket must have a corresponding closing bracket
- Closing bracket cannot come before its opening bracket

**Solution (Java):**
```java
public List<String> generateParenthesis(int n) {
    List<String> result = new ArrayList<>();
    backtrack(result, "", 0, 0, n);
    return result;
}

private void backtrack(List<String> result, String current, 
                       int open, int close, int max) {
    // Base case: valid combination complete
    if (current.length() == max * 2) {
        result.add(current);
        return;
    }
    
    // Add opening bracket if we haven't used all
    if (open < max) {
        backtrack(result, current + "(", open + 1, close, max);
    }
    
    // Add closing bracket only if it won't break validity
    if (close < open) {
        backtrack(result, current + ")", open, close + 1, max);
    }
}
```

---

## Core Concept

This problem is NOT about brackets. It teaches:

- Constraint-based generation
- State tracking
- Safe construction of nested structures
- Backtracking to avoid invalid states

Any place where order matters, nesting must stay valid, and partial states must never break rules - this pattern applies.

---

## Real-World Use Cases

### 1. Dynamic JSON Generation

**Problem:** Generating dynamic JSON for UI configurations (charts, graphs, dashboards)

**Example:** Chart configuration in React/SAPUI5
```javascript
// Need to build this dynamically based on user selections
{
  "series": [
    {
      "name": "MTTR",
      "data": [...],
      "drilldown": {
        "series": [...]
      }
    }
  ],
  "chart": {
    "type": "line"
  }
}
```

**Challenge:**
- Multiple series with conditional nesting
- Optional fields and drilldowns
- One missing `}` or `]` crashes the entire graph
- User can add/remove sections dynamically

**Solution Pattern (Generate Parentheses Logic):**
```javascript
class JSONBuilder {
    constructor() {
        this.openBraces = 0;
        this.openBrackets = 0;
        this.json = "";
    }
    
    canOpenObject() {
        return this.openBraces < this.maxNesting;
    }
    
    canCloseObject() {
        return this.openBraces > 0;
    }
    
    addObject(key, value) {
        if (this.canOpenObject()) {
            this.json += `"${key}": {`;
            this.openBraces++;
            // ... add content
        }
    }
    
    closeObject() {
        if (this.canCloseObject()) {
            this.json += "}";
            this.openBraces--;
        }
    }
}
```

**Mapping:**

| Generate Parentheses | Dynamic JSON |
|---------------------|--------------|
| `(` open bracket | `{` or `[` |
| `)` close bracket | `}` or `]` |
| open < n | allowed nesting level |
| close < open | don't close before open |
| length == 2n | JSON complete |
| backtrack | undo invalid nesting |

### 2. API Payload Generator (Backend)

**Problem:** Building dynamic filter payloads based on user selections

**Example:** Search API with nested conditions (Java)
```java
public class PayloadBuilder {
    private int openBlocks = 0;
    private StringBuilder payload = new StringBuilder();
    
    public void addFilter(String field, String operator, String value) {
        if (canOpenBlock()) {
            payload.append("{");
            openBlocks++;
            
            payload.append("\"").append(field).append("\": {");
            payload.append("\"").append(operator).append("\": ");
            payload.append("\"").append(value).append("\"");
        }
    }
    
    public void closeFilter() {
        if (canCloseBlock()) {
            payload.append("}");
            openBlocks--;
        }
    }
    
    private boolean canOpenBlock() {
        return openBlocks < maxNestingLevel;
    }
    
    private boolean canCloseBlock() {
        return openBlocks > 0;
    }
}

// Usage
PayloadBuilder builder = new PayloadBuilder();
builder.addFilter("date", "from", "2024-01-01");
builder.addFilter("equipment", "id", "123");
builder.closeFilter();
builder.closeFilter();
// Result: Valid nested JSON
```

**Prevents:**
- Broken JSON syntax
- API 400 errors
- Runtime failures

### 3. Query Builders (Backend)

**Problem:** Dynamic WHERE clause construction

**Example:** SQL query builder (Java)
```java
public class QueryBuilder {
    private int openParens = 0;
    private StringBuilder query = new StringBuilder("WHERE ");
    
    public void addCondition(String condition, boolean useParens) {
        if (useParens && canOpenParen()) {
            query.append("(");
            openParens++;
        }
        
        query.append(condition);
    }
    
    public void addLogicalOperator(String operator) {
        query.append(" ").append(operator).append(" ");
    }
    
    public void closeGroup() {
        if (canCloseParen()) {
            query.append(")");
            openParens--;
        }
    }
    
    private boolean canOpenParen() {
        return openParens < maxDepth;
    }
    
    private boolean canCloseParen() {
        return openParens > 0;
    }
}

// Usage
QueryBuilder qb = new QueryBuilder();
qb.addCondition("Plant = 'A'", true);
qb.addLogicalOperator("AND");
qb.addCondition("Equipment = 'X'", true);
qb.addLogicalOperator("OR");
qb.addCondition("Equipment = 'Y'", false);
qb.closeGroup();
qb.closeGroup();

// Result: WHERE (Plant = 'A' AND (Equipment = 'X' OR Equipment = 'Y'))
```

**Used in:**
- Report builders
- Advanced filters
- Search engines

### 4. UI Rule Builders

**Problem:** Building complex filter expressions in UI

**Example:** Advanced search panel (JavaScript)
```javascript
class RuleBuilder {
    constructor() {
        this.groups = [];
        this.openGroups = 0;
    }
    
    startGroup(operator) {
        if (this.canOpenGroup()) {
            this.groups.push({
                operator: operator,
                conditions: [],
                isOpen: true
            });
            this.openGroups++;
        }
    }
    
    addCondition(field, operator, value) {
        if (this.openGroups > 0) {
            const currentGroup = this.groups[this.groups.length - 1];
            currentGroup.conditions.push({ field, operator, value });
        }
    }
    
    endGroup() {
        if (this.canCloseGroup()) {
            this.groups[this.groups.length - 1].isOpen = false;
            this.openGroups--;
        }
    }
    
    canOpenGroup() {
        return this.openGroups < this.maxNesting;
    }
    
    canCloseGroup() {
        return this.openGroups > 0;
    }
}

// Result: (Plant = A AND (Equipment = X OR Equipment = Y))
```

**Examples:**
- Power BI filters
- SAP Fiori Smart Filters
- Advanced search panels

### 5. Workflow Engines (Backend)

**Problem:** Building rule engine conditions

**Example:** Feature flag conditions (Java)
```java
public class RuleEngine {
    private JsonObject buildCondition() {
        JsonObject root = new JsonObject();
        
        // Track nesting depth
        if (canNest()) {
            JsonObject andBlock = new JsonObject();
            andBlock.add("role", new JsonPrimitive("ADMIN"));
            
            // Nested OR condition
            if (canNest()) {
                JsonArray orBlock = new JsonArray();
                // ... add conditions
                andBlock.add("or", orBlock);
            }
            
            root.add("and", andBlock);
        }
        
        return root;
    }
}

// Result:
{
  "if": {
    "and": [
      { "role": "ADMIN" },
      { "or": [...] }
    ]
  }
}
```

**Used in:**
- BPMN workflows
- Rule engines
- Feature flags

### 6. HTML/XML Generator

**Problem:** Dynamically generating nested HTML structures

**Example:** Table builder (JavaScript)
```javascript
class HTMLBuilder {
    constructor() {
        this.html = "";
        this.openTags = [];
    }
    
    openTag(tagName) {
        if (this.canOpenTag()) {
            this.html += `<${tagName}>`;
            this.openTags.push(tagName);
        }
    }
    
    closeTag() {
        if (this.canCloseTag()) {
            const tag = this.openTags.pop();
            this.html += `</${tag}>`;
        }
    }
    
    canOpenTag() {
        return this.openTags.length < this.maxDepth;
    }
    
    canCloseTag() {
        return this.openTags.length > 0;
    }
}

// Usage
builder.openTag("table");
builder.openTag("tr");
builder.openTag("td");
builder.closeTag(); // </td>
builder.closeTag(); // </tr>
builder.closeTag(); // </table>
```

### 7. Configuration DSL Parser

**Problem:** Parsing nested configuration files

**Example:** Config validator (Java)
```java
public class ConfigValidator {
    public boolean isValid(String config) {
        int openBraces = 0;
        
        for (char c : config.toCharArray()) {
            if (c == '{') {
                openBraces++;
                if (openBraces > maxNesting) {
                    return false; // Too deep
                }
            } else if (c == '}') {
                openBraces--;
                if (openBraces < 0) {
                    return false; // Closing before opening
                }
            }
        }
        
        return openBraces == 0; // All closed
    }
}
```

---

## Why This Matters in Production

### Prevents Runtime Errors
```javascript
// Bad: No validation
const json = `{"data": {${userInput}}`; // Can break

// Good: Controlled generation
builder.openObject("data");
builder.addField("value", userInput);
builder.closeObject(); // Always valid
```

### Safe Construction
- Invalid states caught early
- No malformed structures
- Easier debugging
- Better error messages

### Practical Scenarios
- Dynamic form builders
- Report generators
- API clients with complex queries
- Configuration generators

---

## Interview Tip

When explaining this problem, say:

"This pattern is useful for dynamic JSON payload generation, query builders, UI filter engines, and chart configuration generation where nesting must always stay valid. It's about constraint-based generation - ensuring we never create invalid states while building complex nested structures."

This demonstrates senior-level understanding of state validation and safe construction patterns.

---

## Key Takeaway

Generate Parentheses is a blueprint for safely generating any nested, rule-bound structure - not just brackets. It teaches constraint validation at each step to prevent invalid states.