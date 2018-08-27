## State of the Space

Matt G Condon's IDEO CoLab speech on [Getting Up to Speed On Blockchain](https://www.youtube.com/watch?v=PvO3J4AKLWo)

//link to different sections in this file and make it easily queriable and navigatable

Notes:

--My background (powerpoint it)

For the class, my main goals are to 
(1) -->foster intellectual curiosity
(2) -->provide tools/guidance to enable your success in this space
(3) -->foster diverse discussions to make the class *symbiotic*
We are peers and will learn together. Think of this whole class as a massive experiment and my goal is to make so that we can share the knowledge that we have with each other. (2 heads are better than 1 comic)...emphasize fact that everyone has an area to contribute

This is going to be the least technical lecture. I'm teaching to a wide range of audiences. Some of you may be too advanced for this class
-->for you, I want to do these things
----encourage side projects and help you with them
----enable your personal development; building the skills that will be useful in the future and positioning yourself in this space

for the intermediate crowd;
-->plug you into great sources of knowledge in the space
-->make you aware of opportunities to build and contribute

for the new people;
-->introduce to you the space
-->foster curiosity and enable exploration

Historical perspective:
--concept of a database
--want a place to store data and make sure that it stays there
--keeping history can be useful for many reasons
--twitter is a database of tweets that twitter controls
----they allow you to change a subset of the database (your tweets) 
----they are the gatekeepers so they can shut down the network or change your history whenever they want (you are not in control of your data/information, they are)

--then, we started thinking of the concept of distributed database to scale computation, storage, etc.
--but these were still controlled/managed by the same cloud provider, etc. (Amazon, Google pics)

--paradigm shift with the Bitcoin Whitepaper
--end of 2008, bank bailout and financial crisis
----Satoshi Nakamoto released this paper onto a mailing list
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

How can you learn more about this and plug yourself into the space:
-->Bitcoin came out in 2008, it's been 10 years
-->following resources (create lists and put them on the course page)
-->point to student onboarding page on the website

"Blockchain vs Cryptocurrency"
--to stop someone from attacking the network, it has to cost money to submit transactions. So miner's dont accept transactions that don't pay enough money. This is the transaction fee for each transaction (it represents the value provided by the network and is priced in the native currency)
--transaction fees fluctuate in real value so it is better to use some native currency. Show why it wouldn't make sense to do this with the USD. The value provided by the network will fluctuate and this influences miner rewards (which partly consist of transfer fees and partly consist of issuance)
--you need to stop people from abusing the network; example is a sybil attack: because there is no cost to instantiating each identity, people can spam the network with identities. Mining and proof of work anchors a cost to voting essentially.

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

So how does proof of work work?
*explain process
*explain block size debate
*use Smerkle tweet inbox quote on Bitcoin's principles on maintaining backward compatibility (define this term well)
--ASIDE: recent monetary policy debate on ethereum and how there is a lot of 'politics'. Explain the politics and how everyone has conflicting incentives.

**THIS IS A LOT ALREADY...STOP HERE AND TIME IT AFTER FINISHING THE POWERPOINT; CONSIDER USING ANY REMAINING TIME TO INTRODUCE KNOWLEDGE GRAPH AND OTHER IDEAS...ALSO WHAT NEEDS TO BE DONE BEFORE NEXT CLASS**

