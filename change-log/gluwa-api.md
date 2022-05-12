---
description: All release notes pertaining to Gluwa API and Dashboard
---

# Gluwa API Change Log

## 05-12-2022 - Version 2022.05.01

### API&#x20;

* ETH token support (getbalance/payments/transfers)
* USDC token support (getbalance/payments/transfers)
* USDT support (getbalance/payments/transfers)
* CTC support (payments/transfers)
* Lottery Account fixes

## 03-21-2022 - Version 2022.03.02

### API&#x20;

* Fixed amount and fee for exchange transactions

## 03-21-2022 - Version 2022.03.01

### API&#x20;

* Added pagination and fixed negative values + currency amount for push notifications
* Updated maturity date and deposits + withdrawals in transaction history
* Improved an error handling of "No minimumBlockchainFee"

## 02-16-2022 - Version 2022.02

### API&#x20;

* Launched new Transaction History API endpoint for V7 app redesign
* Launched API endpoint that displays rate/fee amount per token
* Launched API endpoint to return current price in USD&#x20;
* Launched PushNotificationHistory endpoint supporting 'All' currencies
* Updated QR code to include payload in the response&#x20;
* Updated blockchain.info 429 (Over limit) responses

## 01-19-2022 - Version 2022-01

### API&#x20;

* Unified ECurrency used by Gluwa API and MarketMaker&#x20;
* Improved handling of Luniverse and Infura 429 (Over limit) responses&#x20;
* Added alerts for the addresses monitored&#x20;
* Added GCRE (CTC ERC-20 Token) support on QR Code endpoint&#x20;

### API - Dashboard

* Updated password policy

## 12-06-2021 - Version 2021.12

### API&#x20;

* Updated CircuitBreaker policy on RPC clients
* Fixed some minor code smells

### API  - Dashboard

* Updated password policy

## 10-13-2021 - Version 2021.10-1

### API - Gluwa Invest

* Fixed Bond Account deposits showing in push notifications

### API&#x20;

* Launched peg/unpeg (USDC-G <> sUSDC-G)
* Launched USDC-G support&#x20;
* Updated Peg/Unpeg daily limit and minimum amount&#x20;
* Fixed API signature format&#x20;
* Implemented send fee of 0.1% for sUSDC-G&#x20;
* Replaced GasNow by Etherscan Gas Tracker&#x20;
* Replaced ETHless Mint in API by user self-service Mint&#x20;
* Updated GetBalance endpoint
* Added API endpoint for returning all token balances
* Added Gluwa API localization for Push Notifications

## 09-04-2021 - Version 2021.09.1

### API - Gluwa Invest

* Added API endpoints to support Gluwa Invest Bond Account functionality
* Added OTP endpoints for Terms and Conditions Onboarding
* Added user agreement endpoints to manage user data and documents for account sign-up
* Updated and expanded the Account Setup checklist endpoint
* Added Bond Account smart contract support
* Added sUSDC-G deposit endpoint functionality
* Updated push notifications for Veriff sessions
* Fixed several InternalServerError issues
* Fixed some UI flow issues including Verification Failed for iOS devices and Log In navigation issues

### API

* Added API to convert USDC to USDC-G&#x20;
* Refactored code for peg/unpeg&#x20;
* Fixed peg endpoint nonce format&#x20;
* Fixed peg notifications
* Updated queue client factory in the retry function&#x20;
* Fixed wrong hashes passed when calling processUnpeg&#x20;
* Added support for longer nonce in Gluwa API
* Fixed validating transaction hash before attempting to process&#x20;
* Fixed peg notifications&#x20;
* Fixed error in Internal transaction controller for BTC transactions

## 08-30-2021 - Version 2021.08.2

### API

* Fixed transfers on successful peg

## 08-12-2021 - Version 2021.08.1

### API

* Fixed Webhookreceiver Signature endpoint&#x20;
* Enhanced Cloudflare IP Address logging&#x20;
* Fixed USDC-G transaction signature validation&#x20;
* Removed unused OTP validation from SNGNG and PegUnpeg&#x20;
* Added Balance Check before processing the transaction

## 07-28-2021 - Version 2021.07.1

### Dashboard

* Fixed secure flag set on all cookies for tools and dashboard&#x20;

### API

* Updates SDKs in accordance with contracts so that updates are aligned
* Implemented Web3ClientFactory and improved RetryQueue and API to handle an increased load in TPS&#x20;
* Installed USDC-G support&#x20;
* Updated Peg/Unpeg for USDC-G and sUSDC-G gateway
* Installed Peg/Unpeg related features and updates for the Gluwa Internal API, Gluwa public API and interaction endpoints with the Luniverse Gatekeeper
* Fixed GetBalance API to get correct address and balance
* Added additional logging/metrics for GluwaExchangeRequests

## 06-01-2021

### Dashboard

* Updated jQuery to latest version&#x20;
* Fixed (export) transaction bugs&#x20;

### API

* Included HTTP request/response in the logs
* Launched toggle App Settings for currency releases&#x20;
* Updated gas price calculation

## 10-13-2020

### Dashboard

* Updated the Transaction History and Details pages with additional information and options

### API

* Updated QR code generation endpoint&#x20;
* Updated Webhook responses to V2 (Opt-in update for Dashboard coming in the near future)

## 08-09-2020

### API

* Performance improvements
* Backend bug-fixes and enhancements
* Transaction signature validation improvements

## 08-27-2020

### API

* Added additional logging to API backend&#x20;
* Improved Bitcoin unspent output handling in exchange API&#x20;
* Misc bugfixes

### Dashboard

* Added ability to export transaction history to a comma-separated values (.CSV) file&#x20;
* Added additional transaction filtering options (Date, Amount, Order ID)&#x20;
* Improved transaction history filtering Misc bugfixes
