# Raze_Network

Project overview

Raze network is a substrate-based cross-chain privacy-preserving payment protocol for the Polkadot ecosystem.. The Raze network applies zk-snark to the Zether framework to build a second-layer decentralized anonymous payment module. It will be then imported as a substrate-based smart contract. The goal of this contract is to enable the cross-chain privacy-preserving payment scheme for the Polkdot ecosystem.

Similar to the Zether framework, it will have three technical modules: mint, transfer and redeem. The mint module will convert any ERC-20 token into an anonymized version, while the redeem module will convert the anonymized token into its native form. The transfer module is the one that enables the anonymous transfer of the token. It will conceal the transaction amount and protect the anonymity of both sender and receiver. 

The mint module will create a Raze ERC-20 token account by running a mint contract or fund the token directly to an existing Raze account. Each Raze account is identified by a public key. Therefore, the mint module will generate a ciphertext under the account public key which encrypts the amount of minted token. If the account has already exist before the mint operation, the generated ciphertext will be homomorphically added to the current ciphertext to increase the hidden token amount.   

Since the transaction amount of each Raze account is encrypted and therefore hidden, the remaining question for the transfer module is how to hide the identities of both the sender and receiver. Similar to the anonymous transfer module of the Zether scheme, the identities of the involved parties are hidden through "one-out-of-many" proof. Essentially the "one-out-of-many" proof is very close to ring signature, which allows the verification of the fact that the transaction is launched by one of the many parties in the ring while not knowing who among them is the sender or reciever of the transaction, and thus hide their identities. Compared with the Zether's "one-out-of-many" proof scheme, we will apply zk-snark scheme to design and implement this "one-out-of-many" proof and thus significantly reduce both the proof size. It will also help reduce the verification cost, especially when it is amortized. Therefore, our scheme will have a reduced gas cost for the involved users. 

The redeem module converts the anonymized token backe to its original form. The redeem module also needs to invoke a zero-knowledge proof functionality which aims to prove that the user who initiates this module knows the secret key and the redeemed amount is smaller than the encrypted balance of the Raze account. Since range proof is part of the statement and we will also apply zk-snark scheme to reduce the gas cost of this operation. 

Since the account balance of each Raze account will be encrypted under a public key, and hence it will guarantee the transaction amount confidentiality. The "one-out-of-many" proof will hide the sender and receiver identities among a ring of Raze accounts, and hence guarantee the user anonymity. The raze network can be viewed as a pool of boiling water, where each water molecule interacts with each other in a chaotic and vibrant fashion. Whenever a user deposit a certain amount of token through invoking the mint module, the token would be like a water molecule drops into this pool of boiling water, it is no longer traceable.  

![alt text](https://github.com/razenetwork/Raze_Network/blob/main/raze_architecture.png?raw=true)


A raze account is identified by a public key Y. The encrypted balance would be

