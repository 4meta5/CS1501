---
id: 1;
term: Harberger Tax;
status: Active;
author: Amar Singh <ars9he@virginia.edu>; 
created: 2018-8-28;
cited: SimonDLR, Glen Weyl, Eric Posner 

---

* [SimonDLR article](https://medium.com/@simondlr/what-is-harberger-tax-where-does-the-blockchain-fit-in-1329046922c6)
* [Property is Only Another Name for Monopoly](https://academic.oup.com/jla/article/9/1/51/3572441)
* Harberger tax introduces two concepts to property ownership: (1) Citizens value their own property and pay taxes on it accordingly (self-assessed tax) (2) At any point in time, anyone can buy the property for the set price (force a sale)
* Pros: The property is always owned by whoever is willing to pay the most. This allows the market to allocate property to productive owners, thereby achieving allocative efficiency.
* Cons: By allowing forced sale at any time, the policy may dissuade the current owner from investing in the asset, thereby sacrificing investment efficiency. Forced sales also are not optimal for certain assets (ie real estate and/or housing).
* "Due to the quadratic loss of monopolies in private ownership, a reduction in investment efficiency at the benefit of allocative efficiency, results in great benefits to welfare (and thus society overall)"
*"Harberger taxation achieves 70-90% of the maximum possible allocative welfare gains and the investment losses erode only 10 to 20% of these gains"
* This policy structure could be implemented alongside continuous auctions to enable novel distribution schemes built on the blockchain. From @simondlr "...reducing the transaction costs to coordinate allows us to form new organizations that create new wealth"
* [Vitalik Blog Post On Radical Markets](https://vitalik.ca/general/2018/04/20/radical_markets.html)

---
id: 2;
term: Bloom Filter;
status: Active;
author: Amar Singh <ars9he@virginia.edu>; 
created: 2018-8-28;
cited: chris_seberino 

---

* [ETC Article by Chris Seberino](https://ethereumclassic.github.io/blog/2017-02-10-bloom-filters/)
* Bloom Filters exist on Ethereum to facilitate auditability without significantly increasing overhead.
*Events must be easily searchable to enable applications to display events with relatively low overhead. Because blockchain storage space is expensive, bloom filters are used to quickly determine set membership with marginal storage requirements.
* Non-blockchain use cases: Browsers use them to detect malicious websites; databases use them to avoid searching for nonexistent data.
* Advantage: require less memory than indexes (strong space advantage)
Disadvantage: Sometimes give false positives (may claim an object is a member of a set when it is not), but never gives false negatives. Extra checks can be implemented to eliminate false positives.
* Main use in blockchains: Bloom filters enable cryptocurrency wallets and lightweight clients to quickly obtain account information without running a full node.
* Cryptography: It is a probabilistic data structure: it tells us that the element either definitely is not in the set or may be in the set. The basic construction uses a bit vector to store maps or hashes of set elements that have been added.
* [Ethereum Stack Exhange Explanation](https://ethereum.stackexchange.com/questions/16117/proving-the-existence-of-logs-to-the-blockchain)
* [Wiki](https://en.wikipedia.org/wiki/Bloom_filter)

---
id: 3;
term: Merkle Tree;
status: Active;
author: Amar Singh <ars9he@virginia.edu>; 
created: 2018-8-29;
cited: Consensys, Simondlr 

---

* Merkle tree is a data structure that compresses data while still allowing close supervision of state changes. Every leaf node is labelled with the hash of a data block and every non-leaf node is labelled with the hash of its child nodes.
* [Consensys Graphic](https://t.co/OB8960Hzyt)
* Merkle Trees are very useful for securing and verifying data. For this reason, they are often used in blockchains in conjunction with Bloom Filters (for more info, check this stack exchange response [link](https://t.co/qOMNvakIb1)
* Sparse Merkle Tree (SMT) is a variant of a Merkle Tree designed to minimize storage requirements for specific databases. Google first proposed this data structure when tracking [certificate revocation](https://t.co/hdCv6C4Dpu)
* SimonDlr explained how the SMT is used in Plasma Cash to "prove the existence of a leaf with some data, thereby also proving non-inclusion of the rest of the state." This may be useful for NFT issuance/tracking off-chain [article](https://t.co/PzTH9JCAlO)

---
id: 4;
term: Smart Contract;
status: Active;
author: Amar Singh <ars9he@virginia.edu>; 
created: 2018-8-30;
cited: Nick Szabo, Vitalik Buterin, Patrick McCorry

---

![Smart Contract Laws](/assets/smartcontractlaw.ppg)
* Smart contracts were first discussed by @NickSzabo4  in 1996 ~"The basic idea of smart contracts is that many kinds of contractual clauses can be embedded in hardware/software to make the breach of contract prohibitively expensive for the breacher." [link](http://www.fon.hum.uva.nl/rob/Courses/InformationInSpeech/CDROM/Literature/LOTwinterschool2006/szabo.best.vwh.net/smart_contracts_2.html)
* The canonical example used by Szabo is vending machines ~ "The machine accepts coins, and via a simple mechanism, which makes a beginner's level problem in design with finite automata, dispense change and product fairly."
* Szabo expanded on smart contract security in a publication the next year: [link](http://www.fon.hum.uva.nl/rob/Courses/InformationInSpeech/CDROM/Literature/LOTwinterschool2006/szabo.best.vwh.net/smart_contracts_idea.html) In 2002, Szabo returned to this topic and wrote about a formal language for analyzing contracts. Many of these ideas are recycled in contemporary work [link](http://www.fon.hum.uva.nl/rob/Courses/InformationInSpeech/CDROM/Literature/LOTwinterschool2006/szabo.best.vwh.net/contractlanguage.html)
* One common mechanism is the use of collateral in smart contracts to attach a cost to claims. This consists of requiring users to put some $$ in a vault to 'vote' on certain things. If the given users violate the voting rules, their $$ is slashed (burned, destroyed, etc)
* Requiring collateral for voting is frequently used as an anti-sybil mechanism in proof of stake. @VitalikButerin has explored this idea in an article on slashing in early 2014 [slasher](https://blog.ethereum.org/2014/01/15/slasher-a-punitive-proof-of-stake-algorithm/)
* Vitalik enumerated Minimal Slashing Conditions in a blog post in March, 2017 [minimal slashing conditions](https://medium.com/@VitalikButerin/minimal-slashing-conditions-20f0b500fc6c)
* [Tweet storm on history of Ethereum PoS Development from Vitalik](https://twitter.com/VitalikButerin/status/1029900695925706753)