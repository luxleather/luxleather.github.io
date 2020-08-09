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

## Introduction

This tutorial will guide you in detail through the steps necessary to setup a masternode on Ubuntu 18.04 64-bit remote server (VPS) that is controlled via your local PC wallet. Your local wallet is not required to be kept open all the time whilst collecting masternode payments.

_Requirements:_

- Local system – your computer, which will run Control wallet and hold the masternode coins
- VPS with Ubuntu Server 18.04 64-bit OS and unique IP address that is running 24/7
- Minimum VPS specs: 50 GB of storage space, 4 GB of RAM, at least 1 dedicated CPU core
- Latest Core wallet release of the Masternode coin you want to install
- Collateral - Amount of coins that should be locked in Control wallet to run Masternode

_(NOTE: You will need a different IP address for each masternode you plan to host.)_

## Installation of your PC/Control wallet

**Step 1** – Download wallet

Download the most recent version of the Core wallet of your choosen coin. _(Official github of the crypto project should be safest place to download latest release of the wallet.)_

**Step 2** – Extract and install the wallet

Choose the proper version for your operating system. Extract it, install and run the wallet. After starting the wallet for the first time, it will offer you to make a default data directory. It is recommended that you make new folder where control wallet data will be stored. Choose newly made directory on this step.

**Step 3** – Download the latest snapshot/bootstrap _(optional)_

This is entirely optional step. It is always recommended not to trust, but to verify the blockchain yourself. However, since this would take longer than downloading the latest blockchain snapshot, there is a way to speed up the synchronization by downloading the latest snapshot usually from official github of the coin project.

**Step 4** – Create a Masternode on Control wallet

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

## VPS Remote wallet installation

To be able to access a VPS, you need a software/SSH client like PuTTY for example. After you successfully login to your VPS, follow the further steps.

**Step 1** – Install most recent security patches and firewall

A clean server install will need some software updates. Enter the following command which will bring the system up to date:

`sudo apt-get update && sudo apt-get -y upgrade`

One of the important steps is to check firewall and setup Masternode port:

`ufw status` - check firewall status
`sudo ufw allow <mn port>/tcp` - setting up Masternode port
`sudo ufw enable` - enable firewall

**Step 2** – Download and extract Core wallet for Linux

Enter the following command lines one by one to download and extract wallet _(from official github of your choosen coin)_:

`cd ~ && wget <github url to linux wallet>.tar.gz`

`tar -zxvf <github url to linux wallet>.tar.gz && sudo rm -f <github url to linux wallet>.tar.gz`

for zipped file:

`apt install unzip`

`cd ~ && wget <github url to linux wallet>.zip`

`unzip <github url to linux wallet>.zip && sudo rm -f <github url to linux wallet>.zip`

### Masternode Configuration

**Step 3** – Create the masternode configuration file and populate

Before the node can operate as a masternode a custom configuration file needs to be created. Since we have not loaded the wallet yet, we will create the necessary directories and the configuration file by typing the following command lines one by one:

`mkdir ~/.<coin name> && cd ~/.<coin name> && sudo apt-get install nano && touch <coin name>.conf && nano <coin name>.conf`

This command has created a blank configuration file of our choosen coin where we will enter our masternode configuration variables. Now we should properly setup configuration settings.

Paste the following configuration settings into the editor _(paste is being done simply by right mouse click)_, your conf file on your VPS should look like:

```
rpcuser=<YOUR_OWN_RPC_USERNAME>
rpcpassword=<YOUR_OWN_RPC_PASSWORD>
rpcallowip=127.0.0.1
server=1
daemon=1
logtimestamps=1
maxconnections=256
masternode=1
externalip=<IP of your VPS>
masternodeaddr=<IP of your VPS:PORT>
masternodeprivkey=<Masternode genkey already created in Control wallet console>
```
Save and exit the editor by pressing `CTRL-O` and Enter to save and `CTRL-X` to exit the editor.

**Step 4** – Download the latest snapshot

Same as couple steps earlier, this step is entirely optional. It is always recommended not to trust, but to verify the blockchain yourself. If you still don’t want to go slower, but best and safest way is to search snapshot/bootstrap on official github of the coin project.

To download it, type wget and paste the link address afterwards that you copied earlier and press enter, it should look similar to:

`wget <github snapshot url>.zip`

Wait for the snapshot to download completely. It might take some time, depending on your VPS download speed and size. After the download is complete, you have to unzip the file. Easy way is to type unzip and just copy-paste the name extracted from the end of link address used earlier above:

`unzip <snapshot name>.zip`

After it’s successfully extracted, it is recommended to remove the .zip file to free some space on VPS:

`rm <snapshot name>.zip`

### Masternode start

**Step 5** – Load the masternode

With the configuration created we are now ready to load the masternode and sync to the network. Load the masternode by typing the following command:

`cd ~/<coin name>/bin`

`./<coin name>d -daemon` _(here we starting the daemon)_

You will get the message “<Coin name> server starting”. To follow the progress until the wallet is fully loaded and synchronized, type:

`tail -f ~/.<coin name>/debug.log`

Wait until you see the message similar to:
`2025-05-15 13:31:01 CMasternodeSync::GetNextAsset – Sync has finished`
`2025-05-15 13:31:01 CActiveMasternode::ManageStatus() – not capable: Hot node, waiting for remote activation.`

Once you get this message, you are completely synced and masternode is ready to be started. Press `CTRL-C` to get back to command line.

You can get info of running daemon by:

`./<coin name>-cli getinfo`

You will get back information where in line "block" you can see recent block count. To be sure daemon is on right chain type:

`./<coin name>-cli getblockhash <block>` and compare result of block hash on some explorer of the coin.

**Step 6** - Start Masternode from Control wallet _(final step)_

Now when we have fully synced Masternode on our VPS it is time to start it from Control wallet. To do this go to wallet console and type:

`startmasternode alias false <your MN alias>`

**Step 7** - Final check

If everything went well, you should receive the following message:

“Masternode successfully started”

Also we check on our VPS with command:

`./<coin name>-cli getmasternodestatus`

“Masternode successfully started”
