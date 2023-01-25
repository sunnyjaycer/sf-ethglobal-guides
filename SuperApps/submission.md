---
id: 3cbd3
slug: introduction-to-superfluid
description: Intro to superfluid protocol
---


<Section name="1. Prerequisites" description="Prerequisites for this tutorial">

## Prerequisites

This guide assumes you've already completed the [**Money Streaming 101 Guide**](https://ethglobal.com/guides/introduction-to-superfluid-protocol-be10i) which overviews Superfluid and shows you the bare basics of whipping up money streams in Solidity.

Here, we'll going deeper in Superfluid with Solidity. Let's get into it!

</Section>

<Section name="2. What's a Super App" description="Introduction to Super Apps">

## What is a Super App

Super Apps are smart contracts that are registered with the Superfluid Protocol allowing them to **react** to money streams. This reactivity is programmed in **callbacks**.

Callbacks are custom code that a developer can implement in a Super App that triggers when a stream to the Super App is created, updated, or deleted. This code could enact anything from minting an NFT to even initiating a new stream - it's super flexible.

That's all there is to it!

## Why Are Super Apps Needed?

Since Super Apps can react to money streams, they create an intermediate layer of programmability to Super Agreement's that wouldn't be possible with just wallet-to-wallet action.

As a result, applications can be made that mesh together custom logic with Super Agreements actions to create scalable dApps with innovative user experiences.

For instance, you could create a lending Super App where loan repayment is be done via money stream rather than manual and repetitive repayment transactions. Also, imagine a Super App supporting on-chain subscriptions paid through money streams with a built-in affiliate program that automatically redirects portions of subscription streams to referrers. The possibilities with Super Apps are endless!

## Question

<Quiz id={"dhewf"} />

What is the term for the custom code in Super Apps which triggers in reaction to stream actions?
- Hooks
- Callbacks
- Super Agreement
- Customization layer

<Quiz id={"edk9f"} />

Which of the following functionalities requires usage of a Super App? (Hint: Super Apps are all about reactivity!)
- A smart contract that can create multiple streams to various accounts
- A smart contract that can receive a stream
- A smart contract that increments a variable in response to a stream being opened to it
- A smart contract that lets an account mint an NFT if it has an ongoing stream open to it

</Section>

<Section name="3. Anatomy of a Super App" description="Anatomy of a Super App">

To show how to set up Super Apps, we're going to build a simple "BUIDLxDispenser" Super App and deploy it on Goerli

All it will do is react to a stream of fDAIx by sending the sender a stream of BUIDLx tokens. If the sender stops streaming fDAIx to it, the BUIDLxDispenser will cancel the their stream of BUIDLx tokens.

First, click on this [**REMIX IDE**](https://remix.ethereum.org/?#gist=f94b782fd4d5b472de714cbfc59832f2&version=soljson-v0.8.13+commit.abaa5c0e.js&optimize=false&runs=200&evmVersion=null) link to find the BUIDLxDispenser contract. Note that the IDE will open all the environment folders at once; you can close them such that the file directory looks like this:

![retracted-directory](./assets/retracted-directory.png)

## Set Up

First, we make these imports which are needed for setting up the contract to interact with the Superfluid Protocol:

```
import {ISuperfluid, ISuperToken, SuperAppDefinitions} from "@superfluid-finance/ethereum-contracts/contracts/interfaces/superfluid/ISuperfluid.sol";

import {SuperTokenV1Library} from "@superfluid-finance/ethereum-contracts/contracts/apps/SuperTokenV1Library.sol";

import {IConstantFlowAgreementV1} from "@superfluid-finance/ethereum-contracts/contracts/interfaces/agreements/IConstantFlowAgreementV1.sol";

import {SuperAppBase} from "@superfluid-finance/ethereum-contracts/contracts/apps/SuperAppBase.sol";
```

We need the BUIDLxDispenser to inherit `SuperAppBase` which lays out callback functions for our Super App that we can override to implement our desired functionality.

## Constructor

For this contract, we're going to store the fDAIx Super Token that will be streamed to the contract and BUIDLx Super Token that will be streamed into the contract.

We also want to store the Superfluid Host contract and Constant Flow Agreement.

```
    constructor(
        ISuperToken _fDAIx,
        ISuperToken _BUIDLx,
        ISuperfluid _host,
        IConstantFlowAgreementV1 _cfa
    ) {

        fDAIx = _fDAIx;
        BUIDLx = _BUIDLx;
        host = _host;
        cfa = _cfa;
```

In order for our smart contract to get it's reactive abilities, it needs to be registered with the host (you can read more about the registration mechanism [here](https://docs.superfluid.finance/superfluid/protocol-overview/in-depth-overview/super-apps#registering-with-the-protocol)).

These definitions (like `BEFORE_AGREEMENT_CREATED_NOOP`) configure how this Super App will be reacting to streams. We'll come back to touch on this and if you'd like to read deeper, see [here](https://docs.superfluid.finance/superfluid/developers/super-apps/super-app#super-app-configuration).

```
    // register super app with host
    host.registerApp(
        SuperAppDefinitions.APP_LEVEL_FINAL |
        SuperAppDefinitions.BEFORE_AGREEMENT_CREATED_NOOP |
        SuperAppDefinitions.BEFORE_AGREEMENT_UPDATED_NOOP |
        SuperAppDefinitions.BEFORE_AGREEMENT_TERMINATED_NOOP |
        SuperAppDefinitions.AFTER_AGREEMENT_UPDATED_NOOP
    );
```

This code here is to make our lives easier by minting the BUIDLxDispenser contract 10 million BUIDL tokens and wraps it into BUIDLx.

BUIDLx token is a [wrapper Super Token](https://docs.superfluid.finance/superfluid/protocol-overview/in-depth-overview/super-tokens#wrapper). Its underlying token is BUIDL. 

BUIDL token has a public mint function which lets anyone mint their own BUIDL whenever they want. Makes life easy for this tutorial!

```
    // Get the underlying BUIDL ERC20 token from the BUIDLx Super Token 
    IBUIDL buidlToken = IBUIDL( BUIDLx.getUnderlyingToken() );

    // Minting the contract 1,000,000 BUIDL tokens to start off
    buidlToken.mint(address(this), 10_000_000e18);

    // Approving the BUIDL tokens to be wrapped into BUILDx
    buidlToken.approve(address(BUIDLx), 10_000_000e18);

    // Wrapping the BUIDL into BUIDLx - now 
    BUIDLx.upgrade(10_000_000e18);
```

## Callbacks

Let's skip down to the last two functions in this contract: `afterAgreementCreated` and `afterAgreementTerminated`. This external functions are our callbacks!

If I start a stream to the BUIDLxDispenser contract, `afterAgreementCreated` is triggered and if I delete the stream `afterAgreementTerminated` is triggered. 

Let's break down `afterAgreementCreated` starting with the parameters 👇

```
    function afterAgreementCreated(
        ISuperToken _superToken,
        address _agreementClass,
        bytes32, //_agreementId
        bytes calldata _agreementData,
        bytes calldata, //_cbdata
        bytes calldata _ctx
    )
```

This looks like a lot, but it's quite simple! 
- `_superToken`: the Super Token that's being streamed to the contract
- `_agreementClass`: Since Superfluid has both the Constant Flow Agreement for money streaming and the Instant Distribution Agreement for instant distributions, this specifies what agreement is engaging the Super App. This is pretty much always the Constant Flow Agreement, but we want to ensure for security.
- `_agreementData`: encoded data on the sender and receiver of the stream
- `_ctx`: You can think of this as the initial "Superfluid state" coming into the callback (it's context!). If you do Superfluid actions in your callback (like create streams), you'll want to incorporate these changes into an updated context and return it at the end of the callback so the Superfluid Protocol understands what's new. This will become clearer below.

The rest of the callback header looks like this:
```
    external
    override
    onlyExpected(_superToken, _agreementClass)
    onlyHost
    returns (bytes memory newCtx)
```
The `onlyExpected` modifier makes sure that we're only reacting to a stream of fDAIx and the Constant Flow Agreement. Any other engagement should revert.

The `onlyHost` modifier makes sure that it's only the Host that can call this callback. You can read more on this [here](https://docs.superfluid.finance/superfluid/protocol-overview/in-depth-overview/super-apps#registering-with-the-protocol).

The function returns `newCtx`

Let's look at the callback body now:

```
    newCtx = _ctx;

    // Get sender
    (address sender,) = abi.decode(_agreementData, (address, address));

    // Create stream of BUIDLx to them
    newCtx = BUIDLx.createFlowWithCtx(sender, TEN_TOKENS_PER_MONTH, _ctx);
    
    return newCtx;
```

We 