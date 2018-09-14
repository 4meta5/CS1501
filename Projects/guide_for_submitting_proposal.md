<!-- 
Objectives:
(1) Design structure so that markdown files can easily be parsed into JSON objects in the future
(2) Borrow from Aragon Markdown files https://github.com/aragon/nest/blob/master/Guide_for_submitting_a_request_for_funding.md
 -->
## Breakdown of what you should include in a Request to Post a Project

[Open Roles](/open_roles.md)

We're going to use issues to manage project proposals as well as any relevant discussion. 

> # Request to Post Project (#X)
> _Remember to mention the [project proposal](https://github.com/AmarRSingh/CS1501/issues) Issue number that your creating the request for_

**Team name**:
> _Name of your project (can leave this empty)_

**Open Roles**:
> _What parts of the project are you seeking collaboration for and what experience could be useful_

## Proposal
> _Detailed description of the project_

___
## Pull Request example for submitting a request for posting a project
```
# Request to Post Project (#X)

**Team name**: Smerkle

**[Proof of Concept](https://github.com/smerkle)**

**Open Roles**: 
1. [Developer Roles]():
  * Building an [Aragon](https://aragon.org) DApp. Preferences: JavaScript (or willing to learn), interest in Economics/Governance
  * Scaling to Production with [Parity](https://paritytech.io)/[Polkadot](https://polkadot.network). Preferences: Rust (or willing to learn)
  * (Coming Soon) Data Analyst Role for Analyzing Information Database. Preferences: Python 
2. [Research Roles]():
  * Read and research the mechanisms discussed in [Radical Markets](http://radicalmarkets.com). Preference: Interest in Economics/Governance
2. [Community Manager Roles]():
  * Management of Twitter, Peepeth, Reddit, Telegram, Riot, etc.

## Proposal
[Smerkle](https://github.com/smerkle) is a database for blockchain knowledge. The first iteration, [Valdrada]() will function as a free library for the community. In the future, we plan to migrate the database permissions to enable decentralized governance to curate content. Because our primary use case is organizing information, we will start with a simple Node.js server before gradually migrating to decentralized storage as well as Ethereum smart contracts to govern permissions. Eventually we want to migrate to Parity/Polkadot because [Rust](https://paritytech.io/why-rust/); also we believe in the team and the [tech](https://paritytech.io/what-is-substrate/) that they're developing.

```
### **Preview of an Pull Request example:**
> # Request to add Smerkle (#X)
>
> **Team name**: Smerkle
>
> **Proof of concept**: https://github.com/smerkle
>
> **Open Roles**: [Developer](), [Research](), [Community]()
>
> ## Proposal
>
> [Smerkle]() is a database for blockchain knowledge. The first iteration, [Valdrada]() will function as a free library for the community.
> In the future, we plan to migrate the database permissions to enable decentralized governance to curate content. Because our primary use case is organizing information, we will start with a simple Node.js server before gradually migrating to decentralized storage as well as Ethereum smart contracts to govern permissions. 
> Eventually we want to migrate to Parity/Polkadot because [Rust](https://paritytech.io/why-rust/); also we believe in the team and the [tech](https://paritytech.io/what-is-substrate/) that they're developing.

___
## Empty Pull Request template for submitting a request for funding
```
# Request to Post Project(#X)

**Team name**:

**Proof of concept**:

**Open Roles**:

## Proposal
```


# ProjectName

## 

Model this after EIPs but make it as short as possible
```
proposal: {
        project_summary: __,
        github: ___,
        other_resources: ___,
        name (optional): __,
}

```
When a proposal is submitted, it is first sent into a discussion feed. It must achieve some level of support before it can be formally added to the list of open projects. Open projects usually have roles posted that describe an informal criteria that may be suitable for the position. We encourage any involved projects to be accessible to students of all experience levels. 

Open Projects

My list of projects:
* Knowledge Graphs
* State Channel Projects
* Learning Rust and Solidity
* Working with NuCypher tools in Python to learn about file encryption (other encryption tasks like file encryption)

# Cryptography

# Economics/Governance
Governance based on Radical Markets Book:
* Quadratic Voting
* Liberal Radicalism

# BUIDL
For projects, we will encourage students to form teams to explore promising software in the space. We believe that now is a very good time to learn how to interact with permissionless networks.

Coming Soon Maybe (Hardware)
Need to find someone to fill this position.