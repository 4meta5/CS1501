# Zero Knowledge Proofs

> today we'll take a foray into one of the most interesting ideas in cryptography

**Roadmap:** Zero Knowledge Proofs => Brief History of Digital Privacy => Zcash => zk-SNARKs => zk-STARKs

> *In the end, I think applying these privacy and anonymity technologies to every blockchain out there is going to be essential because to limit freedom of expression, to limit freedom of association, you don't only have to deal with prior restraint. If you have the freedom to transact with everyone, but all your transactions are visible to everyone, then you can be punished after the fact for transacting with the wrong people, which essentially robs you of your freedom to transact. We have to realise that privacy is a foundational human right and without it, freedom of association, freedom of (political) expression, all go away.* - Andreas Antonopolous 

## Brief History of Digital Privacy
> *If privacy is outlawed, then only outlaws will have privacy.* - Philip R. Zimmermann, [Why I Wrote PGP](https://www.philzimmermann.com/EN/essays/WhyIWrotePGP.html)

Designed to empower individual rights, Pretty Good Privacy (PGP) was published on the Internet in 1991 by Phil Zimmermann. PGP is an encryption program that provides cryptographic privacy and authentication for data communication. By open sourcing PGP, Zimmermann violated the US export restrictions for cryptographic software, putting him at the center of a brutal criminal investigation that spanned nearly half a decade. In 1996, the US government dropped its case against Zimmermann, but the fight for privacy was far from over. As Zimmermann recounts in [Why I Wrote PGP](https://www.philzimmermann.com/EN/essays/WhyIWrotePGP.html), *"the only way to hold the line on privacy in the information age is strong cryptography"*.

Although the [struggle for control over digital interactions](https://en.wikipedia.org/wiki/Crypto_Wars#cite_note-55) [has](https://arstechnica.com/tech-policy/2015/01/uk-prime-minister-wants-backdoors-into-messaging-apps-or-hell-ban-them/) [not](https://blogs.wsj.com/digits/2015/01/16/obama-sides-with-cameron-in-encryption-fight/?guid=BL-DGB-39944&dsk=y) abated, progress on the implementation of cryptography has enabled the de facto encryption of communication over the Internet via SSL (Secure Socket Layer) and TLS (Transport Layer Security). When these protocols were first introduced, there was much of the same criticism that we see today in the form of "encryption enables criminal activity" and "you shouldn't be scared if you have nothing to hide". While these arguments may seem appealing on the surface, they ignore history by assuming **trust** in the underlying system and everyone else that uses it. 

This is a mistake. To foster an environment in which people can be truly safe, we must aspire towards systems in which *the requirement of trust itself is minimized*. Blockchain technology is a step in the right direction; it minimizes trust in the underlying system by incentivizing cooperation among a network's stakeholders [1]. Still, most contemporary blockchains operate as open public ledgers, displaying transaction data pseudonymously. This can lead to [critical leaks of user identity](https://www.technologyreview.com/s/608716/bitcoin-transactions-arent-as-anonymous-as-everyone-hoped/). To alleviate this problem and uphold digital privacy in all forms, it bears repeating the vision first set out by Eric Hughes in [A Cypherpunk Manifesto](https://www.activism.net/cypherpunk/manifesto.html): *"privacy in an open society requires anonymous transaction systems...An anonymous system empowers individuals to reveal their identity when desired and only when desired; this is the essence of privacy"*. 

[1] see explanations by (i) [Vitalik](https://blog.ethereum.org/2015/04/27/visions-part-2-the-problem-of-trust/) and (ii) [Preethi](https://medium.com/@preethikasireddy/eli5-what-do-we-mean-by-blockchains-are-trustless-aa420635d5f6) for a more nuanced explanation of how blockchains minimize trust

## Zcash 
Building on decades of research, Zcash was developed and launched by top cryptographers from MIT, Technion, John Hopkins, Tel Aviv University, and UC Berkeley. [Zcash](https://z.cash/) is a blockchain that ensures complete confidentiality of transaction data. Zero-knowledge proofs allow transactions to be verified without revealing the sender, receiver or transaction amount.

> [Zcash ceremony](https://www.wnycstudios.org/story/ceremony)

**Relevant ZeroKnowledgeFM Podcasts**
* [Zooko talks Zcash](https://www.zeroknowledge.fm/50)
* [Zokrates with Jacob Eberhardt](https://www.zeroknowledge.fm/41)
* [Benedikt Bunz on Bulletproofs and Verifiable Delay Functions](https://www.zeroknowledge.fm/40)
* [Intro to zkSNARKs with Howard Wu](https://www.zeroknowledge.fm/38)
* [Zero Knowledge at Zcon0](https://www.zeroknowledge.fm/32)
* [Introduction to Zero Knowledge Proofs](https://www.zeroknowledge.fm/21)

**More Info on Zcash**
* [How It Works](https://z.cash/technology/)
* [What are zk-SNARKs](https://z.cash/technology/zksnarks/)
* [Cultivating Sapling: Faster zk-SNARKs](https://z.cash/blog/cultivating-sapling-faster-zksnarks)
* [Anatomy of A Zcash Transaction](https://z.cash/blog/anatomy-of-zcash/)

**Vitalik's 3-part zk-SNARKs Explainer**
1. [Exploring Elliptic Curve Pairings](https://medium.com/@VitalikButerin/exploring-elliptic-curve-pairings-c73c1864e627)
2. [Quadratic Arithmetic Programs: from Zero to Hero](https://medium.com/@VitalikButerin/quadratic-arithmetic-programs-from-zero-to-hero-f6d558cea649)
3. [zk-SNARKs: Under the Hood](https://medium.com/@VitalikButerin/zk-snarks-under-the-hood-b33151a013f6)

**Other Useful Resources**
* [A (Relatively Easy to Understand) Primer on Elliptic Curve Cryptography](https://blog.cloudflare.com/a-relatively-easy-to-understand-primer-on-elliptic-curve-cryptography/) by Nick Sullivan of Cloudflare
* [Introducing Elliptic Curves](https://jeremykun.com/2014/02/08/introducing-elliptic-curves/) by Jeremy Kun
* [Zero-knowledge proofs intro with Str4d](https://www.youtube.com/watch?v=Y9YgRDJAFEE&t=12s)
* [zkSNARKs in a nutshell](https://blog.ethereum.org/2016/12/05/zksnarks-in-a-nutshell/) by Christian Reitweissner (Ethereum Blog Post)