# Part 1: Blockchain Origin Story
 
For background information on the _why_ behind blockchain and distributed ledgers, read Part 0 [found here]({% post_url 2017-07-29-blockchain-part-0.md}).

A distributed digital currency was [one of the most sought-after applications for distributed systems](https://www.youtube.com/watch?v=Cod7U9IIz5U), however a huge problem stood in the way of its development: the [double spend problem](https://en.wikipedia.org/wiki/Double-spending). As the distributed ledger evolved and researches made progress in applied cryptography this obstacle was ultimately overcome.

## Table of Contents
- [Discovering the Chain](#discovering-the-chain)
    - [1991: Haber and Stornetta](#1991-haber-and-stornetta)
    - [2008: Satoshi Nakamoto](#2008-satoshi-nakamoto)
- [Crypto Review](#crypto-review)
    - [Cryptographic Hashing](#cryptographic-hashing)
    - [Public Key Cryptography](#public-key-cryptography)
- [Bitcoin](#bitcoin)
    - [Transactions](#transactions)
    - [Proof of Work](#proof-of-work)

# Discovering the Chain
## 1991: Haber and Stornetta

![Bellcore](/assets/images/posts/2017/bellcore.png)

In 1991, two employees of Bellcore (AT&T's telecom R&D company) Stuart Haber and W. Scott Stornetta published an article in the Journal of Cryptology on [_How to Time-Stamp a Digital Document_](https://link.springer.com/article/10.1007/BF00196791) using a cryptographically secured chain of blocks. Their research was motivated primarily by finding a way to build a ledger of changes made to a digital document. It wasn't until 15 years later that their research was put to use in solving the double spend problem.

## 2008: Satoshi Nakamoto

![Bitcoin](/assets/images/posts/2017/bitcoin.png)

Satoshi Nakamoto* published the seminal [Bitcoin paper](https://bitcoin.org/bitcoin.pdf) in 2008. The paper is surprisingly simple; it succinctly describes how a distributed ledger and proof of work (more on that later) could be used to ensure that mutually distrusting peers could engage in currency transactions without fear of double spending. 

On January 3, 2009, shortly after Nakamoto published the paper, the Bitcoin genesis block was created.

I recommend the paper to anyone with a intermediate understading of cryptography. I'll still go through some blockchain basics as described in the paper, but before we dive into the technical details of the blockchain, let's review some cryptography basics. If you need more than this review, I recommend watching these short videos on [public key cryptography](https://www.youtube.com/watch?v=wXB-V_Keiu8) and [cryptographic hashing](https://www.youtube.com/watch?v=0WiTaBI82Mc). 

\*<sub>[Satoshi Nakamoto](https://en.wikipedia.org/wiki/Satoshi_Nakamoto) is the pseudonym used by the creator of Bitcoin. His identity is still not public.</sub>

# Crypto Review

## Cryptographic Hashing
Crytographic hashes are secure one-way functions; they will easily convert a message to a fixed-length unique bit-string, but they are non-invertible. The only way to reverse a hash function is by a brute-force search which could take thousands of years, if not more, depending on the hashing algorithm used. 

Here is an example of a linux command that takes the two words `blockchain` and `Blockchain` and hashes them using the SHA256 algorithm. The output is the hexidecimal digest representation of the bitstring. They are vastly different, even though the input words are very similar. 

```{sh}
$ echo 'blockchain' | sha256sum
5318d781b12ce55a4a21737bc6c7906db0717d0302e654670d54fe048c82b041
$ echo 'Blockchain' | sha256sum
fe7d0290395212c39e78ea24ba718911af16effa13b48d1f6c9d86e8355e0770
```

Figure 1 pushes the point further:

![hash example](/assets/images/posts/2017/hash.png)
*Figure 1 - Hash Diagram*

If you're not sure why you would even want a one-way function, [here are some applications of hashing](https://en.wikipedia.org/wiki/Cryptographic_hash_function#Applications).

## Public Key Cryptography
The strength of blockchain security lies in its use of public key cryptography. It is also called assymetrical cryptography because it is comprised of _two_ keys - a public key and a private key. Both keys may be used to encrypt data, however they may only _decrypt_ data that was encrypted by its partner key. In other words, if you encrypt a message with a public key, only the private key may decrypt that message. In like manner, only the public key may decrypt messages that were encrypted by the private key. 

![public key flowchart](/assets/images/posts/2017/publickey.png)
*Figure 2 - Public Key Flowchart*

This property of public key cryptography enables some very important security features that we rely on daily, whether we know it or not:

* Confidentiality - easily send a confidential message to anyone using their public key
* Key Exchange - the ability to securely share an assymetric key with anyone
* Message Signing ([non-repudiation](https://en.wikipedia.org/wiki/Non-repudiation)) - ensure a message really is from the person you think it is

This little detour will hopefully make understanding the Bitcoin blockchain easier.

# Bitcoin

Detailed in the Bitcoin paper, the blockchain is surprisingly simple. 

## Transactions

Like any ledger, Bitcoin is made up of __transactions__. Eve (Owner 1 in Figure 3) sends 5 BTC (bitcoins) to Bob (Owner 2). Eve simply creates a new transaction with the details (5 BTC), includes Bob's public key (this is considered his _address_), and signs the transaction with her private key. Since she is the only one who could be the author of that signature, and since she was the previous owner of that money, the transaction is considered valid. As long as Bob maintains control of his private key, he controls that money, since the private key is all that is needed to move the money to someone else. 

![Bitcoin transactions](/assets/images/posts/2017/bitcoin_transactions.png)
*Figure 3 - Bitcoin Transactions*

This system allows for a non-repudiable chain of events (a ledger) to be established. Is this enough for digital currency? Not yet! Tthere is no mechanism in place to ensure that Eve doesn't double spend her money. What if she sends the same 5 BTC to Alice? And then to a dozen other recipients? This is the problem of double-spending

## Proof of Work

Nakamoto suggested a solution to the double spend problem by a proof-of-work network that serves to maintain the truth and _consensus_ of the state of the ledger by hashing a group of transactions at regular intervals. The transactions are collected into _blocks_ which are then hashed along with the previous block. This creates a chain of blocks that are cryptographically validated and are resistent to forgery - a blockchain (Figure 4).

![blockchain](/assets/images/posts/2017/blockchain.png)
*Figure 4 - Blockchain*

In essence, _miners_ on the proof-of-work network are competing to complete a block. A miner that is successful in mining a block gets a reward of some number of bitcoins plus transaction fees. Completing a block requires the following:
1. Some number of transactions (Tx)
1. The previous block's hash
1. A nonce that meets the current _difficulty target_

\#3 is the core of the proof-of-work concept. [The difficulty target](https://en.bitcoin.it/wiki/Target) is a number wherein the hash of the block must be lower than or equal to for it to be accepted. As the difficulty target gets smaller it becomes more difficult to mine a block - and this number is adjusted to try and keep the block production to one every ten minutes. So, if there is an increase in miners competing for a block, the difficulty target is adjusted to keep the block production rate steady.

Consensus between miners and users is managed by the simple concept of accepting the longest legitimate chain of blocks. So if a miner computes a block it is immediately accepted by everyone else and all miners start working on a new block. A very important part of this proof-of-work system is that it makes it extremely difficult for Eve to double-spend. For her to double-spend she would have to sign the duplicate transactions at some point in the chain and then recompute the chain of transactions to make her illegitimate chain the longest. This is infeasable because it would require the majority of all mining power to be dedicated toward this goal - and even then it would only allow Eve to double-spend, not conjur bitcoins out of thin air. If she could muster that kind of compute power she would be much better off just using it to mine new bitcoins. 

There certainly is more to Bitcoin than just these, especially now since Bitcoin has evolved beyond Nakamoto's Bitcoin paper. But for now we will use this basic understanding of blockchain to help us understand the state of blockchains today. 

Continue to [Part 2, Blockchain Present and Future]({% post_url 2017-07-29-blockchain-part-2.md})
