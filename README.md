# Raze_Network

> This document is referenced in the terms and conditions and therefore needs to contain all the required information. Don't remove any of the mandatory parts presented in bold letters or as headlines! See the [Open Grants Program Process](https://github.com/w3f/Open-Grants-Program/blob/master/README_2.md) on how to submit a proposal.

* **Project:** Raze Network
* **Proposer:**
* **Payment Address:** 


## Project Overview

Raze network is a second-layer protocol that will provide cross-chain payment privacy for the entire DeFi stack of the Polkadot ecosystem. The core technical module of the Raze network is a second-layer decentralized anonymous payment module, which will serve as a universal plugin-and-play infrastructure for the Polkadot DeFi ecosystem. This module will be imported as a substrate-based smart contract, which allows the user to hide one's account address and financial information (as shown in the Figure below) before participating the Polkadot DeFi stacks, be it DEX, liquidity mining, loan, insurance, etc. Since the Zether framework was designed to support the Ethereum account model, our contract will support both ERC-20 cross-chain private payment and the mainstream token issued in the Polkadot ecosystem such as DOT/KUSAMA, and hence significantly increase the liquidity of the Polkadot ecosystem. Our ultimate goal is to protect the Polkadot users from the surveillance of big brother. 

![alt text](https://github.com/razenetwork/Raze_Network/blob/main/raze_polkadot.png?raw=true)

### Project Details 

We will apply the Zether framework to build the second-layer decentralized anonymous payment module. It will be then imported as a substrate-based smart contract. Similar to the Zether framework, it will have three technical modules: mint, transfer and redeem. The mint module will convert any ERC-20 token into its anonymized version, while the redeem module will convert the anonymized token back to its native form. The transfer module is the one that enables the anonymous transfer of the anonymized token. It will conceal the transaction amount and guarantee the anonymity of both sender and receiver. 

The mint module will create a Raze account by running a mint contract and deposit the anonymized token to the Raze account. Each Raze account is identified by a public key `pk` and the mint module will generate a ciphertext under the account public key which encrypts the amount of minted token. If the account has already existed before the mint operation, the generated ciphertext will be homomorphically added to the existing ciphertext to increase the encrypted amount.   

Since the transaction amount of each Raze account is encrypted and therefore hidden, the remaining question for the transfer module is how to hide the identities of the involved parties. Similar to the anonymous transfer module of the Zether scheme, we hide the identities of the involved parties through the "one-out-of-many" proof. The "one-out-of-many" proof is conceptually close to ring signature, which proves that a transaction is launched by one of the many parties in the anonymity set (or the ring) while not revealing exactly who among them is the sender or receiver of the transaction, and thus hide their identities. The transfer module's zkp will also prove the payment consistency and provide a range proof demonstrating there is no negative amount involved in the transaction, which could potentially allow the adversary to create money out of thin air. 

The redeem module converts the anonymized token back to its original form. The redeem module also needs to invoke a zero-knowledge proof module which aims to prove that the user initiating the redeem module knows the secret key of the corresponding Raze account and the redeemed amount is smaller than the Raze account balance. 

To sum up, we use public-key homomorphic encryption to protect the balance and transactional amount confidentiality and invoke the "one-out-of-many" proof to hide the sender and receiver identities, and some other zero-knowledge proof schemes to guarantee the payment consistency. The raze network can be viewed as a pool of boiling water, where each water molecule interacts with each other in a chaotic and vibrant fashion. Whenever a user deposits a certain amount of token through invoking the mint module, the token would be like a water molecule dropping into this pool of boiling water, it is no longer traceable.  

To facilitate the DeFi functionality, we have two additional modules: lock and unlock. The lock module would allow an account owner to lock the account while the unlock module allows the account owner to unlock the account. 

We will also build a cross-chain bridge that can map any ERC-20 token to the Polkadot blockchain and thus enable the cross-chain payment of these tokens.

![alt text](https://github.com/razenetwork/Raze_Network/blob/main/raze_architecture.png?raw=true)

Each raze user can register a raze account any time (s)he wishes. The registration algorithm CreateAddress generates a secret key `sk` and the corresponding public key `pk`. The public key is the identifier of the raze account. 

To invoke the Mint contract, the client-side runs a CreateMintTx algorithm, which takes as input the raze account `pk` and the amount of native token `amt` as inputs. The output of the CreateMintTx algorithm is a ciphertext `cp_1` encrypted under the public key `pk`. If the raze account `pk` is already attached with a ciphertext `cp_0`, the newly created ciphertext will be homomorphically added to the existing ciphertext to increase the encrypted amount. The updated ciphertext will be formed as `cp_1*cp_0`. Otherwise, the new ciphertext will be attached to the raze account. The native token will be stored in the mint contract. 

To invoke the transfer contract, the client-side runs a CreateTransferTx algorithm, which takes as inputs the raze account secret key `sk`, and the amount of transferred token `amt`, the public keys of sender `pk_s`, receiver `pk_r` and the public keys of the anonymity set `{pk_a}`. The output of the CreateTransferTx algorithm is a zero-knowledge proof that the prover knows one of the secret keys of the aforementioned public key set, the consistency proof of the payment, and range proof. The statement of this zkp is 
![alt text](https://github.com/razenetwork/Raze_Network/blob/main/zkp_statement.png?raw=true)

The client-side runs a CreateRedeemTx algorithm to invoke the redeem contract. It takes the account secret key `sk`, the withdrawal amount `amt`, and the public key `pk` as input to generate a zero-knowledge proof showing that the user knows the secret key `sk` for the account public key `pk` and the account has enough balance for the withdraw operation. The zero-knowledge proof will be used to invoke the Redeem contract. The statement of this zkp is 
![alt text](https://github.com/razenetwork/Raze_Network/blob/main/redeem_statement.png?raw=true)

The user can invoke the lock module by running a CreateLockTx algorithm on the client-side. The client inputs a secret key `sk` and an Ethereum address `addr` to generate a signature to demonstrate he is indeed the owner of the account and he authorizes to lock the account to the input address `addr`. The signature would be `Sign(x, addr)`. Similarly, the user can invoke the unlock module by running a CreateUnlockTx on the client-side. The input of CreateUnlockTx algorithm is the same as that of the CreateLockTx algorithm. It will generate a similar signature to unlock the account. Note, we will embed a nonce derived from the current epoch number to prevent the replay attack. 

### Substrate/Polkadot Integration

The raze network will be implemented as Substrate modules. More specifically, we will build a raze substrate pallet that supports:

* The user could mint anonymized token by invoking the Mint module. It will support all the mainstream tokens issued on the Polkadot blockchain such as DOT or KUSAMA and any ERC-20 token.  
* A user who owns a raze account could transfer the anonymized token to another raze account by running the Transfer module to hide the identities of the involved parties and the transferred amount.
* A user could run the Redeem module to convert the anonymized token back to its native form.
* The lock and unlock modules will allow the users to participate in the anonymity mining, and thus provide incentives to the users to join the raze network and thus increase the anonymity set. 

The ledger state will mainly keep a record of the raze accounts, the balance ciphertexts, and the pending transfer tables, etc. 

### Ecosystem Fit 

Decentralized finance (DeFi) in the Polkadot ecosystem is still a newborn. Privacy is essential to the success and prosperity of DeFi. The raze network will serve as a vital infrastructure for the future development of privacy-preserving DeFi in the Polkadot ecosystem. The raze network will not only liberate the Polkadot DeFi ecosystem from the surveillance of the big brother, but also significantly increase the liquidity of the Polkadot ecosystem. 
The closest product to the Raze network is the Manta network, which is a privacy-preserving DEX system. Our proposal differs from theirs in two regards: 
* The Zether framework is designed for the Ethereum account-based model, and hence it naturally facilitates the cross-chain private payment of ERC-20 tokens in the Polkadot ecosystem. Since all the aforementioned smart contract modules are supposed to be imported as Substrate-based modules, they will naturally be compatible with the tokens issued in the Polkadot ecosystem, such as DOT or KUSAMA. To sum up, our work will significantly increase the liquidity of the Polkadot system due to its cross-chain interoperability. 
* our product is much more generic in the sense that it supports all the DeFi products in the Polkadot system, and it also supports anonymity mining, which in itself is already a vital DeFi feature. In contrast, the Manta network focuses solely on the private DEX system, which limits the scope of its application scenarios severely. 
* From the technical perspective, the underlying zero-knowledge proof scheme for the Manta network requires trusted setup, which is why they require a trust ceremony. In contrast, the zero-knowledge proof scheme we adopt does not require any trusted setup. All the public parameters of our system can be randomly sampled from the underlying group in a totally transparent fashion, which is much more in accordance with the decentralization ethos. 

## Team :busts_in_silhouette:

### Team members
* Name of team leader
* Names of team members	

### Contact
* **Contact Name:** Full name of the contact person (e.g. John Brown)
* **Contact Email:** Contact email (e.g. john@duo.com)
* Website

### Legal Structure 
* **Registered Address:** Address of your registered legal entity, if available. Please keep it on one line. (e.g. High Street 1, London LK1 234, UK)
* **Registered Legal Entity:** Name of your registered legal entity, if available. (e.g. Duo Ltd.)

### Team's experience
Please describe the team's relevant experience.  If the project involves development work, then we'd appreciated if you can single out a few interesting code commits made by team members on their past projects. For research-related grants, references to past publications and projects in a related domain are helpful.  

### Team Code Repos
* https://github.com/<your_repo_1>
* https://github.com/<your_repo_2>

### Team LinkedIn Profiles
* https://www.linkedin.com/<person_1>
* https://www.linkedin.com/<person_2>

## Development Roadmap :nut_and_bolt: 



### Overview
* **Total Estimated Duration: 3 months
* **Full-time equivalent (FTE): 3
* **Total Costs:** 1.5 BTC

### Milestone 1 — Raze Substrate Modules and cross-chain bridge implementation
* **Estimated Duration:** 2 month
* **FTE:**  2 
* **Costs:** 0.75 BTC

The main deliverable of this milestone is Raze substrate pallet that supports: mint, transfer, redeem, lock and unlock modules. The substrate modules will support both the cross-chain payment of ERC-20 tokens and the mainstream tokens issued in the Polkadot ecosystem.   

| Number | Deliverable | Specification |
| ------------- | ------------- | ------------- |
| 0a. | License | Apache 2.0 / MIT / Unlicense |
| 1. | Raze Substrate module for private payment | We will implement the zero-knoweldge proof schemes and create a Substrate module that will incorporate the verification logic for the aforementioned modules. It will support the verification of anonymous minting, transfer, redeem, lock and unlock for any ERC-20 token and mainstream Polkadot tokens such as DOT and KUSAMA. 
| 2. | A cross-chain bridge between Ethereum and Polkadot | The bridge will map ERC-20 token to the Polkadot ecosystem.  
| 3. | Benchmark | Benchmark on the throughput and gas cost of the proposed modules |   
| 4. | Docker | We will provide a dockerfile to demonstrate the usage of our modules |

### Milestone 2 — Raze client implementation and integration 
* **Estimated Duration:** 1 month
* **FTE:**  1
* **Costs:** 0.75 BTC

The main deliverable of this milestone is the client that can generate the transactions that can trigger the aforementioned contracts. 

| Number | Deliverable | Specification |
| ------------- | ------------- | ------------- |
| 0a. | License | Apache 2.0 / MIT / Unlicense |
| 1. | Raze client module | We will implement the Register, CreateMintTx, CreateTransferTx, CreateRedeemTx, lock, and unlock modules. This module will allow the client side to generate the necessary transaction to trigger the corresponding modules. 
| 2. | Privacy-preserving DeFi functionality | We will implement anonymous mining functionality, which allows the users to mint and lock private coins and unlock the private coins after a certain period of time.   
| 3. | Benchmark | Benchmark on the latency and usability of the proposed module |    
| 4. | Docker | We will deploy the client APIs on Kusama or Rococo |


## Future Plans
Please include the team's long-term plans and intentions.

## Additional Information :heavy_plus_sign: 
Any additional information that you think is relevant to this application that hasn't already been included.

Possible additional information to include:
* What work has been done so far?
* Are there are any teams who have already contributed (financially) to the project?
* Have you applied for other grants so far?

