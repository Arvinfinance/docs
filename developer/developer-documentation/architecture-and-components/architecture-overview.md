# Architecture Overview

Arvin as a protocol is a very modular set of smart contracts deployed on public blockchains allowing users to deposit collateral, earn yield on it and borrow IN, a USD-pegged stablecoin. It is built on top of BentoBox and KASHI technology, an infrastructure created by BoringCrypto and adapted by our team. The two core contracts enabling the magic behind Arvin are the DegenBox (our version of the BentoBox) and Cauldrons (the actual isolated markets)

## Core Contracts.

### DegenBox.

DegenBox is our version of the BentoBox. It is a smart contract vault handling user funds, their transfer and strategies. It's where the funds will be stored, in a non-custodial way, on behalf of the users and smart contracts. When using Arvin, the DegenBox will store the collateral deposited, enable strategies to earn interest on it, and store the IN until borrowed by the user. The DegenBox uses the concept of 'Share' to represent amounts of token. Because tokens can vary in amounts (because yield strategies are earning interest on them for example), using shares of the total pool of tokens allow the management of yield for the users. It is similar to other vaults like yearn shares.

### Cauldrons.

The Cauldrons are isolated markets. They are based on KASHI technology and handle all the computation related to the borrowing of IN, the solvency of the users and their liquidation. One Cauldron is deployed per collateral accepted on Arvin. Each instance of a Cauldron is a clone of a MasterContract, following the minimal proxy standard. It has parameters such as fees, fee recipient and Loan to Value, controlled by the MasterContract. To make IN available for users to borrow, the IN Multisig mints IN and deposits it into the Cauldron's account within DegenBox. While they can appear as circulating, until borrowed by users those IN can't move, they are out of circulation. The Cauldron is the entry-point for the user, they interact with it to deposit collateral, leverage it or borrow IN. It is also the entry-point for liquidators, interacting with it to check the solvency of a user and liquidate the position if needed. Cauldrons have the ability to bundle several "Actions" (deposit tokens, add it as collateral, borrow, leverage…) into one transaction, saving our users time and gas while supporting complex sequences of operations atomically, like leverage.

### Oracles.

Oracles are key to Arvin's stability. To keep IN backed by collateral, the smart contracts need to know at all times the price, in USD (as IN is pegged to USD) of each collateral. Therefore, for every market, we have an oracle pricing the relevant collateral in USD. It is connected to the Cauldron itself. For the vast majority of the markets, ChainLink oracles are used, either directly (when the collateral is 'naked', like ETH for example) or indirectly (when the collateral is wrapped, like yearn vaults for example). To allow for upgradability, and because you can't change the oracle of a Cauldron, we sometimes use Proxy Oracles, that point to a 'real' Oracle. The oracle the proxy points to can be changed by the Owner of the Proxy Oracle.

### Strategies.

Strategies enable the deployments of funds sitting in the DegenBox into various yielding strategies, enabling the user to earn interests on the collateral deposited while borrowing. Strategies are connected to the DegenBox itself and allow for the deployment of some percentage of the funds (known as the targetPercent) into a strategy. Being connected to the DegenBox, the strategy is based on tokens, not cauldrons. While multiple cauldrons can exist with the same collateral, one strategy will manage one token. When earning a profit, the Strategy increases the value of a Share of the token, distributing it equally across all deposits.

## Periphery Contracts.

While basic functionalities of Arvin can be achieved with the core contracts, the periphery contracts allow more advanced functionalities, like leveraging, handling some specific collateral types, fee distribution, and Cauldron parameters management.

### Swappers.

Swappers are special contracts allowing the swap of one token for another one. In Arvin's case, they are used to enable leveraging and deleveraging in an atomic way (as one of the 'Actions' of the transaction to the Cauldron). They can be 'simple', using a pre-defined hard coded route, or more advanced, using aggregators like the 0x protocol. They are not connected directly to either DegenBox or Cauldron, but is added as one of the actions when sending a transaction to the cauldron.

### Wrappers.

Because some token have specific non-standard functionalities, like claimable rewards, or rebasing tokens, they can't be used directly within Arvin while maintaining their functionalities. For that reason, we use Wrappers on some collateral types to adapt Arvin to those types of collateral. (some examples of those are Convex LP positions or GLP positons) The use of wrappers mean that from the Cauldron's perspective, the collateral is the wrapped token. From a user-flow perspective, it means that the user has to wrap their tokens to use them, either seamlessly via an action when they call the Cauldron or via a separate transaction before interacting with the cauldron.

### CauldronOwner.

The CauldronOwner is another contract the user does not interact with. It is the Owner of all the masterContracts, and concentrates them all under one contract, allowing authorized 'operators' to perform actions on the masterContract, like change interest rate, borrow limit, deprecate cauldrons reduce the IN available in a market…
