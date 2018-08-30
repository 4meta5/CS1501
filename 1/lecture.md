# State of the Space

1. [Getting Up To Speed on Blockchain](#intro)
    1. [Distributed Database](#distributed)
    2. [Bitcoin](#bitcoin)
    3. [Blockchain for Money](#money)
    4. [Incentive Feedback Loops](#incentives)
2. [Logistics](#logistics)
    1. [Student Onboarding](#onboarding)
    1. [Basic Cryptography](#crypto)
    2. [Proof of Work](#pow)

## Getting Up to Speed on Blockchain <a name="intro"></a>

Matt G Condon's IDEO CoLab speech on [Getting Up to Speed On Blockchain](https://www.youtube.com/watch?v=PvO3J4AKLWo)

Consider the concept of a database. A database is used to store data. Depending on the speific use case, certain properties may be desired. 

If privacy is important, then the data should be *encrypted*. By the end of next class, you'll understand how encryption works at a high level. In many cases, it is important that a history of all changes to the database are maintained and tamper-proof. Indeed, having an auditable history can be useful for many reasons, but that's not how contemporary applications work.

**Let's take a look at Twitter.** Twitter can be seen as a database of tweets that twitter controls. Twitter manages permissions on the subset of the database that you can access and interact with. Even so, Twitter is the gatekeeper so they can shut down the network or change your history. In this world, there is no concept of your data. This is Twitter, Facebook, Google, Amazon's data and they don't keep it to themselves.

**Optional Activity:**What properties does Twitter have that could enable us to create an auditable cryptocurrency. *Push general consensus towards the public qualities of tweets*. Even so, this implementation would still rely on using Twitter.

**One Key Takeaway: public auditability is an important blockchain property!**
This is the reason for the use of Bloom Filters along with Merkle Trees in modern blockchains. We have covered both of these terms in the ./tweets.md file.

### Distributed Database <a name="distributed"></a>
To scale computation, techniques were developed to coordinate the distribution of computational load across multiple hardware devices. 

**Sidenote:**Many of these techniques (i.e. 'sharding') are being rediscovered in the permissionless setting with recent innovations in the Ethereum Casper family of protocols. 

Nowadays, it is common to use virtual machines, container technology, clustering, cloud services, and other services to outsource computation/storage across multiple servers/devices. Even so, these implementations often suffer from a trust bottleneck. Specifically, these services are often centralized and put enormous trust in entities like Google, Amazon, Facebook, etc. We have already seen how trust can be abused and consumer confidentiality can easily be ignored. The logical next step is to design technology that puts users in control of their own data.

**Good Watch:** [Citizen Four](https://en.wikipedia.org/wiki/Citizenfour)

**Interesting Read (recent):** [Vitalik Blog Post on Layer 1 vs 2 outlook and limitations of sharding in practice](https://vitalik.ca/general/2018/08/26/layer_1.html)

### Bitcoin <a name="bitcoin"></a>
In the aftermath of the financial crisis, the Bitcoin whitepaper was released onto a mailing list in late 2008. The paper was extremely innovative in how it defined roles and managed incentives to enable a permissionless ledger for transactions. The anonymity of Bitcoin's author added to the hype and continues to entertain conspiracy theorists. The paper's main innovation was in designing a distributed database that wasn't controlled by any single person/entity. At a high level,it defined a system to fairly distribute power over the database. Because proof of work relies on relatively consistent mining overhead costs, it ensures that the system couldn't be gamed by any one entity. In this manner, Bitcoin's primary allure is and continues to be its ability to achieve decentralization in a permissionless setting. Decentralization measures whether any single entity or consortium of members can manipulate control using their share of power. Blockchain networks like Ethereum and Bitcoin often claim to enable *trustless* transaction validation, but this claim relies on each blockchain being decentralized. In both cases, it is up for debate how decentralized the respective blockchain's operations are *in reality*. 

****

---cypherpunk manifesto
----described a model that could enable the creation of a distributed database that wasn't controlled by 
any single person/entity...it creates a method to create some percent share over the database (this is called trustless, you don't need to trust the other people that are maintaining the database...the reason is because everyone can basically act in their self-interest and everything will be secure)
----the database is owned by the superset of everyone on the network; 
blockchain:
--you have some state and you want to make changes to it
--so the changes must reference the previous state and this creates an immutable history of changes
--define immutable...put some data out there and have it stay there and not change
--in the database, we tracked how much money someone has and that's bitcoin
--So at a high level, this works in a way where I decide I want to send some amount to someone else so I sign a message/transaction and send that to the network and everyone network validates the transaction by referencing the previous state and verifies that I have those funds and the transaction is legitimate before adding it to the block, which enables and symbolizes transfer of value
--people called miners are in charge of doing the verification
--just a database with very specific rules on how it can change

### Blockchain for Money <a name="money"></a>
* [Hayek Quote and Quiao Article](https://medium.com/@QwQiao/hayek-and-stablecoins-3c7f3291d728)
* [Lightning as Unicast Transaction Network](https://medium.com/@melik_87377/lightning-network-enables-unicast-transactions-in-bitcoin-lightning-is-bitcoins-tcp-ip-stack-8ec1d42c14f5)
* [Cantillon Effect Szabo Tweet](https://twitter.com/NickSzabo4/status/1031232173561368576)
* [Mises Wire](https://mises.org/library/how-central-banking-increased-inequality)
* [Cantillon Wiki](https://en.wikipedia.org/wiki/Richard_Cantillon)

### Incentive Feedback Loops <a name="incentives"></a>
* [Incentive Loops](https://medium.com/@Trustless_State/incentive-loops-how-crypto-actually-fixes-stuff-a7aa7aa3ae04)


Different blockchains have different use cases. Bitcoin might be more traditional so it may value backwards compatibility longer to ensure no client becomes outdated or unusable. Ethereum is looking to be a platform and needs to scale transaction throughput. This explains the transition from proof of work to proof of stake. Shuffle quickly through a slide on why Ethereum isn't really proof of stake. It sacrifices decentralization and permissionlessness for greater throughput. Anyway, one thing all blockchains have in common is that they are designed to manage incentives. They delineate roles and incentivize participation in a coordinated protocol. Recent examples are Handshake, Melonport, Ox, BAT. We'll dive deeper later but cover this at a high level now.

"Incentive Loops"
--borrow from **Brock Pierce's speech** and link

"Blockchain vs Cryptocurrency"
--explanation as to why "I believe in blockchain but not cryptocurrency is ridiculous"
--to stop someone from attacking the network, it has to cost money to submit transactions. So miner's dont accept transactions that don't pay enough money. This is the transaction fee for each transaction (it represents the value provided by the network and is priced in the native currency)
--transaction fees fluctuate in real value so it is better to use some native currency. Show why it wouldn't make sense to do this with the USD. The value provided by the network will fluctuate and this influences miner rewards (which partly consist of transfer fees and partly consist of issuance)
--you need to stop people from abusing the network; example is a sybil attack: because there is no cost to instantiating each identity, people can spam the network with identities. Mining and proof of work anchors a cost to voting essentially.

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

Before next class, we'll provide some resources to become familiar with basic cryptography.

Next class, we'll cover pow as well.

### Basic Cryptography <a name="crypto"></a>
Recently developed useful cryptography:
*hash function
*merkle trees (Google origin and how they're used in Plasma for Sparse Merkle Trees)
*public-key cryptography
*digital signatures

--consider watching more videos and putting together some reading list
--high level overview of public key cryptography
you sample some reliable source of randomness like atmospheric radiation and use this to generate a private key. 
--you can generate a public key using a trapdoor function (one-way function, like a hash function)...explain how this works
--it's impossible to guess the private key even with the public key because you can't reverse the hash function
--also, no one will get the same public key because hash functions are collision-resistant
--It is basically impossible for anyone to generate your private key and whenever you sign transactions, you sign your private key with your public key...this allows you to establish some sense of identity (term: self-sovereign identity...mention impacts)

**Reading**
* [Basics of Hash Functions](https://medium.com/@ConsenSys/blockchain-underpinnings-hashing-7f4746cbd66b)
* [Public Key Crytography](https://security.stackexchange.com/questions/25741/how-can-i-explain-the-concept-of-public-and-private-keys-without-technical-jargo)


### Proof of Work <a name="pow"></a>
So how does proof of work work?
*explain process
*explain block size debate
*use Smerkle tweet inbox quote on Bitcoin's principles on maintaining backward compatibility (define this term well)
--ASIDE: recent monetary policy debate on ethereum and how there is a lot of 'politics'. Explain the politics and how everyone has conflicting incentives.

**Reading**
* [Cypherpunk's Manifesto](https://www.activism.net/cypherpunk/manifesto.html)
* [Bitcoin's Academic Pedigree](https://queue.acm.org/detail.cfm?id=3136559)
* [Bitcoin's Whitepaper](https://bitcoin.org/bitcoin.pdf)