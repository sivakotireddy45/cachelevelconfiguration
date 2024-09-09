# cachelevelconfiguration

Here’s a detailed explanation of the approach, key decisions, and steps on how to run the application that you can include in your GitHub README.md.

Multilevel Cache System
Introduction
This project implements a Multilevel Caching System that supports dynamic addition of cache levels, data retrieval, insertion, and eviction policies. The system allows efficient management of cached data across multiple levels, where each level can have its own cache size and eviction policy (LRU or LFU).

Features
Dynamic Cache Levels: Add or remove multiple cache levels (L1, L2, ... Ln) at runtime.
Eviction Policies: Supports both Least Recently Used (LRU) and Least Frequently Used (LFU) eviction policies.
Data Promotion: When data is retrieved from lower cache levels, it is promoted to the higher-level cache.
Thread-Safe: Uses locks to ensure thread-safety during cache operations.
Concurrency: Allows for concurrent reads and writes.
Approach
1. Cache Levels:
Each cache level is represented by the CacheLevel class.
Each level has a specified size and uses either LRU or LFU eviction policies to handle cache overflow.
2. Eviction Policies:
LRU: This policy removes the least recently used item when the cache is full. Internally, an OrderedDict is used to track access order.
LFU: This policy removes the least frequently accessed item. A frequency counter is maintained using defaultdict(int) to track how often items are accessed.
3. Multilevel Cache System:
The MultilevelCacheSystem class manages a list of cache levels.
You can dynamically add or remove cache levels.
Data Retrieval:
Data is retrieved from the highest-priority cache first (L1).
If data is found in a lower-level cache, it is promoted to higher levels.
Data Insertion:
New data is always inserted at the highest-priority cache level (L1).
If the cache is full, eviction happens according to the chosen policy.
4. Thread Safety:
Thread-safety is handled using threading.Lock, ensuring that no two threads can modify the cache at the same time.
Key Decisions
LRU vs LFU: The eviction policies are chosen based on the needs of the application. LRU is useful when recent data is more likely to be reused, while LFU is suited for scenarios where frequently accessed data should be retained longer.
OrderedDict for LRU: We used OrderedDict because it keeps the insertion order and allows efficient reordering when data is accessed.
Frequency Counter for LFU: For LFU, we used a defaultdict(int) to maintain a count of how frequently each key is accessed.
