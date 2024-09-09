# cachelevelconfiguration

# Multilevel Cache System
### Introduction
This project implements a **Multilevel Caching System** that supports dynamic addition of cache levels, data retrieval, insertion, and eviction policies. The system allows efficient management of cached data across multiple levels, where each level can have its own cache size and eviction policy (LRU or LFU).

# Features
**Dynamic Cache Levels:** Add or remove multiple cache levels (L1, L2, ... Ln) at runtime.

**Eviction Policies:** Supports both Least Recently Used (LRU) and Least Frequently Used (LFU) eviction policies.

**Data Promotion:** When data is retrieved from lower cache levels, it is promoted to the higher-level cache.

**Thread-Safe:** Uses locks to ensure thread-safety during cache operations.

**Concurrency:** Allows for concurrent reads and writes.

## Approach
### 1. Cache Levels:
Each cache level is represented by the **CacheLevel** class.

Each level has a specified size and uses either LRU or LFU eviction policies to handle cache overflow.

### 2. Eviction Policies:
**LRU:** This policy removes the least recently used item when the cache is full. Internally, an **OrderedDict** is used to track access order.

**LFU:** This policy removes the least frequently accessed item. A frequency counter is maintained using **defaultdict(int)** to track how often items are accessed.

### 3. Multilevel Cache System:
The **MultilevelCacheSystem** class manages a list of cache levels.

You can dynamically add or remove cache levels.

#### Data Retrieval:
Data is retrieved from the highest-priority cache first (L1).

If data is found in a lower-level cache, it is promoted to higher levels.

#### Data Insertion:
New data is always inserted at the highest-priority cache level (L1).

If the cache is full, eviction happens according to the chosen policy.

### 4. Thread Safety:
Thread-safety is handled using **threading.Lock**, ensuring that no two threads can modify the cache at the same time.

## Key Decisions
**LRU vs LFU:** The eviction policies are chosen based on the needs of the application. LRU is useful when recent data is more likely to be reused, while LFU is suited for scenarios where frequently accessed data should be retained longer.

**OrderedDict for LRU:** We used **OrderedDict** because it keeps the insertion order and allows efficient reordering when data is accessed.

**Frequency Counter for LFU:** For LFU, we used a **defaultdict(int)** to maintain a count of how frequently each key is accessed.

## Example Workflow

**Import necessary modules and define the cache classes as in the provided code**

**Create the multilevel cache system**

cache_system = MultilevelCacheSystem()

**Add cache levels**

**Add L1 with size 2 and LRU eviction policy**

cache_system.addCacheLevel(2, 'LRU')  

**Add L2 with size 3 and LFU eviction policy**

cache_system.addCacheLevel(3, 'LFU')  

**Insert data into the cache**

cache_system.put("X", "10")

cache_system.put("Y", "20")

**Retrieve data from L1**

print(cache_system.get("X"))  # Output: 10 (retrieved from L1)

**Insert more data, triggering eviction in L1**

**L1 evicts the least recently used item**

cache_system.put("Z", "30")

**L1 evicts the least recently used item**

cache_system.put("W", "40")  

**Retrieve data from L2 and promote it to L1**

**Output: 20 (retrieved from L2, promoted to L1)**

print(cache_system.get("Y"))  

**Add more data to trigger LFU eviction in L2**

cache_system.put("A", "50")

cache_system.put("B", "60")

**Display the current state of both caches**

cache_system.displayCache()

**Remove the second cache level**

cache_system.removeCacheLevel(1)

**Display the cache after removing L2**

cache_system.displayCache()

## How to Run
**1. Clone the Repository**
First, clone the repository from GitHub:
git clone https://github.com/sivakotireddy45/cachelevelconfiguration.git

cd multilevel-cache-system

**2. Run the Application:**

You can run the program using any Python interpreter. Simply execute the script containing the cache system:

#### python multilevel_cache_system.py

**3. Testing the System:**

You can modify the example provided in the code or write your own test cases to see how the cache behaves. The **put()** method adds key-value pairs, and the **get()** method retrieves them while promoting them between cache levels.

