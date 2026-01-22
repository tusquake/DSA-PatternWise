# Subsets - Real World Applications

## Problem Statement

Given an integer array nums of unique elements, return all possible subsets (the power set). The solution set must not contain duplicate subsets. Return the solution in any order.

**Example 1:**
```
Input:  nums = [1,2,3]
Output: [[],[1],[2],[1,2],[3],[1,3],[2,3],[1,2,3]]
```

**Example 2:**
```
Input:  nums = [0]
Output: [[],[0]]
```

**Example 3:**
```
Input:  nums = [1,2]
Output: [[],[1],[2],[1,2]]
```

**Constraints:**
- 1 <= nums.length <= 10
- -10 <= nums[i] <= 10
- All the numbers of nums are unique

**Solution (Java):**
```java
// Approach 1: Backtracking (Most intuitive)
// Time: O(n * 2^n), Space: O(n) for recursion
public List<List<Integer>> subsets(int[] nums) {
    List<List<Integer>> result = new ArrayList<>();
    backtrack(nums, 0, new ArrayList<>(), result);
    return result;
}

private void backtrack(int[] nums, int start, 
                       List<Integer> current, 
                       List<List<Integer>> result) {
    // Add current subset
    result.add(new ArrayList<>(current));
    
    // Try adding each remaining element
    for (int i = start; i < nums.length; i++) {
        current.add(nums[i]);              // Include nums[i]
        backtrack(nums, i + 1, current, result);  // Recurse
        current.remove(current.size() - 1); // Backtrack
    }
}

// Approach 2: Iterative (Building subsets incrementally)
// Time: O(n * 2^n), Space: O(1) excluding output
public List<List<Integer>> subsetsIterative(int[] nums) {
    List<List<Integer>> result = new ArrayList<>();
    result.add(new ArrayList<>()); // Start with empty subset
    
    for (int num : nums) {
        int size = result.size();
        // For each existing subset, create new subset by adding num
        for (int i = 0; i < size; i++) {
            List<Integer> newSubset = new ArrayList<>(result.get(i));
            newSubset.add(num);
            result.add(newSubset);
        }
    }
    
    return result;
}

// Approach 3: Bit Manipulation
// Time: O(n * 2^n), Space: O(1) excluding output
public List<List<Integer>> subsetsBitMask(int[] nums) {
    List<List<Integer>> result = new ArrayList<>();
    int n = nums.length;
    int totalSubsets = 1 << n; // 2^n
    
    // Generate all binary numbers from 0 to 2^n - 1
    for (int mask = 0; mask < totalSubsets; mask++) {
        List<Integer> subset = new ArrayList<>();
        
        // Check each bit
        for (int i = 0; i < n; i++) {
            // If i-th bit is set, include nums[i]
            if ((mask & (1 << i)) != 0) {
                subset.add(nums[i]);
            }
        }
        
        result.add(subset);
    }
    
    return result;
}

// Approach 4: Cascading (Similar to iterative but clearer)
// Time: O(n * 2^n), Space: O(1) excluding output
public List<List<Integer>> subsetsCascading(int[] nums) {
    List<List<Integer>> subsets = new ArrayList<>();
    subsets.add(new ArrayList<>());
    
    for (int num : nums) {
        List<List<Integer>> newSubsets = new ArrayList<>();
        
        for (List<Integer> subset : subsets) {
            List<Integer> newSubset = new ArrayList<>(subset);
            newSubset.add(num);
            newSubsets.add(newSubset);
        }
        
        subsets.addAll(newSubsets);
    }
    
    return subsets;
}
```

---

## Core Concept

This problem is NOT just about generating subsets. It teaches:

- Backtracking algorithm
- Power set generation
- Combinatorial enumeration
- Bit manipulation for combinations

Any place where you need to generate all combinations, explore configuration options, test scenarios, or enumerate possibilities - this pattern applies.

---

## Real-World Use Cases

### 1. E-Commerce: Product Bundle Generation

**Problem:** Generate all possible product bundles for promotional offers

**Example:** Bundle generator (Backend - Java)
```java
public class ProductBundleGenerator {
    
    public List<List<Product>> generateBundles(Product[] products) {
        List<List<Product>> bundles = new ArrayList<>();
        generateBundlesHelper(products, 0, new ArrayList<>(), bundles);
        return bundles;
    }
    
    private void generateBundlesHelper(Product[] products, int start,
                                       List<Product> current,
                                       List<List<Product>> bundles) {
        // Add current bundle
        bundles.add(new ArrayList<>(current));
        
        for (int i = start; i < products.length; i++) {
            current.add(products[i]);
            generateBundlesHelper(products, i + 1, current, bundles);
            current.remove(current.size() - 1);
        }
    }
    
    public List<BundleOffer> createPromotions(Product[] products) {
        List<List<Product>> allBundles = generateBundles(products);
        List<BundleOffer> offers = new ArrayList<>();
        
        for (List<Product> bundle : allBundles) {
            if (bundle.size() >= 2) { // Only bundles with 2+ items
                double totalPrice = bundle.stream()
                    .mapToDouble(Product::getPrice)
                    .sum();
                double discount = totalPrice * 0.1 * bundle.size(); // 10% per item
                
                offers.add(new BundleOffer(
                    bundle, 
                    totalPrice, 
                    discount,
                    "Save $" + discount + " on this bundle!"
                ));
            }
        }
        
        return offers;
    }
}

// Amazon "Frequently bought together"
// Walmart bundle deals generation
```

**Use Case:**
- Product bundling strategies
- Promotional offer creation
- Cross-selling optimization

### 2. Menu Planning: Meal Combinations

**Problem:** Generate all possible meal combinations from available ingredients

**Example:** Meal combo generator (JavaScript)
```javascript
class MealComboGenerator {
    
    generateCombinations(ingredients) {
        const combos = [];
        this.backtrack(ingredients, 0, [], combos);
        return combos;
    }
    
    backtrack(ingredients, start, current, combos) {
        combos.push([...current]);
        
        for (let i = start; i < ingredients.length; i++) {
            current.push(ingredients[i]);
            this.backtrack(ingredients, i + 1, current, combos);
            current.pop();
        }
    }
    
    generateMealPlans(ingredients) {
        const allCombos = this.generateCombinations(ingredients);
        const meals = [];
        
        for (const combo of allCombos) {
            if (combo.length >= 3) { // Minimum 3 ingredients for a meal
                const calories = combo.reduce((sum, ing) => sum + ing.calories, 0);
                const protein = combo.reduce((sum, ing) => sum + ing.protein, 0);
                
                meals.push({
                    ingredients: combo.map(i => i.name),
                    nutrition: {
                        calories: calories,
                        protein: protein
                    },
                    category: this.categorize(calories)
                });
            }
        }
        
        return meals;
    }
    
    categorize(calories) {
        if (calories < 300) return "Light meal";
        if (calories < 600) return "Regular meal";
        return "Heavy meal";
    }
}

// HelloFresh, BlueApron meal planning
// Restaurant menu optimization
```

**Use Case:**
- Meal kit services
- Restaurant menu creation
- Dietary planning apps

### 3. Cloud Infrastructure: Resource Configuration Testing

**Problem:** Test all possible combinations of cloud resources

**Example:** Configuration tester (Backend - Java)
```java
public class CloudConfigTester {
    
    public List<List<String>> generateConfigs(String[] resources) {
        List<List<String>> configs = new ArrayList<>();
        generateConfigsHelper(resources, 0, new ArrayList<>(), configs);
        return configs;
    }
    
    private void generateConfigsHelper(String[] resources, int start,
                                       List<String> current,
                                       List<List<String>> configs) {
        configs.add(new ArrayList<>(current));
        
        for (int i = start; i < resources.length; i++) {
            current.add(resources[i]);
            generateConfigsHelper(resources, i + 1, current, configs);
            current.remove(current.size() - 1);
        }
    }
    
    public List<TestResult> testAllConfigurations(String[] services) {
        // Services: ["Redis", "PostgreSQL", "ElasticSearch", "RabbitMQ"]
        List<List<String>> allConfigs = generateConfigs(services);
        List<TestResult> results = new ArrayList<>();
        
        for (List<String> config : allConfigs) {
            if (config.isEmpty()) continue; // Skip empty config
            
            boolean compatible = checkCompatibility(config);
            double cost = calculateCost(config);
            
            results.add(new TestResult(
                config,
                compatible,
                cost,
                compatible ? "VALID" : "INCOMPATIBLE"
            ));
        }
        
        return results;
    }
}

// AWS, Azure infrastructure testing
// Terraform configuration validation
```

**Use Case:**
- Infrastructure testing
- DevOps automation
- Configuration validation

### 4. Feature Flag Combinations: A/B Testing

**Problem:** Generate all combinations of feature flags for testing

**Example:** Feature flag tester (JavaScript)
```javascript
class FeatureFlagTester {
    
    generateFlagCombinations(features) {
        const combinations = [];
        this.backtrack(features, 0, [], combinations);
        return combinations;
    }
    
    backtrack(features, start, current, combinations) {
        combinations.push([...current]);
        
        for (let i = start; i < features.length; i++) {
            current.push(features[i]);
            this.backtrack(features, i + 1, current, combinations);
            current.pop();
        }
    }
    
    generateTestScenarios(flags) {
        // flags = ["darkMode", "newCheckout", "aiRecommendations"]
        const allCombos = this.generateFlagCombinations(flags);
        const scenarios = [];
        
        for (const combo of allCombos) {
            const scenario = {
                enabledFlags: combo,
                disabledFlags: flags.filter(f => !combo.includes(f)),
                testGroup: `Group_${scenarios.length}`,
                userPercentage: combo.length === 0 ? 10 : 
                                combo.length === flags.length ? 10 : 
                                80 / (allCombos.length - 2)
            };
            
            scenarios.push(scenario);
        }
        
        return scenarios;
    }
}

// Optimizely, LaunchDarkly feature testing
// Google Optimize A/B testing
```

**Use Case:**
- A/B testing platforms
- Feature rollout strategies
- Experiment design

### 5. Team Formation: Project Team Combinations

**Problem:** Generate all possible team combinations from employee pool

**Example:** Team builder (Backend - Java)
```java
public class TeamBuilder {
    
    public List<List<Employee>> generateTeams(Employee[] employees) {
        List<List<Employee>> teams = new ArrayList<>();
        generateTeamsHelper(employees, 0, new ArrayList<>(), teams);
        return teams;
    }
    
    private void generateTeamsHelper(Employee[] employees, int start,
                                     List<Employee> current,
                                     List<List<Employee>> teams) {
        teams.add(new ArrayList<>(current));
        
        for (int i = start; i < employees.length; i++) {
            current.add(employees[i]);
            generateTeamsHelper(employees, i + 1, current, teams);
            current.remove(current.size() - 1);
        }
    }
    
    public List<TeamProposal> proposeTeams(Employee[] pool, int minSize) {
        List<List<Employee>> allTeams = generateTeams(pool);
        List<TeamProposal> proposals = new ArrayList<>();
        
        for (List<Employee> team : allTeams) {
            if (team.size() >= minSize && team.size() <= 5) {
                // Check skill diversity
                Set<String> skills = new HashSet<>();
                for (Employee emp : team) {
                    skills.addAll(emp.getSkills());
                }
                
                proposals.add(new TeamProposal(
                    team,
                    skills.size(),
                    calculateSynergy(team),
                    skills.size() >= 5 ? "BALANCED" : "LIMITED_SKILLS"
                ));
            }
        }
        
        return proposals;
    }
}

// HR systems: Team formation
// Project management tools: Resource allocation
```

**Use Case:**
- HR team building
- Project resource allocation
- Skill matrix optimization

### 6. Test Case Generation: Input Combinations

**Problem:** Generate all test input combinations for software testing

**Example:** Test case generator (JavaScript)
```javascript
class TestCaseGenerator {
    
    generateInputCombinations(parameters) {
        const combinations = [];
        this.backtrack(parameters, 0, [], combinations);
        return combinations;
    }
    
    backtrack(parameters, start, current, combinations) {
        combinations.push([...current]);
        
        for (let i = start; i < parameters.length; i++) {
            current.push(parameters[i]);
            this.backtrack(parameters, i + 1, current, combinations);
            current.pop();
        }
    }
    
    generateTestCases(apiParams) {
        // apiParams = ["sortBy", "filterType", "pageSize", "includeMetadata"]
        const allCombos = this.generateInputCombinations(apiParams);
        const testCases = [];
        
        for (const combo of allCombos) {
            const testCase = {
                parameters: combo,
                endpoint: "/api/data",
                expectedBehavior: this.determineExpectedBehavior(combo),
                priority: combo.length === 0 ? "HIGH" : 
                         combo.length === apiParams.length ? "HIGH" : 
                         "MEDIUM",
                description: `Test with ${combo.length} parameters: ${combo.join(', ')}`
            };
            
            testCases.push(testCase);
        }
        
        return testCases;
    }
    
    determineExpectedBehavior(params) {
        if (params.length === 0) return "Return default results";
        if (params.includes("sortBy") && params.includes("filterType")) {
            return "Apply both sort and filter";
        }
        return "Apply specified parameters";
    }
}

// Selenium, Cypress test automation
// API testing frameworks
```

**Use Case:**
- Automated testing
- QA test coverage
- API validation

---

## Why This Matters in Production

### Power Set Size
```
For n elements:
Total subsets = 2^n

Examples:
3 items → 8 subsets (2^3)
5 items → 32 subsets (2^5)
10 items → 1024 subsets (2^10)

Warning: Grows exponentially!
Constraint: n ≤ 10 typically
```

### Three Approaches Compared
```
1. Backtracking:
   - Most intuitive
   - Easy to add constraints
   - Natural recursion

2. Iterative:
   - No recursion overhead
   - Build incrementally
   - Easy to understand flow

3. Bit Manipulation:
   - Clever mathematical approach
   - Each bit = include/exclude decision
   - Fast but less intuitive
```

### Bit Manipulation Explained
```
For [1,2,3]:
Binary 000 → []
Binary 001 → [1]
Binary 010 → [2]
Binary 011 → [1,2]
Binary 100 → [3]
Binary 101 → [1,3]
Binary 110 → [2,3]
Binary 111 → [1,2,3]

Each bit position = one element
```

### Real-World Constraints
- **E-Commerce:** 10 products → 1024 bundles (manageable)
- **Meal Planning:** 8 ingredients → 256 meals
- **Cloud Config:** 6 services → 64 configurations
- **Feature Flags:** 5 flags → 32 test scenarios
- **Team Building:** 7 employees → 128 teams
- **Test Cases:** 6 parameters → 64 test cases

---

## Interview Tip

When explaining this problem, say:

"Subsets teaches generating the power set using backtracking, where we make include/exclude decisions for each element. The pattern explores all 2^n combinations by recursively building subsets and backtracking. This is fundamental to e-commerce product bundling for promotional offers, meal planning services for combination generation, cloud infrastructure testing for configuration validation, A/B testing platforms for feature flag scenarios, HR systems for team formation, and test automation for exhaustive test case generation. The constraint n ≤ 10 exists because 2^10 = 1024 is manageable, but 2^20 = 1 million becomes impractical. Understanding when to use backtracking vs iterative vs bit manipulation depends on constraints and readability needs."

This demonstrates combinatorial thinking and awareness of exponential growth limitations.

---

## Key Takeaway

Subsets is a blueprint for generating all combinations using backtracking. The pattern of making include/exclude decisions for each element applies to product bundling, meal planning, infrastructure testing, feature flag combinations, team formation, and test case generation - any domain requiring exhaustive enumeration of possibilities within exponential growth constraints (typically n ≤ 10-15 elements).