# Part 2: Blockchain Present and Beyond

This is a continuation from [Part 1]({% post_url 2017-07-29-blockchain-part-1.md})

Now let's talk about the state of blockchain technologies now, circa 2017. How has it changed since it was first used in 2009?

# Evolution of Bitcoin

Since Bitcoin was launched in early 2009 it has grown immensely. It is now disrupting financial markets and inspiring startups to follow suit and leap into blockchain technologies. 

While there have been major changes to Bitcoin since it was launched, its blockchain remains the same unbroken link of transactions from the genesis block. That chain is now more than 100GB and it's still growing - even faster as the popularity of Bitcoin increases. This overhead will have to be addressed in the near future. 

Since the Bitcoin source code is open, it's possible to fork the code base and change it to create an 'altcoin'. In fact, many have done this and have varying levels of support and valuation. The valuation of cryptocurrency is an interesting study and certainly keeps a lot of people busy guessing - and has made some of them very wealthy. 



## Ethereum

![Ethereum](/assets/images/posts/2017/ethereum.png)

For some time blockchains were primarily proof of work cryptocurrencies. One of the first steps of a blockchain moving beyond this was [Ethereum](https://en.wikipedia.org/wiki/Ethereum) in 2015, when Ethereum launched a platform that supported, in addition to cryptocurrency, smart contracts.

### Smart Contracts

Smart contracts take traditional contracts and make them a part of a distributed ledger. First, let's consider a traditional, legally binding contract. It's useful to review the definition of a contract, which [per Webster 1828](http://webstersdictionary1828.com/Dictionary/contract), is:

> An agreement or covenant between two or more persons, in which each party binds himself to do or forbear some act, and each acquires a right to what the other promises

The enforcement of a contract is typically done through the law. Let's say Alice would like to contract with Eve to perform yardwork. The terms of the contract are that Alice weeds 100 dandelions. Upon completion of that task, Eve is contracted to pay Alice $20. The contract further stipulates that Alice may weed additional dandelions and be paid $.10 each. Finally, Eve's neighbor Bob is named as a mediator to validate the number of dandelions picked if a dispute arises. 

Alice begins pulling dandelions from Eve's yard and eventually finishes, having pulled 150 dandelions. Per the contract Alice is due $25, however Eve only pays her $10. At this point the contract has been broken and Alice may bring in Bob as mediator and possibly pursue legal action against Eve.

|Party|Role|
|-----|----|
|Alice|Pick dandelions|
|Eve  |Pay agreed rate of $.20 per dandelion up to 100, $.10 thereafter|
|Bob  |Arbitrate payment|

Smart contracts are protocols that allow a contract to be computerized in a way that they are automatically facilitated, verified, and enforced. Consider the example above as a smart contract instead. The terms are the same, but instead of being paid $20 for 100 and $.10 each thereafter, Eve promises to pay 100 ETH and .1 ETH each thereafter, ETH being the cryptocurrency of the Ethereum blockchain. The contract is included in the blockchain with the original conditions. Once both Alice and Eve agree on the number of dandelions picked, the smart contract would automatically execute and Alice would be given her 25 ETH. Bob may still play a role as a third party, he simply needs to be written into the smart contract. Furthermore, smart contracts allow this sort of interaction between anonymous and untrusted parties.

![Smart contract](/assets/images/posts/2017/smart-contract.png)
*Figure 1 - Smart Contract Example for Insurance*

The below image generalized well how a smart contract might live on a blockchain (shared, replicated ledger).

<!-- ![smart contract](/assets/images/posts/2017/smart_contract.png)
*Image credit: Jo Lang / R3 CEV* -->

The Ethereum Project made an interesting decision to further expand smart contracts - [Turing completeness](https://en.wikipedia.org/wiki/Turing_completeness). This extension to smart contracts makes the Ethereum blockchain capable of performing arbitrary computations. This became the [Ethereum Virtual Machine](https://en.wikipedia.org/wiki/Ethereum#Ethereum_Virtual_Machine). 

There is incredible potential in , which we will discuss later. However, some of the proposed use cases for smart contracts [have been shown to be infeasable](https://www.coindesk.com/three-smart-contract-misconceptions/). Smart contracts have limitations, and it's important to understand what place they might have in future blockchain technology, or your own organization. 

Additionally, blockchain virtual machines have a huge amount of overhead when executing instructions - porting Doom to the Ethereum Virtual Machine is probably a futile effort.

Smart contracts are computer code. Computer code may have flaws, and arbitrary code will have arbitrary flaws. It's only a matter of time until smart contracts are executed erroneously using a vulnerability in the smart contract code. 



### Beyond Smart Contracts

While smart contracts were originally intended to mirror legal contracts, they have been expanded on the Ethereum blockchain in such a way that they are Turing complete. What does this mean? In essence, a Turing complete system is capapble of performing all the calculations necessary to make it a *computer* by our modern definition.

A good definition for _smart_ contracts was given by [Richard Brown](https://gendal.me/2015/02/10/a-simple-model-for-smart-contracts/):

> A smart-contract is an event-driven program, with state, which runs on a replicated, shared ledger and which can take custody over assets on that ledger.

If something is Turing complete, it can calculate pi, send an email, and play *Doom*. Can the Ethereum blockchain? Yes, but **very slowly** ([25 transactions per second as of January 2016](https://en.wikipedia.org/wiki/Ethereum#Performance)). This is what Ethereum calls the Ethereum World Computer. It sounds a bit like Deep Thought from *Hitchhiker's Guide to the Galaxy*, but maybe a bit faster at coming up with the answer *42*.

<!-- ![Deep Thought](/assets/images/posts/2017/deepthought.png)
*Image credit: Hitchhiker's Guide to the Galaxy* -->

There is incredible potential in smart contracts, which we will explore briefly when we consider the future of blockchains. However, some of the suggested use cases for smart contracts [have been shown to be infeasable](https://www.coindesk.com/three-smart-contract-misconceptions/). Therefore, it's important to understand how much overhead there is in adding instructions to the blockchain and how slowly these instructions execute in order to understand what place smart contracts can have in future blockchain technology.

One more thing - smart contracts are computer code, and computer code [may contain bugs](https://qz.com/1034321/ethereum-hack-a-coding-error-led-to-30-million-in-ethereum-being-stolen/). Combining the possibility of bugs with the immutability of the blockchain should be considered by anyone embracing smart contracts.

## Proof of Stake

Proof-of-Stake ([PoS](https://en.wikipedia.org/wiki/Proof-of-stake)) algorithms are an alternative to proof-of-work mechanism used to achieve distributed consensus among peers. Instead of competing to generate an acceptable hash using intense computational power, PoS determines the next block winner in a deterministic way based on the stake of the account holder. 

There are different implementations that use different methods to choose the next block owner, such as randomization or 'coin age' (the time since that coin has been awarded a block). In either case, the probability increases with the amount of stake the account holder has. If you own more coins, the likelihood that you will be awarded a block is increased.

This area of development is very interesting because the drawbacks of proof-of-work increase as the competition for blocks increase. For example in 2014 the energy consumption to mint a bitcoin was equivalent to [16 gallons of gasoline](http://www.coindesk.com/carbon-footprint-bitcoin/). The costs have only increased. Ethereum has announced they [plan to move to proof-of-stake](https://www.ethnews.com/ethereums-road-map-for-2017) for block generation in the future and many new cryptocurrencies are opting to use proof-of-stake instead.

# Beyond Bitcoin: Permissioned Ledgers

Bitcoin and Ethereum are considered permissionless systems - in other words, their users are largely anonymous or at least pseudonymous. They do not require permission to become a part of either network. There are no gatekeepers. The transactions may be verified by the public, and transactions may be initialized by the public (as long as they have bitcoins to spend).

Conversely, a permissioned system has some sort of authentication to ensure that only permissioned users participate in the ledger. Permissioned ledgers may be validated by the public but may only be written to by trusted (permissioned) individuals. Or, they may be completely private - accessing and contributing to the ledger restricted to authenticated nodes only. 

**Permissionless (Public):**
* Publicly readable (& verifiable)
* Publicly writable

**Permissioned (Private):**
* Publicly or privately readable
* Privately writable

Permissioned ledgers really expand the potential applications of blockchain technology. For example, a bank is likely not interested in a cryptocurrency that relies on anonymous validation (such as Bitcoin or Ethereum), especially when they want to send money to another bank that they know. Instead of using permissionless cryptocurrencies, the bank may create a clone (or fork) of Bitcoin but only allow transactions signed by known good actors (that they can authenticate).

All the organizations that are uncomfortable with using a distributed ledger subject to an anonymous majority now have the opportunity to exploit the advantages of blockchains without sacrificing control or paying for the expense of proof-of-work.

## Others

Beyond the [myriad althernative cryptocurrencies](https://en.wikipedia.org/wiki/List_of_cryptocurrencies) (altcoins) that have sprung up in the years since Bitcoin, there are some noteworthy applications of blockchains that are worthy of a quick overview.

### ICO

A phenomenon that has cropped up in the last few months has been the Initial Coin Offering (ICO). Meant to mimic an Initial Public Offering (IPO), ICOs offer a limited block of cryptocurrency for sale to the public. This is meant to generate an influx of cash into the company and, like an IPO, is a risky venture.

ICOs don't represent anything new in blockchain technology, but are instead new ways of distributing the cryptocurrency. They are likely here to stay, at least as long as cryptocurrency is. 

# Bitcoin Future

## Off-Ledger Integration

Since much of the blockchain development has been around permissionless (and anonymous) blockchains, the idea of involving off-chain assets (property, goods, services) has not been seriously attempted until fairly recently. One of the problems facing integrating off-chain assets is legal enforcability of blockchain contracts. If a permissioned ledger uses legally accountable validators (miner) and meets other legal criteria, it could pass government regulatory requirements.

## Real Estate

Extending smart contracts to real estate would have incredible potential to bypass an entire cottage industry dedicated to nickle-and-diming homebuyers. Buying a home is an expensive and risky process, but if you could automate much of the contracts through a distributed ledger with smart contracts that appropriately (and legally) incorporated off-ledger assets (such as titles), the cost and time required to buy a houst could be significantly decreased. 

## Government

Government beurocracy costs us [billions annually](http://harvardpolitics.com/arusa/under-the-hood-the-cost-of-bureaucracy/). Incorporating distributed ledgers with trusted verifiers maintained by the government would save the American taxpayer a significant amount of money by ensuring automatic regulatory compliance. Furthermore, the various state governments could each maintain their own authorized permissioned ledgers that interoperate with other ledgers, such as those envisioned by Hyperledger below. Much of government records are public record - maintaining them in the public eye in a verifiable manner would bring our government into the digital age in a manner never conceived. 

Private ledgers promise to provide the same benefits for data that isn't public, but still needs a verifiable chain of custody. The original vision for blockchains was to timestamp edits to a digital document. Private ledgers could provide similar chains of custody for classified documents that would help avoid a Snowden-like scenario in the future.

## Hyperledger

<!-- ![Hyperledger Logo](/assets/images/posts/2017/hyperledger.png)
*Image credit: The Linux Foundation* -->

Started by the Linux foundation in 2015, [Hyperledger](https://www.hyperledger.org/) is a collection of open source blockchains and tools. It is made up of different fledgling platforms that are sponsored by different organizations, including Intel and IBM.

Their vision includes enabling different parties to securely interact with the same universal source of truth, including:

#### Financial Institutions
<!-- ![hyperledger banks](/assets/images/posts/2017/hyper_banks.png) -->
#### Healthcare Organizations
<!-- ![hyperledger banks](/assets/images/posts/2017/hyper_banks.png) -->
#### Transportation Organizations 
<!-- ![hyperledger banks](/assets/images/posts/2017/hyper_banks.png) -->

The idea is that any industry could benefit from the immutability and public verifiability that blockchain offers.

### Sovrin (Hyperledger Indy)

<!-- ![Sovrin Logo](/assets/images/posts/2017/sovrin.svg)
*Image credit: Sovrin Foundation* -->

One of the more ambitions Hyperledger frameworks is Hyperledger Indy sponsored by the [Sovrin Foundation](https://sovrin.org/). Sovrin aims to utilize blockchain technology to provide each human being a sovreign, digital identity. The foundation aims to provide identies that are "100% owned and controlled by an individual or organization" that can interoperate with other distributed ledgers. 

---

Now, let's see how a blockchain proof-of-concept looks. 

[Continue to Part 3: Privledge]({% post_url 2017-07-29-blockchain-part-3.md})
