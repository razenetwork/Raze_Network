# Raze_Network

> This document is referenced in the terms and conditions and therefore needs to contain all the required information. Don't remove any of the mandatory parts presented in bold letters or as headlines! See the [Open Grants Program Process](https://github.com/w3f/Open-Grants-Program/blob/master/README_2.md) on how to submit a proposal.

* **Project:** Raze Network
* **Proposer:**
* **Payment Address:** 


## Project Overview

Raze network is a native privacy-preserving layer for the Polkadot ecosystem. More specifically, it will provide a cross-chain end-to-end privacy for the entire DeFi stack of the Polkadot encosystem. The core technical module of the Raze network is a second-layer decentralized anonymous payment module, which guarantees the payment privacy of the parties involved in the Polkadot ecosystem. We will provide a substrate-based smart contract, which allows the user hide its account address and balance before participating the Polkadot DeFi stacks, be it DEX, liquidity mining, loan, insurance, etc. Our goal is to protect the users' transaction data from the surveilance of big brother. 

![alt text](https://github.com/razenetwork/Raze_Network/blob/main/raze_polkadot.png?raw=true)

### Project Details 

We will apply ZK-SNARK to the Zether framework to build our second-layer decentralized anonymous payment module. It will be then imported as a substrate-based smart contract. The goal of this contract is to enable the cross-chain privacy-preserving payment scheme for the Polkdot ecosystem.

Similar to the Zether framework, it will have three technical modules: mint, transfer and redeem. The mint module will convert any ERC-20 token into an anonymized version, while the redeem module will convert the anonymized token into its native form. The transfer module is the one that enables the anonymous transfer of the token. It will conceal the transaction amount and protect the anonymity of both sender and receiver. 

The mint module will create a Raze ERC-20 token account by running a mint contract or fund the token directly to an existing Raze account. Each Raze account is identified by a public key. Therefore, the mint module will generate a ciphertext under the account public key which encrypts the amount of minted token. If the account has already exist before the mint operation, the generated ciphertext will be homomorphically added to the current ciphertext to increase the hidden token amount.   

Since the transaction amount of each Raze account is encrypted and therefore hidden, the remaining question for the transfer module is how to hide the identities of both the sender and receiver. Similar to the anonymous transfer module of the Zether scheme, the identities of the involved parties are hidden through "one-out-of-many" proof. Essentially the "one-out-of-many" proof is very close to ring signature, which allows the verification of the fact that the transaction is launched by one of the many parties in the ring while not knowing who among them is the sender or reciever of the transaction, and thus hide their identities. Compared with the Zether's "one-out-of-many" proof scheme, we will apply zk-snark scheme to design and implement this "one-out-of-many" proof and thus significantly reduce both the proof size. It will also help reduce the verification cost, especially when it is amortized. Therefore, our scheme will have a reduced gas cost for the involved users. 

The redeem module converts the anonymized token backe to its original form. The redeem module also needs to invoke a zero-knowledge proof functionality which aims to prove that the user who initiates this module knows the secret key and the redeemed amount is smaller than the encrypted balance of the Raze account. Since range proof is part of the statement and we will also apply zk-snark scheme to reduce the gas cost of this operation. 

Since the account balance of each Raze account will be encrypted under a public key, and hence it will guarantee the transaction amount confidentiality. The "one-out-of-many" proof will hide the sender and receiver identities among a ring of Raze accounts, and hence guarantee the user anonymity. The raze network can be viewed as a pool of boiling water, where each water molecule interacts with each other in a chaotic and vibrant fashion. Whenever a user deposit a certain amount of token through invoking the mint module, the token would be like a water molecule drops into this pool of boiling water, it is no longer traceable.  

![alt text](https://github.com/razenetwork/Raze_Network/blob/main/raze_architecture.png?raw=true)

Each raze user can register a raze account any time (s)he wishes. The registeration algorithm CreateAddress will generate a secret key and public key. The public key is the identifier of this raze account. 

The client side runs the CreateMintTx algorithm, which takes as input the raze account pk and the amount of native token amount amt as inputs. The output of the CreateMintTx algorithm is a ciphertext encrypted under the public key pk. If raze account pk already has a ciphertext cp, the newly created ciphertext will be homomorphically added to the existing ciphertext to increase the amount of token under the raze account. Otherwise, the new ciphertext will be attached to the raze account. The native token will be stored in the mint contract. 

To invoke the transfer contract, the client side runs the CreateTransferTx algorithm, which takes as inputs the raze account secret key sk, and the amount of token amt, the public keys of both sender, receiver raze account and those of the anonymity set. The output of the CreateTransferTx algorithm is a zero-knowledge proof that the prover knows one of the secret keys of the anonymity set, the consistency of the payment and range proof. equation todo....


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

This section should break out the development roadmap into a number of milestones. Since the milestones will appear in the grant contract, it helps to describe the functionality we should expect, plus how we can check that such functionality exists in the product. Whenever milestones are delivered, we refer to the contract to ensure that everything has been delivered as expected.

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
* **Full-time equivalent (FTE):**  Workload of an employed person ([see](https://en.wikipedia.org/wiki/Full-time_equivalent)) (e.g. 2 FTE)
* **Total Costs:** Amount of Payment in BTC or DAI for the whole project. The total amount of funding needs to be below $30k for initial grants and $100k for follow-up grants at the time of submission. (e.g. 0.80 BTC)

### Milestone 1 Example — Implement Substrate Modules 
* **Estimated Duration:** 1 month
* **FTE:**  1
* **Costs:** 0.75 BTC

| Number | Deliverable | Specification |
| ------------- | ------------- | ------------- |
| 0a. | License | Apache 2.0 / MIT / Unlicense |
| 0b. | Documentation | We will provide both inline documentation of the code and a basic tutorial that explains how a user can (for example) spin up one of our Substrate nodes. Once the node is up, it will be possible to send test transactions that will show how the new functionality works. |
| 0c. | Testing Guide | The code will have unit-test coverage (min. 70%) to ensure functionality and robustness. In the guide we will describe how to run these tests | 
| 0d. | Article/Tutorial | We will write an article or tutorial that explains the work done as part of the grant. 
| 1. | Substrate module: X | We will create a Substrate module that will... (Please list the functionality that will be coded for the first milestone) |  
| 2. | Substrate module: Y | We will create a Substrate module that will... |  
| 3. | Substrate module: Z | We will create a Substrate module that will... |  
| 4. | Substrate chain | Modules X, Y & Z of our custom chain will interact in such a way... (Please describe the deliverable here as detailed as possible) |  
| 5. | Docker | We will provide a dockerfile to demonstrate the full functionality of our chain |

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

