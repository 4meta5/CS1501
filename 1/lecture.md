# State of the Space

1. [Getting Up To Speed on Blockchain](#intro)
    1. [Distributed Database](#distributed)
    2. [Bitcoin](#bitcoin)
    4. [Incentive Feedback Loops](#incentives)
2. [Logistics](#logistics)
    1. [Student Onboarding](#onboarding)
    1. [Basic Cryptography](#crypto)
    2. [Proof of Work](#pow)

## Getting Up to Speed on Blockchain <a name="intro"></a>

Matt G Condon's IDEO CoLab speech on [Getting Up to Speed On Blockchain](https://www.youtube.com/watch?v=PvO3J4AKLWo)

A **database** is used to store data. Desirable properties depend on the nature of the information being stored. A few desirable properties could include:
* *Privacy*
* Auditability
* Permissioned
* Permissionless
* Cheap Storage/Overhead
* Cache
* Liveness
* Availability
* Consistency
* Partitions
* *Finality*

This list is by no means exhaustive. For a recent take on what *finality* means in the context of blockchains, check out this article from [Mechanism Labs](https://medium.com/mechanism-labs/finality-in-blockchain-consensus-d1f83c120a9a)

**Let's take a look at Twitter.** Twitter can be seen as a database of tweets that twitter controls. Twitter manages permissions on the subset of the database that you can access and interact with. Even so, Twitter is the gatekeeper so they can shut down the network or change your history. In this world, there is no concept of your data. This is Twitter, Facebook, Google, Amazon's data and they don't keep it to themselves.

**Optional Activity:**What properties does Twitter have that could enable us to create an auditable cryptocurrency. *Push general consensus towards the public qualities of tweets*. Even so, this implementation would still rely on using Twitter.

**One Key Takeaway: public auditability is an important blockchain property!**
This is the reason for the use of Bloom Filters along with Merkle Trees in modern blockchains. We have covered both of these terms in the ./tweets.md file (and tweeted them from the Twitter account @smerklenetwork)

### Distributed Database <a name="distributed"></a>
To scale computation, techniques were developed to coordinate the distribution of computational load across multiple hardware devices. 

**Sidenote:**Many of these techniques (i.e. 'sharding') are being rediscovered in the permissionless setting with recent innovations in the Ethereum Casper family of protocols. 

Nowadays, it is common to use virtual machines, container technology, clustering, cloud services, and other services to outsource computation/storage across multiple servers/devices. Even so, these implementations often suffer from a trust bottleneck. Specifically, these services are often centralized and put enormous trust in entities like Google, Amazon, Facebook, etc. We have already seen how trust can be abused and consumer confidentiality can easily be ignored. The logical next step is to design technology that puts users in control of their own data.

**Good Watch:** [Citizen Four](https://en.wikipedia.org/wiki/Citizenfour)

**Interesting Read (recent):** [Vitalik Blog Post on Layer 1 vs 2 outlook and limitations of sharding in practice](https://vitalik.ca/general/2018/08/26/layer_1.html)

**Cantillon Effect** experienced after quantitative easing post-crisis is cited to prove central bank incompetence and motivate decentralized governance. More information at [this link](https://mises.org/library/how-central-banking-increased-inequality).

### Bitcoin <a name="bitcoin"></a>
In the aftermath of the financial crisis, the Bitcoin whitepaper was released onto a mailing list in late 2008. The paper was extremely innovative in how it defined roles and managed incentives to enable a permissionless ledger for transactions. The anonymity of Bitcoin's author added to the hype and continues to entertain conspiracy theorists. The paper's main innovation was in designing a distributed database that wasn't controlled by any single person/entity. At a high level,it defined a system to fairly distribute power over the database. Because proof of work relies on relatively consistent mining overhead costs, it ensures that the system couldn't be gamed by any one entity. In this manner, Bitcoin's primary allure is and continues to be its ability to achieve decentralization in a permissionless setting. Decentralization measures whether any single entity or consortium of members can manipulate control using their share of power. Blockchain networks like Ethereum and Bitcoin often claim to enable *trustless* transaction validation, but this claim relies on each blockchain being decentralized. In both cases, it is up for debate how decentralized the respective blockchain's operations are *in reality*. 

Interesting view on how recent development may mirror past trends in internet protocol development: [Lightning as Unicast Transaction Network](https://medium.com/@melik_87377/lightning-network-enables-unicast-transactions-in-bitcoin-lightning-is-bitcoins-tcp-ip-stack-8ec1d42c14f5)

**How does this work? (High Level Answer)**
The database is owned by the superset of everyone on the network. You have some *state* and you want to make changes to it. Assume Alice wants to send some Bitcoin to Bob. Alice signs a message (transaction) transferring ownership of funds to Bob. Alice sends the message to the network and, if it is valid, all full nodes on the network validate the transaction and add it to the blockchain. This explanation doesn't cover proof of work (which will be covered in detail next class).

### Incentive Feedback Loops <a name="incentives"></a>
* [Incentive Loops](https://medium.com/@Trustless_State/incentive-loops-how-crypto-actually-fixes-stuff-a7aa7aa3ae04)

Different blockchains have different use cases. Bitcoin might be more traditional so it may value backwards compatibility to ensure no client becomes outdated or unusable. Ethereum is looking to be an application platform and, therefore, needs to scale transaction throughput. This explains the transition from proof of work to proof of stake. It sacrifices decentralization and permissionlessness for greater throughput.In general, blockchains delineate roles and incentivize participation in a coordinated protocol. Recent examples are Handshake, Melonport, Ox, BAT. We'll dive deeper later but cover this at a high level now.

[Brock Pierce Speech](https://www.youtube.com/watch?v=0mpF_bIVxak)

## Logistics <a name="logistics"></a>
Class Background and Details: 
* 1 credit pass/fail
* No one should be taking this class for an easy A
* Resources are provided to learn anything ranging from modern consensus mechanisms to zero knowledge cryptography
* Optional project opportunities will be posted to encourage building real-world applications and interacting with blockchain tech

Github information (on powerpoint this is more important)

My Background:
* 4th year Math and CS double major
* Read papers under guidance of Professor Evans for my 3rd year
* Interned at Hyperledger to build blockchain dispute resolution schemes for permissioned networks
* Interned at Blockmatics to make blockchain developer courses
* Previous experience with finance/markets/monetary policy

Supervisor: Professor David Evans [website](https://www.cs.virginia.edu/~evans/) and [blog](http://www.jeffersonswheel.org)

### Student Onboarding <a name="onboarding"></a>

How can you learn more about this and plug yourself into the space:
-->Bitcoin came out in 2008, it's been 10 years
-->following resources (create lists and put them on the course page)
-->point to student onboarding page on the website


Key Opportunities:
* Knowledge Graph
    * Blockchain Protocol Layer Graph
    * Middleware Graph
    * Dapp Graph
    * Actor Graph
    * [example of organization of knowledge](https://medium.com/@josh_nussbaum/blockchain-project-ecosystem-8940ababaf27)
* A6 Staking
* DCentral Bank

Follow on Twitter: @smerklenetwork

Join Chat Groups:
* Telegram Groups
* Althea Riot Chat
* Gitter, Slack, IRC, etc.

Languages:
* Javascript/ES6
* Python
* Rust
* Golang

Before next class, we'll provide some resources to become familiar with basic cryptography. Next class, we'll cover pow as well.

### Basic Cryptography <a name="crypto"></a>
Cryptography:
* hash function
* merkle trees (Google origin and how they're used in Plasma for Sparse Merkle Trees)
* public-key cryptography
* digital signatures

**Reading**
* [Basics of Hash Functions](https://medium.com/@ConsenSys/blockchain-underpinnings-hashing-7f4746cbd66b)
* [Public Key Crytography](https://security.stackexchange.com/questions/25741/how-can-i-explain-the-concept-of-public-and-private-keys-without-technical-jargo)

### Proof of Work <a name="pow"></a>
* Math and Whitepaper Walk Through
* block size debate
* Maintaining backward compatibility vs. Upgradability
* Monetary Policy
* Politics

**Reading**
* [Cypherpunk's Manifesto](https://www.activism.net/cypherpunk/manifesto.html)
* [Bitcoin's Academic Pedigree](https://queue.acm.org/detail.cfm?id=3136559)
* [Bitcoin's Whitepaper](https://bitcoin.org/bitcoin.pdf)

**Extra**
* [Video of Joseph Poon discussing Plasma](https://www.youtube.com/watch?reload=9&v=oOQmnhQrq_U)
This video will provide a smooth introduction to layer 2 scaling solutions.