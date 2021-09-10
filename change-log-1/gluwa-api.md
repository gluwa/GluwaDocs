---
description: All release notes pertaining to Gluwa API and Dashboard
---

# Gluwa API

## 09-04-2021 - Version 2021.09.1

### API

* Added API endpoints to support Gluwa Invest Bond Account functionality
* Added OTP endpoints for Terms and Conditions Onboarding
* Added user agreement endpoints to manage user data and documents for account sign-up
* Updated and expanded the Account Setup checklist endpoint
* Added Bond Account smart contract support
* Added sUSDC-G deposit endpoint functionality
* Updated push notifications for Veriff sessions
* Fixed several InternalServerError issues
* Fixed some UI flow issues including Verification Failed for iOS devices and Log In navigation issues. 

## 08-13-2021 - Version 2021.08.01

### API

* Fixed Webhookreceiver Signature endpoint 
* Enhanced Cloudflare IP Address logging 
* Fixed USDC-G transaction signature validation 
* Removed unused OTP validation from SNGNG and PegUnpeg 
* Added Balance Check before processing the transaction

## 07-27-2021 - Version 2021.07.01

### Dashboard

* Fixed secure flag set on all cookies for tools and dashboard 

### API

* Updates SDKs in accordance with contracts so that updates are aligned
* Implemented Web3ClientFactory and improved RetryQueue and API to handle an increased load in TPS 
* Installed USDC-G support 
* Updated Peg/Unpeg for USDC-G and sUSDC-G gateway
* Installed Peg/Unpeg related features and updates for the Gluwa Internal API, Gluwa public API and interaction endpoints with the Luniverse Gatekeeper
* Fixed GetBalance API to get correct address and balance
* Added additional logging/metrics for GluwaExchangeRequests

## 06-01-2021

### Dashboard

* Updated jQuery to latest version 
* Fixed \(export\) transaction bugs 

### API

* Included HTTP request/response in the logs
* Launched toggle App Settings for currency releases 
* Updated gas price calculation

## 10-13-2020

### Dashboard

* Updated the Transaction History and Details pages with additional information and options

### API

* Updated QR code generation endpoint 
* Updated Webhook responses to V2 \(Opt-in update for Dashboard coming in the near future\)

## 08-09-2020

### API

* Performance improvements
* Backend bug-fixes and enhancements
* Transaction signature validation improvements

## 08-27-2020

### API

* Added additional logging to API backend 
* Improved Bitcoin unspent output handling in exchange API 
* Misc bugfixes

### Dashboard

* Added ability to export transaction history to a comma-separated values \(.CSV\) file 
* Added additional transaction filtering options \(Date, Amount, Order ID\) 
* Improved transaction history filtering Misc bugfixes

