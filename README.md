![Snímka](https://user-images.githubusercontent.com/55763425/206808750-d0846ca5-abb4-4400-9b60-bc72ea1e6240.PNG)


### Cross-chain GMP transfer tool.
- **Transfer through Axelar GMP across 5 compatible chains**
- **Calculate fees for your transactions before making them**

# AxelZen readme contents
#### [1. Introduction](#1-introduction)<br />
#### [2. Overview](#2-overview)<br />
#### [3. Installation](#3-installation-for-local-use)<br />


# 1. Introduction 🌿
Application is deployed & able to be previewed on following link - [Try out AxelZen on Heroku]()

AxelZen is GMP Transfer tool built specifically to enhance Axelar GMP functionality and make cross-chain transfers as simple as possible for everyone. Tool features calculation of fees for each transaction as well as GMP functionality itself. Users can simply login with Metamask and create GMP transfers within seconds. AxelZen UI is designed to be as straight forward as possible.

# 2. Overview 🔎

There are not that many tools focusing on Axelar GMP (General Message Passing) functionality. AxelZen focuses on ease of use, being fast, reliable, open source, user friendly and not centralized. Transactions can be created by filling simple details (Few of them just by selecting from list) and simple click of a button.

The project allows user to start Application, that can right out of the box use Axelar GMP Functionality between 5 compatible chains. 
Currently compatible chains are:
- Ethereum
- Moonbeam
- Avalanche
- Fantom
- Polygon

GMP screen has notifications for important actions (eg. entered address not correct, details not filled, transaction sent and processed, fees calculating or wallet connected/ not connected ).

Instead of filling lengthy calls and having to interact with smart contracts AxelZen fills them for you.

# 3. Installation for local use 

Installation is straight forward as with any Vue.js project.

### Getting local repository ready 
First we start by installing necessary libraries
```
sudo apt install git make
```
Ensure you have latest node.js installed if not use following command
```
sudo n stable
```
Clone this repository with command
```
git clone https://github.com/dudo50/axelzen.git
```

### After the repository is cloned proceed to install application dependencies with
```
pnpm install
```

### Launching application after dependencies are installed
```
pnpm run serve
```
### Building application for production
```
pnpm build
```

## Project entered into:
Axelar Gitcoin The Illuminate/22 Hackathon Bounty.
