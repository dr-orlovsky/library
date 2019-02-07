A very brief engineering introduction into consensus algorithms and distributed ledger data structures
===

**_Dr Maxim Orlovsky_**

Consensus algorithms define a sequence of actions by which independent *agents* (also named *actors*, *parties* or, in early research on the topic before the wide Internet adoption *processors*) come to an equivalent *view* (at least by the majority of the agents) on the parts of the distributed system. This can be determined, for instance, by some *client* (not participating in the consensus) obtaining the same result for his query regarding the consensus-based state of the system from any (or at least most) consensus participants.

Most often, consensus algorithms are used for solving the following problems:
- Leader election (selecting the agent among all consensus participants with the right to update the global state of the system)
- Atomic swaps (exact order of events that can't be ordered deterministically basing on their internal properties)
- State replication (maintaining global state shared across all, or most, of the agents)

These are the three major use cases for the consensus algorithms are highly interrelated. For instance, state replication can be solved by the proper ordering of state changes (i.e. atomic broadcasts) - and a proper leader election process may allow ordered atomic broadcasting by itself (however, there are consensuses achieving the same result without leader election process).

Historically, consensus algorithms originated from the research on multiprocessor computing; they were solving a problem of the global state for the cases when processor may fail (i.e. become unresponsive). In these cases communications were synchronous, i.e. bound by some upper and known time limit ∆.

Later, with the development of telecommunication and computer networks two other problems had arisen: the presence of unknown communications delays and adversaries. The former led to new research on partly synchronous and asynchronous consensus algorithms (when ∆ is either unknown or absent, when there is no upper limit for a possible network delays) and creation of algorithm that can tolerate arbitrary agent behaviour (*Byzantine behavior*) - so-called Byzantine fault-tolerant algorithms (or, simply, BFT consensuses).

With the wide adoption of Internet, the problem of adversaries become even larger. If within multiprocessor environments or telecommunication infrastructure each agent can be identified, the same can't be done in many cases with the Internet. Thus, a new line of public (or *permissionless*) consensuses has appeared, where the consensus algorithm had to become a protocol with embedded rules and procedures for identification and exclusion of Byzantine agents - like by some collateral mechanisms reducing the economic ability of such agents to further participate the protocol. Such systems acquired public attention under names of PoW and PoS consensuses. We will name such protocols *BFT consensuses with economical incentivisation* (BFT-EI). In many cases, asynchronicity and permissionless require to sacrifice other consensus qualities, like determinism or ability to be applied for a leader election scenarios.

Basing on the above, consensus algorithms can be classified with different criteria.
1. The subject of a consensus: atomic broadcasts, leader election, state replication (or, as another option, leader-based and leaderless algorithms)
2. Forms of fault tolerance: no fault tolerance, tolerance to non-arbitrary faults and Byzantine tolerance (BFT)
3. Assumptions for the consensus can be reached: synchronous, party synchronous, asynchronous
4. Consensuses with open and closed participation: permissioned vs permissionless, or public/opened vs private/closed.
5. Internal economical incentivisation: absent or present (BFT-EI)
6. Deterministic qualities of consensus, i.e. ability to achieve *finality*: consensus algorithms with finality property and non-deterministic consensuses.

These classes can be combined in some natural groups, like the opened participation will result in non-deterministic consensus properties (since we can't enumerate all of the consensus actors at a given moment of time), i.e. absence of finality, and it will be leaderless. A typical example of such consensus is Bitcoin PoW.

We define the following typical consensus property combinations:
1. PoW: permissionless synchronous non-deterministic leaderless BFT-EI
2. PoS: permissionless partly-synchronous deterministic leadership-based BFT-EI
3. BFT (in a narrow sense): permissioned, deterministic BFT. Can have different synchronicity assumptions and can be a leader- or leaderless
4. DAGs: permissionless asynchronous non-deterministic leaderless non-BFT tolerant
5. Other mixed (usually historical or heavy-experimental) types

Many real-world applications of consensus algorithms, like those found in modern experimental blockchains, tend to utilize a set of different consensus mechanics, like ones used for leader election (usually BFT-agreement-based, but sometimes even PoW), others for atomic broadcasts (PoS) and another for achieving determinism is state replication - usually in form of a BFT-type "gadget".

By themselves, a consensus algorithm does not require some particular data structure; they are all about agent behaviour and network communications. However, after invention of Bitcoin (which was first public-consensus based application) it appeared that systems requiring global state it can be useful to utilize some best practices on data structures, like log-like ledgers with cryptographic enhancements - so-called blockchains - or their more generic kind of directed acycled graphs (DAGs), usually utilized by consensuses from the group #4 of our classification above.
