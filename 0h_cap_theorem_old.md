In this lesson, we will discuss the CAP theorem, a key concept developed by the computer scientist Eric Brewer that governs why and how NoSQL databases are scalable and good for big data. We'll also touch on Eric Brewer's acronym BASE, which describes how NoSQL databases achieve the requirements of the CAP theorem when distributed across multiple locations.

These are in contrast to the ACID theorem, which generally explains how SQL databases behave. We discussed the [ACID theorem](https://old.learnhowtoprogram.com/ruby-and-rails/ruby-database-basics/acid-databases) in Ruby/Rails and C#/.NET, but if you need a refresher or didn't go through those courses, we recommend taking a look.

**Note:** You are not required to memorize this information or perfectly understand it. The goal is to give you a better idea of some of the tradeoffs in a distributed NoSQL system. 

## The CAP Theorem
---

While the ACID theorem includes four principles that a SQL database **must** follow, the CAP theorem is different. The CAP theorem argues that a database system can use **at most** two of its three principles. Let's look at those three principles and then explore why there needs to be a compromise.

* **Consistency**: All users have access to the same data in the system at the same time.

* **Availability**: The data is always available for users.

* **Partition Tolerance**: The system works across a distributed network (such as multiple servers).

One of the reasons that NoSQL is often better for big data is the fact that it is designed to be distributed across multiple locations. (On the other hand, SQL has traditionally been designed to work on just one node.) For instance, a big company might have database servers all over the world. If one of these databases goes down, there are always others to pick up the slack. This distribution is a huge advantage of NoSQL databases. This is the "P" in "CAP": **partition tolerance**.

Because SQL databases have one node and NoSQL databases can have many, it is said that SQL databases **scale vertically** (on one server) while NoSQL databases **scale horizontally** across many servers.

However, if we have partition tolerance, we can't have both consistency and availability. We'll demonstrate why with an example.

Let's say you work for a company that has database servers in Beijing, New York, and London. A user in Shanghai makes an update to their account. That information is now going to be sent to a database. But which one? Even if it's sent to every single server, it will probably reach the server in Beijing first. If the database in Beijing updates immediately, it will no longer be **consistent** with the databases in New York and London, which don't have that update yet. On the other hand, if the server in Beijing waits for the other servers so they can all be updated simultaneously, the data will be consistent but we will be compromising **availability** — we are delaying the update to the Beijing server. It may not seem like a big deal with one update near Beijing, but a large company like Facebook or Google could be handling thousands of updates per second all over the world.

Since having a strong distributed system (partition tolerance) is a huge advantage of many NoSQL options, either consistency or availability are compromised. This doesn't even account for occasional network failures, which are inevitable. However, large companies will introduce redundancy — databases and servers that have overlapping functionality — so that if a server or database goes down then others can pick up the slack. Those network failures will result in further compromises to consistency or availability.

Most distributed systems want data to be available all of the time, which leads to a tradeoff in consistency. The goal, then, is what is called **eventual consistency**. This means that when the update to the database server happens in Beijing, the databases won't be consistent across all servers yet — but they will be consistent eventually.

This is in contrast to **strong consistency**, which means that everything has to be consistent everywhere at the same time (compromising either availability or partition tolerance). Strong consistency is a tenet of SQL databases. Technically, it's possible to create SQL databases across a distributed system. However, it's not very common yet.

## BASE
---

There's another acronym called **BASE**, also developed by Eric Brewer, which describes how NoSQL databases achieve consistency. BASE states that a system should be "basically available, have a soft state, and eventually consistent."

When a system is **basically available**, data is available most of the time. For instance, the data in the Beijing server won't be immediately available to the server in London. Also, database servers can and will fail, leading to downtime in availability.

When a database system has a **soft state**, it may update even when it hasn't been written to. When the server in London finally gets data from Beijing, it will update — but not because data was written directly to it. In order for data to become available and consistent, it may be written to the database at different times.

Finally, we have **eventual consistency**, which we just discussed. Eventually, the data in London will match the data in Beijing.

The CAP theorem is essential to understanding how distributed systems work. Meanwhile, the BASE acronym shows how the requirements of CAP can be loosened enough that every condition is more or less met — just not stringently.

By the way, Eric Brewer is vice-president of infrastructure at Google. His theorems aren't just concepts — they are widely applied at the world's largest software companies. We'll even see CAP and BASE come into play when we discuss how Firebase works.
