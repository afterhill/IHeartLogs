If you made it this far you know most of what I know about logs; here are a few interesting references you may want to check out. 

Everyone seems to use different terms for the same things, so it is a bit of a puzzle to connect the database literature to the distributed systems stuff to the various enterprise software camps to the open source world. Nonetheless, here are a few pointers.

###Academic Papers, Systems, Talks, and Blogs

- These are good overviews of state machine and primary-backup replication.
- PacificA is a generic framework for implementing log-based distributed storage systems at Microsoft.
- Spanner—Not everyone loves logical time for their logs. Google’s new database tries to use physical time and models the uncertainty of clock drift directly by treating the timestamp as a range.
- Datanomic: “Deconstructing the database” is a great presentation by Rich Hickey, the creator of Clojure, on his startup’s database product.
- “A Survey of Rollback-Recovery Protocols in Message-Passing Systems“—I found this to be a very helpful introduction to fault tolerance and the practical application of logs to recovery outside databases.
- “The Reactive Manifesto”—I’m actually not quite sure what is meant by reactive programming, but I think it means the same thing as “event driven.” This link doesn’t have much information, but this class by Martin Odersky (of Scala fame) looks fascinating.
- Paxos!
  - Leslie Lamport has an interesting history of how the algorithm was created in the 1980s but was not published until 1998 because the reviewers didn’t like the Greek parable in the paper and he didn’t want to change it.
  - Once the original paper was published, it wasn’t well understood. Lamport tried again and this time even included a few of the “uninteresting details,” such as how to put his algorithm to use using actual computers. It is still not widely understood.
  - Fred Schneider and Butler Lampson each give a more detailed overview of applying Paxos in real systems.
  - A few Google engineers summarize their experience with implementing Paxos in Chubby.
  - I actually found all of the Paxos papers pretty painful to understand but dutifully struggled through them. But you don’t need to because this video by John Ousterhout(of log-structured filesystem fame) will make it all very simple. Somehow these consensus algorithms are much better presented by drawing them as the communication rounds unfold, rather than in a static presentation in a paper. Ironically, this video, which I consider the easiest overview of Paxos to understand, was created in an attempt to show that Paxos was hard to understand.
  - “Using Paxos to Build a Scalable Consistent Data Store”—This is a cool paper on using a log to build a data store. Jun, one of the coauthors, is also one of the earliest engineers on Kafka.
- Paxos has competitors! Actually each of these map a lot more closely to the implementation of a log and are probably more suitable for practical implementation:
  - “Viewstamped Replication” by Barbara Liskov is an early algorithm to directly model log replication.
  - Zab is the algorithm used internally by Zookeeper.
  - RAFT is an attempt at a more understandable consensus algorithm. The video presentation, also by John Ousterhout, is great, too.
- You can see the role of the log in action in different real distributed databases:
  - PNUTS is a system that attempts to apply the log-centric design of traditional distributed databases on a large scale.
  - HBase and Bigtable both give another example of logs in modern databases.
  - LinkedIn’s own distributed database, Espresso, like PNUTS, uses a log for replication, but takes a slightly   different approach by using the underlying table itself as the source of the log.  
- If you find yourself comparison shopping for a replication algorithm, this papermight help you out.
- Replication: Theory and Practice is a great book that collects a number of summary papers on replication in distributed systems. Many of the chapters are online (for example, 1, 4, 5, 6, 7, and 8).
- Stream processing. This is a bit too broad to summarize, but here are a few things I liked:  
  - “Models and Issues in Data Stream Systems” is probably the best overview of the early research in this area.
  - “High-Availability Algorithms for Distributed Stream Processing”
  - A couple of random systems papers:
    o “TelegraphCQ”
    o “Aurora”
    o “NiagaraCQ”
    o “Discretized Streams”: This paper discusses Spark’s streaming system.
    o “MillWheel: Fault-Tolerant Stream Processing at Internet Scale” describes one of Google’s stream processing systems.
    o “Naiad: A Timely Dataflow System”
    
###Enterprise Software

The enterprise software world has similar problems but with different names.

Event sourcing

- As far as I can tell, event sourcing is basically a case of convergent evolution with state machine replication. It’s interesting that the same idea would be invented again in such a different context. Event sourcing seems to focus on smaller, in-memory use cases that don’t require partitioning. This approach to application development seems to combine the stream processing that occurs on the log of events with the application. Since this becomes pretty non-trivial when the processing is large enough to require data partitioning for scale, I focus on stream processing as a separate infrastructure primitive.

Change data capture

- There is a small industry around getting data out of databases, and this is the most log-friendly style of database data extraction.

Enterprise application integration

- This seems to be about solving the data integration problem when what you have is a collection of off-the-shelf enterprise software like CRM or supply-chain management software.

Complex event processing (CEP)
- I’m fairly certain that nobody knows what this means or how it actually differs from stream processing. The difference seems to be that the focus is on unordered streams and on event filtering and detection rather than aggregation, but this, in my opinion is a distinction without a difference. Any system that is good at one should be good at the other.

Enterprise service bus
- The enterprise service bus concept is very similar to some of the ideas I have described around data integration. This idea seems to have been moderately successful in enterprise software communities and is mostly unknown among web folks or the distributed data infrastructure crowd.

###Open Source

There are almost too many open source systems to mention, but here are a few of them:
o Kafka is the “log as a service” project that is the inspiration for much of this book.
o BookKeeper and Hedwig comprise another open source “log as a service.” They seem to be more targeted at data system internals than at event data.
o Akka is an actor framework for Scala. It has a module for persistence that provides persistence and journaling. (There is even a Kafka plugin for persistence.)
o Samza is a stream processing framework we are working on at LinkedIn. It uses many of the ideas in this book, and integrates with Kafka as the underlying log.
o Storm is a popular stream processing framework that integrates well with Kafka.
o Spark Streaming is a stream processing framework that is part of Spark.
o Summingbird is a layer on top of Storm or Hadoop that provides a convenient computing abstraction.
