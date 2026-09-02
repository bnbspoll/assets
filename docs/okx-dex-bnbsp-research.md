# BNBSP — OKX DEX / Onchain OS Integration Research

Date: 2026-09-02

## Token

- Name: Binance Self-Pool
- Symbol: BNBSP
- Network: BNB Smart Chain
- Chain index: 56
- Decimals: 18
- Contract: `0x7BfBb2466fEF7a76aB57454Ae6d808CE685514DB`
- Website: https://bnbspool.xyz/
- Primary pair: BNBSP/WBNB on PancakeSwap V2

## Verified OKX support

OKX Onchain OS currently lists BNB Smart Chain Mainnet as a supported DEX API chain with chainIndex `56`.

Official documentation:
- Supported chains: https://web3.okx.com/id/onchainos/dev-docs-v5/dex-api/dex-supported-chain
- Get Quotes: https://web3.okx.com/ar/onchainos/dev-docs-v5/dex-api/dex-get-quote
- Get Tokens: https://web3.okx.com/nb/onchainos/dev-docs-v5/dex-api/dex-get-tokens
- Token Search: https://web3.okx.com/de/onchainos/dev-docs-v5/dex-api/dex-market-token-search
- Token Basic Information: https://web3.okx.com/nb/onchainos/dev-docs-v5/dex-api/dex-market-token-basic-info
- Liquidity Sources: https://web3.okx.com/id/onchainos/dev-docs-v5/dex-api/dex-get-liquidity
- EVM swap integration: https://web3.okx.com/id/onchainos/dev-docs/trade/dex-use-swap-quick-start

OKX documents the EVM native-token address as:
`0xEeeeeEeeeEeEeeEeEeEeeEEEeeeeEeeeeeeeEEeE`

OKX also documents PancakeSwap as a supported liquidity source in its DEX liquidity-source catalog.

## Indexing / quote test status

### Token recognition

Not independently confirmed in this run. OKX's official Token Search API supports exact lookup by contract address, and its Token Basic Information API returns token name, symbol, decimals and logo URL when the token is indexed. Both endpoints require authenticated OKX API headers.

### Quote tests

Not executed from this repository because OKX DEX quote endpoints require authenticated API credentials. No API key, secret key, passphrase, or project ID is stored or requested in this repository.

Required read-only tests once credentials are available:

1. BNB -> BNBSP
   - chainIndex: `56`
   - fromTokenAddress: `0xEeeeeEeeeEeEeeEeEeEeeEEEeeeeEeeeeeeeEEeE`
   - toTokenAddress: `0x7BfBb2466fEF7a76aB57454Ae6d808CE685514DB`
2. WBNB -> BNBSP
3. BNBSP -> WBNB

Use a small exact-in amount in base units. Do not execute `/swap` or broadcast any transaction during this test.

A successful quote should be recorded with:
- `code = 0`
- token metadata
- route / `dexRouterList`
- DEX protocol name
- output amount
- price impact / route details where returned

### Route status

BNBSP-specific routing through PancakeSwap is **not confirmed yet**. PancakeSwap is an OKX-supported liquidity source generally, but this does not prove that the BNBSP/WBNB pool is currently selected by OKX's aggregator.

## Logo / metadata submission

An official OKX-owned GitHub repository named `okx/xlayer-tokenlist` exists, but it is explicitly an X Layer token list and requires `chainId: 196`. It is **not appropriate for BNBSP on BSC** and must not receive a BSC BNBSP PR.

Official repository:
https://github.com/okx/xlayer-tokenlist

Its README states that the list is for X Layer Mainnet and the chain ID is 196.

The official OKX repositories searched also include `okx/dex-api-library` and `okx/okx-dex-sdk`, which are API/SDK resources, not a BSC token-logo submission list.

Conclusion: **no official OKX-owned BSC token-logo GitHub submission repository was identified. Do not create a token-list PR for BNBSP.**

For OKX Wallet display, the official documentation says most non-mainstream tokens may need to be added by users and documents EIP-747 `wallet_watchAsset`, including token address, symbol, decimals and image URL. This is a wallet-side token suggestion mechanism, not an OKX global metadata-listing submission.

## Website integration decision

Do not integrate an OKX DEX aggregator swap component into the BNBSP production website yet.

Gate for implementation:
1. Confirm BNBSP is queryable by OKX token search/basic-info.
2. Confirm BNB -> BNBSP quote.
3. Confirm WBNB -> BNBSP quote.
4. Confirm BNBSP -> WBNB quote.
5. Confirm route includes PancakeSwap / usable liquidity source.
6. Confirm returned token metadata/logo behavior.
7. Then implement the website swap UI using server-side OKX authentication and no exposed API secrets.

## Security

- Never commit OKX API keys, secret keys, passphrases, project IDs, private keys, or signing material.
- Frontend must not expose OKX secret/signing credentials.
- Use environment variables such as `.env.local` only for local/server configuration.
- Keep the BNBSP contract address fixed and verified in application configuration.
- Quote/read-only testing must happen before any swap execution.

## Branch

Project repository: `bnbspoll/assets`
Branch: `feature/okx-dex-bnbsp`

No PR is opened against an OKX repository because no official BSC token-list/logo submission repository was identified.
