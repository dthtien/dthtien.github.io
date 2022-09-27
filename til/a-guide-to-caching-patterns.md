# Guide to caching patterns

Deciding to use caching is just the first step in a long journey. This is the trade-off that caching offers: stale data
vs speed.

## Cache-Aside - CA

### Reads
![](https://hazelcast.com/wp-content/uploads/2021/12/cache-aside-read-1.svg)
source: hazelcast.com

### Write
![](https://hazelcast.com/wp-content/uploads/2021/12/cache-aside-write-1.svg)

### Pros and cons
- Pros
  - anybody can read the code and understand it's execution flow
  - The requirements toward the cache provider are at their lowest: it just needs to be able to get and set
  - Straightforward migrations from a cache provider to another one.
- Cons
  - Code needs to handle the inconsistency gap between the cache and the datastore
    - When u successfully updated the cache but the datastore fails to update -> retries needed
    - The cache contains a value that the datastore doesn't incase unsuccessful retries

## Read Through(RT)
- RT moves the responsibility of getting the value from the datastore to the cache provider
![](https://hazelcast.com/wp-content/uploads/2021/12/read-through.svg)
- RT implements the Separation of Concerns priciple.
- The code interacts with the cache only
- The cache will manage the synchronization between itself and the datastore
- It requires a more advanced cache provider than the CA
- Maploader

## Write Though(WT)
- WT moves the responsibility of writing to the cache provider

![](https://hazelcast.com/wp-content/uploads/2021/12/write-through.svg)

- The code is now free of failure handling and retry logic -> cache will handle it then

## Write Bebind - WB

![](https://hazelcast.com/wp-content/uploads/2021/12/write-behind.svg)

- The last arrow's arrowhead means that the cache sends and asynchronous message to the datastore
- With WB, the cache sets the value to the datastore and doesn't wait for confirmation while others need to wait
- This approach speeds the whole process since the datastore is the slowest component -> network and write to disk
- Runs the risk of introducing inconsistentcies in the cache
- You don't know if the set was even successful

## Refresh Ahead - RA
- The old saying goes that there are two hard things in CS: naming things and cache invalidation.
- Cache invalidation is about planning how long an item should be stored in cache before it expires
- Reading from datastore is an expensive operation: get through the network and then request data from store
- Prefetch data, making it available before you even request, thus saving you from incurring the performance hit on the
  critical path
- Implementations of RA are cache provider-dependent
- Using change data capture (CDC) to connect cache provider and update cached entities as soon as the datastore is
  updated

![](https://hazelcast.com/wp-content/uploads/2021/12/refresh-ahead.svg)

## Summary

| pattern | consider | cons|
|-------|------|------|
| Cache Aside | when you are limited by the capabilities of your cache provider |
| Read Through | Solid default | |
| Write Through | Solid default | |
| Write behind  | When performance considerations outweigh short-term consistency | Asynchronous systems are handler to reason with
| Refresh - ahead | when fetching data from the datastore impairs throughput| Additional component to develop deploy maintain|

