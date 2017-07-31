# Part 2: Blockchain Present and Beyond

This is a continuation from [Part 1]({% post_url 2017-07-29-blockchain-part-1.md})

Now let's talk about the state of the blockchain now, circa 2017. How has it changed since it was first used in 2009?

# Evolution of Bitcoin

Since Bitcoin was launched in early 2009 it has grown immensely. It is now a global force that is disrupting financial markets and inspiring startups to take the leap into blockchain technologies. 

While there have been major changes to Bitcoin since it was launched, its blockchain remains the same unbroken link of transactions. That chain is now more than 100GB, the overhead of which will have to be addressed in the near future. 

## Ethereum

One of the first steps of a blockchain moving beyond cryptocurrency was [Ethereum](https://en.wikipedia.org/wiki/Ethereum) in 2015, where it launched a platform that supported, in addition to cryptocurrency, smart contracts.

### Smart Contracts

Consider a traditional, legally binding contract. It's useful to review the definition of a contract, which [per Webster (1828)](http://webstersdictionary1828.com/Dictionary/contract), is:

> An agreement or covenant between two or more persons, in which each party binds himself to do or forbear some act, and each acquires a right to what the other promises

The enforcement of a contract is typically done through the law. Let's say Alice would like to contract with Eve to perform yardwork. The terms of the contract are that Alice weeds 100 dandelions. Upon completion of that task, Eve is contracted to pay Alice $20. The contract further stipulates that Alice may weed additional dandelions and be paid $.10 each. Finally, Eve's neighbor Bob is required to validate the number of dandelions picked. 

Alice begins pulling dandelions from Eve's yard and eventually finishes, having pulled 150 dandelions. Bob validates this number. Per the contract she is due $25, however Eve only pays her $10. At this point the contract has been broken and Alice may pursue legal action against Eve.

Smart contracts are protocols that allow a contract to be computerized in a way that they are automatically facilitated, verified, and enforced. Consider the example above as a smart contract instead. The terms are the same, but instead of being paid $20 for 100 and $.10 each thereafter, Eve promises to pay 100 ETH and .1 ETH each thereafter, ETH being the cryptocurrency of the Ethereum blockchain. The contract is included in the blockchain with the original conditions. Once Bob validates the number of dandelions picked, the smart contract would automatically execute and Alice would be given her 25 ETH. Furthermore, smart contracts allow this sort of interaction between anonymous and untrusted parties.

Smart contracts have been implemented and expanded in such a way that they are actually turing complete, making the Ethereum blockchain capable of performing arbitrary computations. This is what's called the [Ethereum Virtual Machine](https://en.wikipedia.org/wiki/Ethereum#Ethereum_Virtual_Machine). 

There is incredible potential in smart contracts, which we will discuss later. However, some of the possible use cases for smart contracts [have been shown to be infeasable](https://www.coindesk.com/three-smart-contract-misconceptions/). Therefore, it's important to understand how much overhead there is in adding instructions to the blockchain and how slowly these instructions execute in order to understand what place smart contracts can have in future blockchain technology.

One more thing - smart contracts are computer code. Computer code is only as secure as it was written. 

# Beyond Bitcoin: Permissioned Ledgers

Bitcoin is considered a permissionless system - in other words, its users are largely anonymous or at least pseudonymous. They do not require permission to become a part of the network. There is no gatekeeper. The transactions may be verified by the public, and transactions may be initialized by the public (as long as they have bitcoins to spend). It is completely public. 

On the other hand, a permissioned system has some sort of authentication to ensure that only permissioned users participate in the ledger. Permissioned ledgers may be validated by the public but may only be written to by trusted (permissioned) individuals. Or, they may be completely private - accessing and contributing to the ledger restricted to authenticated nodes only. 

Permissionless (Public):
* Publicly readable (& verifiable)
* Publicly writable

Permissioned (Private):
* Publicly or privately readable
* Privately writable

Permissioned ledgers really expand the potential applications of blockchain technology. For example, a bank may create a clone (or fork) of Bitcoin but only allow transactions signed by their members to be collated into blocks.

All the organizations that are uncomfortable with using a distributed ledger subject to an anonymous majority now have the opportunity to exploit the advantages of blockchains without sacrificing control. 

## Others

## ICO

# Bitcoin Future

## Applications

## Real Estate

## Government

[]({% post_url 2017-07-29-blockchain-part-3.md})