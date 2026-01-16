# Best Time to Buy and Sell Stock - Real World Applications

## Problem Statement

You are given an array prices where prices[i] is the price of a given stock on the ith day. You want to maximize your profit by choosing a single day to buy one stock and choosing a different day in the future to sell that stock. Return the maximum profit you can achieve from this transaction. If you cannot achieve any profit, return 0.

**Example 1:**
```
Input:  prices = [7,1,5,3,6,4]
Output: 5
Explanation: Buy on day 2 (price = 1) and sell on day 5 (price = 6), profit = 6-1 = 5
```

**Example 2:**
```
Input:  prices = [7,6,4,3,1]
Output: 0
Explanation: No profit possible (prices always decreasing)
```

**Example 3:**
```
Input:  prices = [2,4,1]
Output: 2
Explanation: Buy on day 1 (price = 2) and sell on day 2 (price = 4), profit = 2
```

**Constraints:**
- 1 <= prices.length <= 10^5
- 0 <= prices[i] <= 10^4

**Solution (Java):**
```java
// Approach 1: Single Pass (Optimal)
// Time: O(n), Space: O(1)
public int maxProfit(int[] prices) {
    if (prices == null || prices.length == 0) {
        return 0;
    }
    
    int minPrice = Integer.MAX_VALUE;
    int maxProfit = 0;
    
    for (int price : prices) {
        // Update minimum price seen so far
        if (price < minPrice) {
            minPrice = price;
        }
        // Calculate profit if we sell at current price
        else if (price - minPrice > maxProfit) {
            maxProfit = price - minPrice;
        }
    }
    
    return maxProfit;
}

// Approach 2: Track min and max explicitly
public int maxProfitExplicit(int[] prices) {
    int minPrice = Integer.MAX_VALUE;
    int maxProfit = 0;
    
    for (int i = 0; i < prices.length; i++) {
        minPrice = Math.min(minPrice, prices[i]);
        int currentProfit = prices[i] - minPrice;
        maxProfit = Math.max(maxProfit, currentProfit);
    }
    
    return maxProfit;
}

// Approach 3: Brute Force (for understanding)
// Time: O(n²), Space: O(1)
public int maxProfitBruteForce(int[] prices) {
    int maxProfit = 0;
    
    for (int i = 0; i < prices.length; i++) {
        for (int j = i + 1; j < prices.length; j++) {
            int profit = prices[j] - prices[i];
            maxProfit = Math.max(maxProfit, profit);
        }
    }
    
    return maxProfit;
}
```

---

## Core Concept

This problem is NOT just about stocks. It teaches:

- Tracking minimum/maximum in single pass
- Greedy algorithm (local optimal choices)
- Running calculations without storing history
- Peak-valley pattern recognition

Any place where you need to find optimal buy-sell points, track min-max differences, or analyze time-series data - this pattern applies.

---

## Real-World Use Cases

### 1. Stock Trading: Finding Best Trade Opportunity

**Problem:** Analyze historical stock data to find best single trade

**Example:** Stock analyzer (Backend - Java)
```java
public class StockAnalyzer {
    
    public TradeOpportunity findBestTrade(double[] prices) {
        double minPrice = Double.MAX_VALUE;
        double maxProfit = 0;
        int buyDay = 0;
        int sellDay = 0;
        int tempBuyDay = 0;
        
        for (int i = 0; i < prices.length; i++) {
            if (prices[i] < minPrice) {
                minPrice = prices[i];
                tempBuyDay = i;
            }
            
            double profit = prices[i] - minPrice;
            if (profit > maxProfit) {
                maxProfit = profit;
                buyDay = tempBuyDay;
                sellDay = i;
            }
        }
        
        return new TradeOpportunity(buyDay, sellDay, maxProfit);
    }
    
    public String getTradeAdvice(double[] prices) {
        TradeOpportunity best = findBestTrade(prices);
        return String.format("Buy on day %d at $%.2f, sell on day %d at $%.2f, profit: $%.2f",
            best.buyDay, prices[best.buyDay], 
            best.sellDay, prices[best.sellDay], 
            best.profit);
    }
}

// Used by: Robinhood, E*TRADE, trading platforms
```

**Use Case:**
- Stock trading platforms
- Investment analysis tools
- Backtesting trading strategies

### 2. E-Commerce: Price Drop Alerts

**Problem:** Find when to buy product at lowest price

**Example:** Price tracker (JavaScript)
```javascript
class PriceTracker {
    
    findBestBuyTime(priceHistory) {
        let minPrice = Infinity;
        let maxSavings = 0;
        let bestBuyDay = 0;
        let bestCurrentPrice = 0;
        
        for (let i = 0; i < priceHistory.length; i++) {
            const price = priceHistory[i].price;
            
            if (price < minPrice) {
                minPrice = price;
                bestBuyDay = i;
            }
            
            const savings = price - minPrice;
            if (savings > maxSavings) {
                maxSavings = savings;
                bestCurrentPrice = price;
            }
        }
        
        return {
            shouldHaveBought: priceHistory[bestBuyDay].date,
            atPrice: minPrice,
            currentPrice: bestCurrentPrice,
            missedSavings: maxSavings
        };
    }
    
    trackProduct(productId) {
        const history = this.getPriceHistory(productId);
        const analysis = this.findBestBuyTime(history);
        
        console.log(`Best time to buy was: ${analysis.shouldHaveBought}`);
        console.log(`Price then: $${analysis.atPrice}`);
        console.log(`You could have saved: $${analysis.missedSavings}`);
    }
}

// Amazon, CamelCamelCamel price tracking
```

**Use Case:**
- Price comparison websites
- Shopping deal alerts
- Best time to buy notifications

### 3. Hotel Booking: Find Cheapest Booking Window

**Problem:** Determine optimal booking time for hotel rooms

**Example:** Hotel price analyzer (Backend - Java)
```java
public class HotelPriceAnalyzer {
    
    public BookingAdvice findBestBookingTime(double[] dailyRates) {
        double lowestRate = Double.MAX_VALUE;
        double maxDifference = 0;
        int bookDay = 0;
        int checkInDay = 0;
        
        for (int i = 0; i < dailyRates.length; i++) {
            if (dailyRates[i] < lowestRate) {
                lowestRate = dailyRates[i];
                bookDay = i;
            }
            
            double difference = dailyRates[i] - lowestRate;
            if (difference > maxDifference) {
                maxDifference = difference;
                checkInDay = i;
            }
        }
        
        return new BookingAdvice(
            bookDay, 
            lowestRate, 
            checkInDay, 
            dailyRates[checkInDay],
            maxDifference
        );
    }
}

// Booking.com, Hotels.com use similar analysis
```

**Use Case:**
- Hotel booking platforms
- Travel price alerts
- Dynamic pricing analysis

### 4. Cloud Computing: Spot Instance Pricing

**Problem:** Find best time to purchase cloud spot instances

**Example:** Spot instance optimizer (Backend - Java)
```java
public class SpotInstanceOptimizer {
    
    public SpotPurchaseAdvice analyzePricing(double[] hourlyPrices) {
        double minPrice = Double.MAX_VALUE;
        double maxSavings = 0;
        int purchaseHour = 0;
        int terminateHour = 0;
        
        for (int i = 0; i < hourlyPrices.length; i++) {
            if (hourlyPrices[i] < minPrice) {
                minPrice = hourlyPrices[i];
                purchaseHour = i;
            }
            
            double savings = hourlyPrices[i] - minPrice;
            if (savings > maxSavings) {
                maxSavings = savings;
                terminateHour = i;
            }
        }
        
        return new SpotPurchaseAdvice(
            purchaseHour,
            minPrice,
            terminateHour,
            maxSavings
        );
    }
}

// AWS EC2 Spot Instances, Google Cloud Preemptible VMs
```

**Use Case:**
- AWS spot instance optimization
- Cloud cost reduction
- Resource scheduling

### 5. Cryptocurrency: Finding Best Exchange Rate

**Problem:** Find optimal time to exchange currency

**Example:** Crypto exchange analyzer (JavaScript)
```javascript
class CryptoExchangeAnalyzer {
    
    findBestExchangeTime(exchangeRates) {
        let lowestRate = Infinity;
        let maxProfit = 0;
        let buyTime = null;
        let sellTime = null;
        
        for (const rate of exchangeRates) {
            if (rate.price < lowestRate) {
                lowestRate = rate.price;
                buyTime = rate.timestamp;
            }
            
            const profit = rate.price - lowestRate;
            if (profit > maxProfit) {
                maxProfit = profit;
                sellTime = rate.timestamp;
            }
        }
        
        return {
            buyAt: buyTime,
            buyPrice: lowestRate,
            sellAt: sellTime,
            profit: maxProfit,
            profitPercent: (maxProfit / lowestRate) * 100
        };
    }
}

// Binance, Coinbase exchange analytics
```

**Use Case:**
- Cryptocurrency trading
- Exchange rate monitoring
- Arbitrage opportunities

### 6. Energy Management: Electricity Price Optimization

**Problem:** Find cheapest time to consume electricity

**Example:** Energy optimizer (Backend - Java)
```java
public class EnergyOptimizer {
    
    public EnergySchedule findOptimalUsage(double[] hourlyRates) {
        double lowestRate = Double.MAX_VALUE;
        double maxCostDiff = 0;
        int startHour = 0;
        int peakHour = 0;
        
        for (int i = 0; i < hourlyRates.length; i++) {
            if (hourlyRates[i] < lowestRate) {
                lowestRate = hourlyRates[i];
                startHour = i;
            }
            
            double costDiff = hourlyRates[i] - lowestRate;
            if (costDiff > maxCostDiff) {
                maxCostDiff = costDiff;
                peakHour = i;
            }
        }
        
        return new EnergySchedule(
            startHour,
            lowestRate,
            peakHour,
            hourlyRates[peakHour],
            maxCostDiff
        );
    }
    
    public String getRecommendation(double[] rates) {
        EnergySchedule schedule = findOptimalUsage(rates);
        return String.format(
            "Run heavy appliances at hour %d (rate: $%.2f/kWh). " +
            "Avoid hour %d (rate: $%.2f/kWh). Save up to $%.2f/kWh",
            schedule.bestHour, schedule.lowestRate,
            schedule.worstHour, schedule.highestRate,
            schedule.maxSavings
        );
    }
}

// Smart home systems, industrial energy management
```

**Use Case:**
- Smart home automation
- Industrial power management
- EV charging optimization

### 7. Ride Sharing: Surge Pricing Analysis

**Problem:** Find best time to book rides avoiding surge pricing

**Example:** Ride price analyzer (JavaScript)
```javascript
class RidePriceAnalyzer {
    
    findBestRideTime(hourlyPrices) {
        let minPrice = Infinity;
        let maxPriceDiff = 0;
        let bestTime = null;
        let worstTime = null;
        
        for (const data of hourlyPrices) {
            if (data.price < minPrice) {
                minPrice = data.price;
                bestTime = data.hour;
            }
            
            const priceDiff = data.price - minPrice;
            if (priceDiff > maxPriceDiff) {
                maxPriceDiff = priceDiff;
                worstTime = data.hour;
            }
        }
        
        return {
            bookAt: bestTime,
            lowestPrice: minPrice,
            avoidAt: worstTime,
            surgePrice: minPrice + maxPriceDiff,
            savings: maxPriceDiff
        };
    }
}

// Uber, Lyft pricing optimization
```

**Use Case:**
- Ride-sharing apps
- Dynamic pricing alerts
- Travel cost optimization

### 8. Inventory Management: Purchase Timing

**Problem:** Determine optimal time to purchase inventory

**Example:** Inventory purchase optimizer (Backend - Java)
```java
public class InventoryOptimizer {
    
    public PurchaseRecommendation optimizePurchase(double[] supplierPrices) {
        double lowestPrice = Double.MAX_VALUE;
        double maxMarkup = 0;
        int purchaseWeek = 0;
        int sellWeek = 0;
        
        for (int i = 0; i < supplierPrices.length; i++) {
            if (supplierPrices[i] < lowestPrice) {
                lowestPrice = supplierPrices[i];
                purchaseWeek = i;
            }
            
            double markup = supplierPrices[i] - lowestPrice;
            if (markup > maxMarkup) {
                maxMarkup = markup;
                sellWeek = i;
            }
        }
        
        return new PurchaseRecommendation(
            purchaseWeek,
            lowestPrice,
            sellWeek,
            supplierPrices[sellWeek],
            maxMarkup
        );
    }
}

// Walmart, Target inventory systems
```

**Use Case:**
- Retail inventory management
- Wholesale purchasing
- Supply chain optimization

### 9. Real Estate: Property Price Trends

**Problem:** Analyze when property was cheapest vs current value

**Example:** Real estate analyzer (JavaScript)
```javascript
class RealEstateAnalyzer {
    
    analyzePropertyValue(priceHistory) {
        let lowestPrice = Infinity;
        let maxAppreciation = 0;
        let bestBuyDate = null;
        let currentDate = null;
        
        for (const record of priceHistory) {
            if (record.price < lowestPrice) {
                lowestPrice = record.price;
                bestBuyDate = record.date;
            }
            
            const appreciation = record.price - lowestPrice;
            if (appreciation > maxAppreciation) {
                maxAppreciation = appreciation;
                currentDate = record.date;
            }
        }
        
        return {
            shouldHaveBought: bestBuyDate,
            atPrice: lowestPrice,
            currentValue: lowestPrice + maxAppreciation,
            appreciation: maxAppreciation,
            appreciationPercent: (maxAppreciation / lowestPrice) * 100
        };
    }
}

// Zillow, Redfin property analytics
```

**Use Case:**
- Real estate platforms
- Property investment analysis
- Market trend analysis

### 10. Data Analytics: Peak-Valley Detection

**Problem:** Find minimum and maximum points in time-series data

**Example:** Metric analyzer (Backend - Java)
```java
public class MetricAnalyzer {
    
    public MetricInsight analyzeMetric(double[] values) {
        double minValue = Double.MAX_VALUE;
        double maxVariance = 0;
        int minIndex = 0;
        int maxIndex = 0;
        
        for (int i = 0; i < values.length; i++) {
            if (values[i] < minValue) {
                minValue = values[i];
                minIndex = i;
            }
            
            double variance = values[i] - minValue;
            if (variance > maxVariance) {
                maxVariance = variance;
                maxIndex = i;
            }
        }
        
        return new MetricInsight(
            minIndex,
            minValue,
            maxIndex,
            values[maxIndex],
            maxVariance
        );
    }
}

// Used in monitoring systems: Datadog, New Relic
```

**Use Case:**
- Performance monitoring
- System metrics analysis
- Anomaly detection

---

## Why This Matters in Production

### Time Complexity
```
Brute Force: O(n²)
- Check every buy-sell pair
- 10,000 data points = 100,000,000 operations

Optimized: O(n)
- Single pass through data
- 10,000 data points = 10,000 operations
- 10,000x faster!
```

### Space Efficiency
```
Only 2 variables needed:
- minPrice: track minimum so far
- maxProfit: track best profit

Space: O(1) - constant memory
Works for millions of data points
```

### Real-World Performance
- **Stock Platforms:** Process millions of trades per second
- **E-commerce:** Track billions of price points
- **Cloud Services:** Optimize costs for thousands of customers
- **Energy Systems:** Real-time pricing for smart grids

---

## Interview Tip

When explaining this problem, say:

"Best Time to Buy and Sell Stock teaches the greedy approach to finding optimal buy-sell points in time-series data. While it looks like a stock problem, it's actually about tracking minimum values and maximum differences in a single pass - O(n) time, O(1) space. This pattern is used everywhere: stock trading platforms, e-commerce price tracking, hotel booking optimization, cloud spot instance pricing, energy management, and any system analyzing time-series data for optimal decision points. The key insight is maintaining the minimum seen so far and calculating profit at each step, without needing to store history."

This demonstrates understanding of both the algorithm and its broad applicability to time-series optimization.

---

## Key Takeaway

Best Time to Buy and Sell Stock is a blueprint for single-pass min-max analysis. The pattern of tracking minimum value and calculating maximum difference applies to stock trading, price tracking, resource optimization, energy management, and any domain requiring optimal timing decisions in time-series data - all with O(n) time and O(1) space efficiency.