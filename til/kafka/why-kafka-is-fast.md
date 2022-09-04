# Why is Kafka fast?

## Context
We will start by acknowledging that the term **fast** is ambiguous. What does it even mean that Kafka is fast?
Are we talking latency? Are we talking throughput? It's fast compared to what? Kafka is optimized for high throughput.
It is designed to move a large number of records in a short amount of time. Think of it as a vast pipe-moving liquid. The bigger the diameter of the pipe, the larger the volume of fluid that can move through it. So when someone says
Kafka is fast. They usually refer to Kafka's ability to carry much data efficiently. What are some of the design
decisions that help Kafka move many data quickly? Many design decisions contributed to Kafka's
performance. In this video, We will focus on 2. We think these two carry the most weight. The first one is Kafka's
reliance on sequential I/O, and the second design choice that gives Kafka its performance advantage is its focus on
efficiency.

## What is **Sequential I/O**?
![Screen Shot 2022-08-21 at 16 37 14](https://user-images.githubusercontent.com/20236616/185784948-e108cf75-a245-4953-8ffa-fb594bd6df04.png)

There's a common misconception that disk access is slow compared to memory access; however, this largely depends on data
access patterns.
Two disk access patterns exist **Random** and **Sequential**. For hard drives, it takes time to physically
move the arm to different locations on the magnetic disks, making random access slow. For sequential access, since
your arm doesn't need to jump around, it's much faster to read and write blocks of data one after others. Kafka takes
advantage of this by using an append-only log as its primary data structure. An append-only log adds new data to the end
of the file. This access pattern is sequential. Now let's bring this idea home with some numbers. On modern hardware
with an array of these hard disks, sequential writes reach hundreds of MB/s while random writes are measured in hundreds
of KB/s. Sequential access is several orders of magnitude faster. Using hard disks has its cost advantage too. Compared
to SSD, hard disks come at one-third of the price but with about three times the capacity.

Giving Kafka a large pool of cheap disk space without any performance penalty means that Kafka can cost-effectively
retain messages for a long time, a feature uncommon to messaging systems before Kafka.

## What is the **Zero-copy** principle in Kafka?
Kafka moves a lot of data from the network to dist and from disk to network. It's critical to eliminate excess
copy when moving pages and data between the disk and the network. This is where the zero-copy principle comes into
the picture. Modern UNIX OSs are highly optimized to transfer data from disk to network without copying data excessively.
Let's dive deeper to see how this is done.
![Screen Shot 2022-08-21 at 16 23 37](https://user-images.githubusercontent.com/20236616/185784923-74ed50e0-7134-4e00-852b-537477f2ccde.png)

### How does Kafka send a page of data on disk to the consumer when zero-copy is not used at all?
The data is loaded from the disk to the OS cache first. Second, the data is copied from the os
cache into the Kafka application. Third, the data is copied to the socket buffer. And fourth, the data is copied from the
socket buffer to the network interface card buffer. And finally, the data is sent over the network to the consumer. This is inefficient. There are four copies and two system calls.
### How does Kafka send a page of data on disk to the consumer when zero-copy is used?
The first step is similar to without zero-copy. The data page is loaded from the disk to the OS cache. With zero-copy, the
Kafka application uses a system called sendfile() to tell the OS to directly copy the data from the OS cache to the
network interface card buffer. In this optimized path, the only copy is from the OS cache into the network card buffer.
This copying with a modern network card is done with DMA(direct memory access). When DMA is used, the CPU is not
involved, making it even more efficient.

## Wrap up
Sequential I/O and the zero-copy principle are the cornerstones of Kafka's high performance. Kafka uses other techniques to
squeeze every ounce of performance out of modern hardware, but these two are the most important in our view

### References
https://www.youtube.com/watch?v=UNUz1-msbOM&t=225s
