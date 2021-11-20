---
eip: <to be assigned>
title: Wrapped Metaverse Tokens
author: Hyungsuk Kang (@hskang9)
discussions-to: <URL>
status: Draft
type: Standards Track
category (*only required for Standard Track): ERC
created: 2021-11-20
---

## Simple Summary
Defines a standard interface for viewing the addresses and balances of wrapped metaverse tokens.

## Abstract

Metaverses are now maturing in ethereum ecosystem, and the standard of the tokens in each metaverse are all aggregated into ERC1155 for efficiency.
Invented by Witek Radomski from Enjin, this has made many metaverse/game project to accomodate their assets to provide ownerships to inhabitants of their fantasy or real so-called world.
However, current ERC1155 has been sacrificing its compatibility to current defi ecosystem due to different data structure. A certain contract to bridge them into prepared erc20 could be possible, but this just can create duplicate datas in the global state. Due to this, I propose the standard wrapped metaverse token contract to wrap metaverse assets into the most widely used ERC20 tokens. 

This EIP introduces a simple interface for exposing the addresses of all wrapped ERC1155 tokens in metaverses, as well as querying wrapped assets' balances.

## Motivation
Many metaverse or NFT projects are choosing ERC1155 for efficiently generating their assets. Choosing ERC1155 for managing non-fungible tokens or other tokens in the game has brought memory efficiencies and less contract address for systems to operate. However, this has brought incompatibility between other existing defi infrastructure(e.g. Dexes, Lending, etc). When a user wants to liquidate their game assets or metaverse assets, he or she needs to change it to intermediary assets to acquire it then change it to ETH then buy other assets in different metaverse. This is very inefficient for interoperability between connecting ecosystems between metaverses as well due to gas fees required to convert one asset with another. Just as Wrapped ether has been invented, I propose to have wrapped ERC1155 asset that can be both backward compatible to existing decentralized finance infrastructure and free trade between metaverses.

### Example

For example, if a user deposits tokens in a Uniswap pool, they will receive liquidity provider (LP) tokens. The user’s balance of these LP tokens does not provide information about the pooled tokens, so the user can not know how many funds are held in the pool without visiting the Uniswap dapp.

If Uniswap implemented this EIP, then wallets could look up the wrapped tokens and display them as part of their UI. They could also query the market price data for the wrapped tokens, allowing them to calculate the market value of the LP token.

## Specification
Using `create2`, Wrapped metaverse tokens must be generated from creating wrapped ERC20 from the owner of ERC1155.
Once wrapped erc20 of metaverse assets are created, people can deposit ERC1155 to the contract then get the same kind and amount of designated ERC20 tokens.
Deposited ERC1155 tokens can be withdrawn from the contract by depositing designated wrapped ERC20 token. 

Current implementation exists in this [repo]().

## Rationale
The specification designates a corresponding wrapped ERC20 token assigned to the ERC1155 token contract.
Wrapped ERC20 tokens are identified via their assetId, ERC1155 address, and data(for receiver).
ERC 1155 does not require any change.

### Introspection

Contracts implementing this standard should make their interface available, such as `IMeta` for unwrapping and wrapping.

## Backwards Compatibility

This EIP is backwards compatible with all existing contracts, as it only provides new view functions. Wrapped ERC20 in this specifcation adds data for receiver, assetId/ERC1155 address to identify itself, but it fully manages to support other ERC20 functionalities.

## Implementation

Implementation is currently made in this [repo]().

Standards of fragmentizing non-fungible-tokens could be added upon wrapping or come up with another standard.

## Security Considerations

There may be security concerns regarding create2 as it has not yet been tested for this use case.
Other than that, this EIP does not introduce any significant security concerns, as it simply standardizes the access to information that is already available in many contracts.

It should be noted that this EIP does not enforce any guarantees about the validity of the information provided. A token could easily report inaccurate balance data, potentially misleading or scamming users.

## Copyright
Copyright and related rights waived via [CC0](https://creativecommons.org/publicdomain/zero/1.0/).