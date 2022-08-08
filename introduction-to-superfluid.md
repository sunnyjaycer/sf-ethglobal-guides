---
id: 3cbd3
slug: introduction-to-superfluid
description: Intro to superfluid protocol
---


<Section name="1. Introduction" description="Introduction to superfluid" hidden="false">

## What is Superfluid

Planning and preparing programmable cashflows are about to get easier and better with the development of single transaction smart contracts that trigger multiple recurrent processes that will ensure the exchange of tokens between two wallets take place.

So, what exactly is programmable cashflow? This is simply recurrent spending. That’s being able to send payments that occur at a specific time repeatedly. For example, payment of salaries is a recurrent spend for organizations. In web3, this is most likely contributors’ payout for work done repeatedly over a period of time.

Superfluid has set out to build the infrastructure that would allow programmable cashflow to run on the blockchain (Ethereum). There are a number of things that make Superfluid unique. They include:

Gasless transfers: This is a case where once you start the process of sending cashflow repeatedly, otherwise known as opening a stream, you don’t get to pay gas fees each time. Once is enough, and that is truly exciting.
No lock-up capital: You do not have to keep capital locked down to be able to process these payments. They can be provided just in time to keep it flowing.
On-chain: The protocol lives and operates on EVM networks, ensuring fast processing and withdrawal.


<Quiz id={"ind7t"} />

</Section>

<Section name="2. How it works" description="Understanding the protocol">

## How it works

Sending tokens (cashflow) from one wallet to another is done through a process called streaming. Streams are powered by agreements (this represents an exchange of tokens between two wallets that occur over time). These agreements are predefined on the smart contracts, and they are used to ensure that what is sent is what has been approved in the transaction.

One of such agreements is the Constant Flow Agreement (CFA) which enables the transfer of tokens from one wallet to the other without gas fees. In this agreement, two wallets can be connected for the purpose of sending tokens between them at no extra cost. To get started, you have to convert your tokens to supertokens, then define three parameters, sender, receiver, and flow rate. The flow rate is the amount of tokens to be sent per second.

Each stream is powered by Ethereum Virtual Machine (EVM) based smart contracts and is usually triggered via the following process:

1. The transaction is opened on the blockchain after setting some predefined agreements
2. A stream is opened as a result
3. Tokens are transferred continuously depending on the flow rate
4. Ensure you balances don’t get to zero

To send your first stream, check this [dashboard](http://app.superfluid.finance/), connect your wallet (Metamask, Wallet Connect, Coinbase Wallet, or Portis), and follow the tour on the platform. Developers can as well build on Superfluid, and they can get started by viewing the [Superfluid Documentation](https://docs.superfluid.finance/).

<Quiz id={"ddhvd"} />

<Quiz id={"vxz3f"} />


</Section>

<Section name="3. Bug Bounty" description="Introduction to protocol security">

## Bug Bounty

The Superfluid team came up with a bug bounty to secure the protocol by looking out for risk parameters within the code. The bug bounty falls within the scope highlighted below:

- Superfluid framework bugs
- Bugs in CFA/IDA in general
    - Anything that would avoid streams from being closed
    - Anything that would result in the sum of all account balances drifting significantly from the total supply
- Theft of tokens in third party wrapper contracts
- Other unexpected behaviour in any of their token contracts

The reward for the bug is based on priority risks and the level of impact that such risk would cause. The table below represents the threat levels, impact, and associated rewards.

## Threat Levels

### Critical

- **Reward**
  - $30,000 - $200,000
- **Impact**
  - A reproducible vulnerability discovered in the core protocol that can be exploited by anyone that results in the loss of a portion or all user funds in the Superfluid protocol:
    - Critical bug which results in the drainage of all user funds.
    - Critical bug which results in the drainage of a portion of user funds.


### Critical

- **Reward**
  - $30,000 - $200,000
- **Impact**
  - A reproducible vulnerability discovered in the core protocol that can be exploited by anyone that results in the loss of a portion or all user funds in the Superfluid protocol:
    - Critical bug which results in the drainage of all user funds.
    - Critical bug which results in the drainage of a portion of user funds.


### High

- **Reward**
  - $10,000 - $30,000
- **Impact**
  - A reproducible bug by anyone that results in the core protocol not functioning properly where funds are stuck.	


### Medium

- **Reward**
  - $5,000
- **Impact**
  - An unknown vulnerability which results in any of the Superfluid contracts consuming unbounded gas for any operation.	

### Low

- **Reward**
  - $2,000
- **Impact**
  - A vulnerability that causes any of the contracts to behave in an unintended manner (which leads to a negative user experience) without resulting in loss of user funds or user funds being stuck.	


<Quiz id={"6hyba"} />

</Section>

<Section name="Next Steps" description="">

**To connect with Superfluid, kindly reach out via the links below:**

There are a lot of services using libp2p at the moment, for applications including file storage, video streaming, crypto wallets, dev tools, and blockchains. Here we list some examples, but the list continues to grow as you read this.

![https://workable-application-form.s3.amazonaws.com/advanced/production/60ddbb1bfd6acf0380bd5d19/52995417-ccb1-4797-9301-87cfedec2c27](https://workable-application-form.s3.amazonaws.com/advanced/production/60ddbb1bfd6acf0380bd5d19/52995417-ccb1-4797-9301-87cfedec2c27)

IPFS uses libp2p extensively, so all other services depending on IPFS indirectly use libp2p as well.

# Resources
f 
Ready to learn more? There are plenty of additional resources to explore, both in ProtoSchool and beyond. Here are some of our favorites:

- [**Discord**](https://discord.gg/XsK7nahanQ)
- [**Twitter**](https://twitter.com/intent/follow?screen_name=Superfluid_HQ)
- [**Linkedin**](https://www.linkedin.com/company/37856773/)
- [**GitHub**](https://github.com/superfluid-finance)


</Section>


