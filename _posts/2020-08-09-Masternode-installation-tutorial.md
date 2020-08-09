---
layout: single
title:  "Masternode installation tutorial"
layout: single
categories: crypto
---
### Masternode Setup Guide

Contents

- Introduction
- Installation of your PC/Control wallet
- VPS Remote wallet installation
- Masternode configuration
- Masternode start

### Introduction

This tutorial will guide you in detail through the steps necessary to setup a masternode on Ubuntu 18.04 64-bit remote server (VPS) that is controlled via your local PC wallet. Your local wallet is not required to be kept open all the time whilst collecting masternode payments.

_Requirements:_

- Local system – your computer, which will run Control wallet and hold the masternode coins
- VPS with Ubuntu Server 18.04 64-bit OS and unique IP address that is running 24/7
- Minimum VPS specs: 50 GB of storage space, 4 GB of RAM, at least 1 dedicated CPU core
- Latest Core wallet release of the Masternode coin you want to install
- Collateral - Amount of coins that should be locked in Control wallet to run Masternode

(NOTE: You will need a different IP address for each masternode you plan to host.)

### Installation of your PC/Control wallet

Step 1 – Download wallet

Download the most recent version of the Core wallet of your choosen coin. _(Official github of the crypto project should be safest place to download latest release of the wallet.)_

Step 2 – Extract and install the wallet

Choose the proper version for your operating system. Extract it, install and run the wallet. After starting the wallet for the first time, it will offer you to make a default data directory. It is recommended that you make new folder where control wallet data will be stored. Choose newly made directory on this step.

Step 3 – Download the latest snapshot/bootstrap _(optional)_

This is entirely optional step. It is always recommended not to trust, but to verify the blockchain yourself. However, since this would take longer than downloading the latest blockchain snapshot, there is a way to speed up the synchronization by downloading the latest snapshot.

Step 4 – Create a Masternode on Control wallet

First of all, make sure that you have enough coins in your wallet for Masternode collateral.

* Unlock the wallet _(If it is encrypted by password.)_
* Create new address labeled with something like "Masternode01" or "MN01"
* Send exact ammount of collateral coins to this address
* Go to installation folder and open file **masternode.conf**
* Add line for Masternode configuration:
 * `<alias> <vpsIP:port> <masternodeprivkey> <txid> <index>`
  * alias = MN01
  * vpsIP:port = IP address of your VPS and masternode port
  * masternodeprivkey = Get it on Control wallet console by typing `masternode genkey`
  * txid (transaction hash) = Get it on Control wallet console by typing `masternode outputs`
  * index (output index) = Above command will give you transaction hash with output index
* Save **masternode.conf** file and close it
* Restart control wallet
* Now your coins are locked for Masternode

_We will get back here to Control wallet later after setup VPS._
