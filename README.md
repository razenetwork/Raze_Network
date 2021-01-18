# Raze_Network

> This document is referenced in the terms and conditions and therefore needs to contain all the required information. Don't remove any of the mandatory parts presented in bold letters or as headlines! See the [Open Grants Program Process](https://github.com/w3f/Open-Grants-Program/blob/master/README_2.md) on how to submit a proposal.

* **Project:** Raze Network
* **Proposer:**
* **Payment Address:** 


## Project Overview

Raze network is a native privacy-preserving layer for the Polkadot ecosystem. More specifically, it will provide cross-chain end-to-end payment privacy for the entire DeFi stack of the Polkadot encosystem. The core technical module of the Raze network is a second-layer decentralized anonymous payment module, which will serve as a universal plugin-and-play infrastructure for the Polkadot DeFi ecosystem. This module will be imported as a substrate-based smart contract, which allows the user to hide one's account address and financial information before participating the Polkadot DeFi stacks, be it DEX, liquidity mining, loan, insurance, etc. Our goal is to protect the users' transaction data from the surveilance of big brother. 

![alt text](https://github.com/razenetwork/Raze_Network/blob/main/raze_polkadot.png?raw=true)

### Project Details 

We will apply ZK-SNARK to the Zether framework to build our second-layer decentralized anonymous payment module. It will be then imported as a substrate-based smart contract. The goal of this contract is to enable the cross-chain privacy-preserving payment scheme for the Polkdot ecosystem.

Similar to the Zether framework, it will have three technical modules: mint, transfer and redeem. The mint module will convert any ERC-20 token into an anonymized version, while the redeem module will convert the anonymized token into its native form. The transfer module is the one that enables the anonymous transfer of the token. It will conceal the transaction amount and protect the anonymity of both sender and receiver. 

The mint module will create a Raze ERC-20 token account by running a mint contract and fund the anonymized token to an existing Raze account. Each Raze account is identified by a public key `pk`. Therefore, the mint module will generate a ciphertext under the account public key which encrypts the amount of minted token. If the account has already exist before the mint operation, the generated ciphertext will be homomorphically added to the existing ciphertext to increase the token amount.   

Since the transaction amount of each Raze account is encrypted and therefore hidden, the remaining question for the transfer module is how to hide the identities of both the sender and receiver. Similar to the anonymous transfer module of the Zether scheme, the identities of the involved parties are hidden through "one-out-of-many" proof. The "one-out-of-many" proof is very close to ring signature, which proves that the transaction is launched by one of the many parties in the anonymity set or the ring while not revealing who among them is the sender or reciever of the transaction, and thus hide their identities. The transfer module's zkp will also prove the payment consistency and provide a range proof demonstrating there is no negative amount involved, which could potentially allow the adversary to create money out of thin air. 

The redeem module converts the anonymized token back to its original form. The redeem module also needs to invoke a zero-knowledge proof module which aims to prove that the user initiating the redeem module knows the secret key and the redeemed amount is smaller than the encrypted balance of the Raze account. 

To sum up, we use public key homomorphic encryption to protect the balance and transactional amount condentionality, and invoke the "one-out-of-many" proof to hide the sender and receiver identities, and zero-knowledege proof to guarantee payment consistency. The raze network can be viewed as a pool of boiling water, where each water molecule interacts with each other in a chaotic and vibrant fashion. Whenever a user deposit a certain amount of token through invoking the mint module, the token would be like a water molecule drops into this pool of boiling water, it is no longer traceable.  

To facilitate the DeFi functionality, we have two additional modules: lock and unlock. The lock module would allows an account owner to lock the account, while the unlock module allows the account owner to unlock the account. 

![alt text](https://github.com/razenetwork/Raze_Network/blob/main/raze_architecture.png?raw=true)

Each raze user can register a raze account any time (s)he wishes. The registeration algorithm CreateAddress will generate a secret key and public key. The public key is the identifier of this raze account. 

The client side runs the CreateMintTx algorithm, which takes as input the raze account pk and the amount of native token amount amt as inputs. The output of the CreateMintTx algorithm is a ciphertext encrypted under the public key pk. If raze account pk already has a ciphertext cp, the newly created ciphertext will be homomorphically added to the existing ciphertext to increase the amount of token under the raze account. Otherwise, the new ciphertext will be attached to the raze account. The native token will be stored in the mint contract. 

To invoke the transfer contract, the client side runs the CreateTransferTx algorithm, which takes as inputs the raze account secret key sk, and the amount of token amt, the public keys of both sender, receiver raze account and those of the anonymity set. The output of the CreateTransferTx algorithm is a zero-knowledge proof that the prover knows one of the secret keys of the anonymity set, the consistency of the payment and range proof. equation todo....

The client side runs the CreateBurnTx algorithm to invoke the Fund contract. It takes the account secret key sk, the withdrawal amount aml and the public key pk as input and generates a zero-knowledge proof that the user knows the secret key and the account pk has enough balance for the withdraw operation. The zero-knowledge proof will be used as inputs to invoke the Burn contract. 

The user can invoke the lock module by running the CreateLockTx algorithm on the client side. The client inputs a secret key sk and an Ethereum address addr to generate a zero-knowledge proof to demonstrate the user knows the secret key of the account and he authorizes the frozen the account, which will be locked to the input address addr. Similarly, the user can invoke the unlock module by running a CreateUnlockTx on the client side. The inputs of CreateUnlockTx algorithm are the same to that of the CreateLockTx algorithm. It will generate a similar zero-knowledge proof to unlock the account. Note, the zero-knowledge proof used here certainly can prevent the replay attack. 

### Ecosystem Fit 

Decentralized finance (DeFi) if a nascent ecosystem. Privacy is essential to the prosperity of DeFi. The raze network will serve as a vital infrastructure for the future development of privacy-preserving DeFi in the Polkadot ecosystem. The raze network will not only liberate the Polkadot DeFi ecosystem from the surveilance, but it will also significantly increase the liquidity of the Polkadot ecosystem. The closest product to the Raze network is the Manta network, which is a privacy-preserving DEX system. Our scheme differs from theirs in two regards: 1. our product is much more generic in the sense that it supports any kind of DeFi product in the Polkadot system, and it also supports anonymity mining, which in itself already a vital DeFi product. In constrast, the Manta network focuses on the private DEX system. 2. From the technical perspective, the underlying zero-knowledge proof scheme for the Manta network requires trusted setup, which is why they require a trust ceremony. In contrast, the zero-knowledge proof scheme we adopt does not require any trusted setup, which is much more in accordance with the decentralization ethos. 

## Development Roadmap
* 2020 Q3 - Project Establishment
* 2020 Q4 - Draft of Whitepaper 
* 2021 Q1 - Official Whitepaper Publish
* 2021 Q1 - Official Website Launch 
* 2021 Q1 - Core and zkSNARKS Implementation Release
* 2021 Q2 - Token and Dapp source code Release
* 2021 Q3 - Protocol and Product Launch
* 2021 Q3 - Liquidity Reward Program
* 2021 Q4 - Implementing the Bridge to other DeFi
* 2021 Q4 - Integrating More Customized Functions


### Overview

Please provide the following:
  * A brief description of the project.
  * An indication of how you will integrate this project into Substrate / Polkadot / Kusama.
  * An indication of why your team is interested in creating this project.

### Project Details 
We expect the teams to already have a solid idea about the project's expected final state.

Therefore, we ask the teams to submit (where relevant):
* Mockups/designs of any UI components
* API specifications of the core functionality
* An overview of the technology stack to be used
* Documentation of core components, protocols, architecture etc. to be deployed
* PoC/MVP or other relevant prior work or research on the topic

### Ecosystem Fit 
Are there any other projects similar to yours? If so, how is your project different?

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


Below we provide an **example roadmap**. In the descriptions it should be clear how the project is related to Substrate and/or Polkadot. We recommend that the scope of the work can fit within a 3 month period and that teams structure their roadmap as 1 month = 1 milestone. 

For each milestone:
* Please be sure to include a specification of your software. Treat it as a contract - the level of detail must be enough to later verify that the software meets the specification.
To assist you in defining it, we created a document with examples for some grant categories [here](../src/grant_guidelines_per_category.md).
* Please include total amount of funding requested per milestone.
* Please note that we require documentation (e.g. tutorials, API specifications, architecture details) in each milestone. This ensures that the code can be widely used by the community.
* Please provide a test suite, comprising unit and integration tests, along with a guide on how to run these.
* Please commit to providing a dockerfiles for the delivery of your project. 
* Please indicate the milestone duration, as well as number of Full-Time Employees working on each milestone, and include the number of days along with their cost per day.
* Deliverables 0a-0d are mandatory and should not be removed, unless you explicitly specify a reason within the PR's `Additional Notes` section (e.g. Milestone X is research oriented and as such there is no code to test)

### Overview
* **Total Estimated Duration:** Duration of the whole project (e.g. 2 months)
* **Full-time equivalent (FTE):**  1
* **Total Costs:** 1 BTC

### Milestone 1 Example — Implement Substrate Modules 
* **Estimated Duration:** 1 month
* **FTE:**  1
* **Costs:** 0.75 BTC

| Number | Deliverable | Specification |
| ------------- | ------------- | ------------- |
| 0a. | License | Apache 2.0 / MIT / Unlicense |
| 1. | Raze Substrate module for private payment | We will implement the zero-knoweldge proof scheme we adopt for the Raze network We will create a Substrate module that will incorporate the verification logic for mint, transfer and redeem module. It will support the verification of anonymous minting, transfer and redeem for Dot and any ERC-20 token. 
| 2. | Benchmark | Benchmark on the latency of the proposed module |  
| 3. | Substrate module: Z | We will create a Substrate module that will... |  
| 4. | Substrate chain | Modules X, Y & Z of our custom chain will interact in such a way... (Please describe the deliverable here as detailed as possible) |  
| 5. | Docker | We will provide a dockerfile to demonstrate the usage of our modules |

### Milestone 1 Example — Implement Substrate Modules 
* **Estimated Duration:** 1 month
* **FTE:**  1
* **Costs:** 0.75 BTC

| Number | Deliverable | Specification |
| ------------- | ------------- | ------------- |
| 0a. | License | Apache 2.0 / MIT / Unlicense |
| 1. | Raze client module | We will implement the Register, CreateMintTx, CreateTransferTx, CreateRedeemTx, lock, and unlock modules. This module will allow the client side to generate the necessary transaction to trigger the corresponding modules. 
| 2. | Privacy-preserving DeFi functionality | We will implement anonymous mining functionality, which allows the users to mint and lock private coins and unlock the private coins after a certain period of time.  
| 2. | Benchmark | Benchmark on the usability of the proposed module |  
| 3. | Substrate module: Z | We will create a Substrate module that will... |  
| 4. | Substrate chain | Modules X, Y & Z of our custom chain will interact in such a way... (Please describe the deliverable here as detailed as possible) |  
| 5. | Docker | We will provide a dockerfile to demonstrate the end-to-end usage of our modules |

### Milestone 2 Example — Additional features
...

## Future Plans
Please include the team's long-term plans and intentions.

## Additional Information :heavy_plus_sign: 
Any additional information that you think is relevant to this application that hasn't already been included.

Possible additional information to include:
* What work has been done so far?
* Are there are any teams who have already contributed (financially) to the project?
* Have you applied for other grants so far?

