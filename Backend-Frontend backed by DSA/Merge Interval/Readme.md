# Merge Intervals - Real World Applications

## Problem Statement

Given an array of intervals where intervals[i] = [start, end], merge all overlapping intervals and return an array of the non-overlapping intervals.

**Example 1:**
```
Input:  [[1,3], [2,6], [8,10], [15,18]]
Output: [[1,6], [8,10], [15,18]]
Explanation: Intervals [1,3] and [2,6] overlap, so merge them into [1,6]
```

**Example 2:**
```
Input:  [[1,4], [4,5]]
Output: [[1,5]]
Explanation: Intervals [1,4] and [4,5] are considered overlapping
```

**Example 3:**
```
Input:  [[1,4], [0,4]]
Output: [[0,4]]
```

**Constraints:**
- 1 <= intervals.length <= 10^4
- intervals[i].length == 2
- 0 <= start <= end <= 10^4

**Solution (Java):**
```java
public int[][] merge(int[][] intervals) {
    if (intervals.length <= 1) {
        return intervals;
    }
    
    // Sort intervals by start time
    Arrays.sort(intervals, (a, b) -> Integer.compare(a[0], b[0]));
    
    List<int[]> result = new ArrayList<>();
    int[] currentInterval = intervals[0];
    result.add(currentInterval);
    
    for (int[] interval : intervals) {
        int currentEnd = currentInterval[1];
        int nextStart = interval[0];
        int nextEnd = interval[1];
        
        // Check if intervals overlap
        if (currentEnd >= nextStart) {
            // Merge intervals
            currentInterval[1] = Math.max(currentEnd, nextEnd);
        } else {
            // No overlap, add new interval
            currentInterval = interval;
            result.add(currentInterval);
        }
    }
    
    return result.toArray(new int[result.size()][]);
}
```

---

## Core Concept

This problem is NOT about intervals. It teaches:

- Detecting and merging overlapping ranges
- Sorting for efficient comparison
- Consolidating fragmented data
- Range optimization

Any place where you need to merge overlapping time slots, consolidate ranges, or optimize fragmented resources - this pattern applies.

---

## Real-World Use Cases

### 1. Calendar/Meeting Room Scheduling

**Problem:** Merge overlapping meeting times to find actual busy periods

**Example:** Calendar availability checker (Backend - Java)
```java
public class CalendarScheduler {
    
    public List<TimeSlot> findBusyPeriods(List<Meeting> meetings) {
        // Convert meetings to intervals
        int[][] intervals = new int[meetings.size()][2];
        for (int i = 0; i < meetings.size(); i++) {
            intervals[i][0] = meetings.get(i).getStartTime();
            intervals[i][1] = meetings.get(i).getEndTime();
        }
        
        // Sort by start time
        Arrays.sort(intervals, (a, b) -> a[0] - b[0]);
        
        List<TimeSlot> busyPeriods = new ArrayList<>();
        int[] current = intervals[0];
        
        for (int[] meeting : intervals) {
            if (current[1] >= meeting[0]) {
                // Overlapping meetings - extend busy period
                current[1] = Math.max(current[1], meeting[1]);
            } else {
                // No overlap - new busy period
                busyPeriods.add(new TimeSlot(current[0], current[1]));
                current = meeting;
            }
        }
        busyPeriods.add(new TimeSlot(current[0], current[1]));
        
        return busyPeriods;
    }
}

// Meetings: [9-10], [9:30-11], [14-15], [14:30-16]
// Result: [9-11], [14-16] (merged overlapping times)
```

**Use Case:**
- Google Calendar availability
- Meeting room booking systems
- Resource scheduling

### 2. Server Downtime Tracking

**Problem:** Consolidate overlapping downtime periods for reporting

**Example:** Uptime monitor (Backend - Java)
```java
public class UptimeMonitor {
    
    public List<DowntimePeriod> consolidateDowntime(
            List<DowntimeEvent> events) {
        
        int[][] intervals = new int[events.size()][2];
        for (int i = 0; i < events.size(); i++) {
            intervals[i][0] = events.get(i).getStartTimestamp();
            intervals[i][1] = events.get(i).getEndTimestamp();
        }
        
        Arrays.sort(intervals, (a, b) -> a[0] - b[0]);
        
        List<DowntimePeriod> consolidated = new ArrayList<>();
        int[] current = intervals[0];
        
        for (int[] event : intervals) {
            if (current[1] >= event[0]) {
                current[1] = Math.max(current[1], event[1]);
            } else {
                consolidated.add(
                    new DowntimePeriod(current[0], current[1])
                );
                current = event;
            }
        }
        consolidated.add(new DowntimePeriod(current[0], current[1]));
        
        return consolidated;
    }
}

// Multiple small outages → Single consolidated downtime report
```

**Use Case:**
- SLA calculations
- Uptime reporting
- Infrastructure monitoring

### 3. Video Streaming: Buffer Management

**Problem:** Merge buffered video segments for continuous playback

**Example:** Video buffer optimizer (JavaScript)
```javascript
function mergeBufferedRanges(bufferedSegments) {
    if (bufferedSegments.length <= 1) return bufferedSegments;
    
    // Sort by start time
    bufferedSegments.sort((a, b) => a.start - b.start);
    
    const merged = [bufferedSegments[0]];
    
    for (let i = 1; i < bufferedSegments.length; i++) {
        const current = merged[merged.length - 1];
        const next = bufferedSegments[i];
        
        if (current.end >= next.start) {
            // Overlapping segments - extend range
            current.end = Math.max(current.end, next.end);
        } else {
            // Gap in buffer - add new segment
            merged.push(next);
        }
    }
    
    return merged;
}

// Buffered: [0-10s], [5-15s], [20-30s]
// Result: [0-15s], [20-30s]
```

**Use Case:**
- YouTube/Netflix buffering
- Audio streaming
- Progressive download optimization

### 4. Database: Query Optimization (Index Range Scans)

**Problem:** Merge overlapping index scan ranges to reduce database queries

**Example:** Query optimizer (Backend - Java)
```java
public class QueryOptimizer {
    
    public List<Range> optimizeIndexScans(List<Range> ranges) {
        if (ranges.isEmpty()) return ranges;
        
        ranges.sort(Comparator.comparingInt(r -> r.start));
        
        List<Range> optimized = new ArrayList<>();
        Range current = ranges.get(0);
        
        for (Range range : ranges) {
            if (current.end >= range.start) {
                // Merge overlapping ranges
                current.end = Math.max(current.end, range.end);
            } else {
                optimized.add(current);
                current = range;
            }
        }
        optimized.add(current);
        
        return optimized;
    }
}

// Queries: WHERE id BETWEEN 1-100, 50-150, 200-300
// Optimized: WHERE id BETWEEN 1-150, 200-300
```

**Use Case:**
- SQL query optimization
- Index range merging
- Reducing database round trips

### 5. Network Traffic: Bandwidth Allocation

**Problem:** Consolidate overlapping bandwidth usage periods

**Example:** Bandwidth monitor (JavaScript)
```javascript
function consolidateUsagePeriods(usagePeriods) {
    if (usagePeriods.length === 0) return [];
    
    usagePeriods.sort((a, b) => a.startTime - b.startTime);
    
    const consolidated = [usagePeriods[0]];
    
    for (const period of usagePeriods) {
        const last = consolidated[consolidated.length - 1];
        
        if (last.endTime >= period.startTime) {
            last.endTime = Math.max(last.endTime, period.endTime);
            last.bandwidth = Math.max(last.bandwidth, period.bandwidth);
        } else {
            consolidated.push(period);
        }
    }
    
    return consolidated;
}

// Usage: [8am-10am: 50MB], [9am-11am: 80MB], [2pm-4pm: 30MB]
// Result: [8am-11am: 80MB], [2pm-4pm: 30MB]
```

**Use Case:**
- Network monitoring
- Billing systems
- Capacity planning

### 6. E-Commerce: Sale Periods

**Problem:** Merge overlapping discount periods to avoid conflicts

**Example:** Discount manager (Backend - Java)
```java
public class DiscountManager {
    
    public List<SalePeriod> mergeSalePeriods(List<SalePeriod> sales) {
        if (sales.isEmpty()) return sales;
        
        sales.sort(Comparator.comparingLong(s -> s.getStartDate()));
        
        List<SalePeriod> merged = new ArrayList<>();
        SalePeriod current = sales.get(0);
        
        for (SalePeriod sale : sales) {
            if (current.getEndDate() >= sale.getStartDate()) {
                // Overlapping sales - take max discount
                current.setEndDate(
                    Math.max(current.getEndDate(), sale.getEndDate())
                );
                current.setDiscount(
                    Math.max(current.getDiscount(), sale.getDiscount())
                );
            } else {
                merged.add(current);
                current = sale;
            }
        }
        merged.add(current);
        
        return merged;
    }
}

// Sales: [Black Friday: 20%], [Cyber Monday: 25%], [Xmas: 30%]
// If overlapping → merge with best discount
```

**Use Case:**
- Promotional campaigns
- Pricing engines
- Coupon validation

### 7. Manufacturing: Production Schedules

**Problem:** Optimize machine usage by merging overlapping production runs

**Example:** Production scheduler (Backend - Java)
```java
public class ProductionScheduler {
    
    public List<ProductionSlot> optimizeSchedule(
            List<ProductionRun> runs) {
        
        runs.sort(Comparator.comparingInt(r -> r.getStartTime()));
        
        List<ProductionSlot> optimized = new ArrayList<>();
        ProductionRun current = runs.get(0);
        
        for (ProductionRun run : runs) {
            if (current.getEndTime() >= run.getStartTime()) {
                // Can combine production runs
                current.setEndTime(
                    Math.max(current.getEndTime(), run.getEndTime())
                );
            } else {
                optimized.add(new ProductionSlot(current));
                current = run;
            }
        }
        optimized.add(new ProductionSlot(current));
        
        return optimized;
    }
}

// Runs: [Machine A: 9-11], [Machine A: 10-12], [Machine A: 14-16]
// Optimized: [9-12], [14-16] (reduced setup times)
```

**Use Case:**
- Factory scheduling
- Equipment utilization
- Batch processing

### 8. Healthcare: Patient Treatment Periods

**Problem:** Consolidate overlapping medication or treatment schedules

**Example:** Treatment scheduler (JavaScript)
```javascript
function mergeTreatmentPeriods(treatments) {
    if (treatments.length === 0) return [];
    
    treatments.sort((a, b) => a.startDate - b.startDate);
    
    const merged = [treatments[0]];
    
    for (const treatment of treatments) {
        const last = merged[merged.length - 1];
        
        if (last.endDate >= treatment.startDate) {
            // Overlapping treatments - check for interactions
            last.endDate = Math.max(last.endDate, treatment.endDate);
            last.medications = [...last.medications, ...treatment.medications];
        } else {
            merged.push(treatment);
        }
    }
    
    return merged;
}

// Tracks concurrent medications to detect potential interactions
```

**Use Case:**
- Patient care management
- Drug interaction checking
- Treatment planning

---

## Why This Matters in Production

### Time Complexity
```java
// Without sorting: O(n²) - compare every interval with every other
// With sorting: O(n log n) - sort once, then single pass O(n)
```

### Performance Impact
- **Calendar with 1000 meetings:** 
  - Brute force: ~500,000 comparisons
  - Optimized: ~10,000 operations (sort + merge)
  
### Memory Efficiency
- Reduces duplicate data storage
- Consolidates fragmented information
- Cleaner data representation

### Business Benefits
- Accurate availability calculation
- Better resource utilization
- Clearer reporting and analytics
- Reduced billing errors

---

## Interview Tip

When explaining this problem, say:

"This pattern is essential for any system dealing with time ranges or overlapping periods - calendar scheduling, server monitoring, video buffering, database optimization, or promotional campaigns. The key is sorting intervals first, then merging in a single pass. It's commonly used in production for consolidating fragmented data and optimizing resource allocation."

This demonstrates understanding of real-world system design and optimization.

---

## Key Takeaway

Merge Intervals is a blueprint for consolidating overlapping ranges. Whether it's time slots, network bandwidth, database queries, or production schedules - anytime you have overlapping ranges that need to be consolidated, this pattern provides an efficient O(n log n) solution.