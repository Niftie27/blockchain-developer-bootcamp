
# Dapp Token Exchange (localhost setup)

**Live demo:** https://low-coffeeshop-big.on-fleek.app/  
**Repository:** https://github.com/Niftie27/DEX

## Architecture
| Layer | Technology | Folder / File |
|--------|-------------|-----------------|
| Backend (Smart-contract) | Solidity 0.8 – `Token.sol`, `Exchange.sol` | `/contracts` |
| Test-net deploy | Hardhat scripts (`1_deploy.js`, `2_seed-exchange.js`) | `/scripts` |
| Front-end | React + Redux, ethers.js, ApexCharts | `/src` |
| CI/CD | Fleek hosting – auto-deploy from branch `main` | `.fleek.json` |
| Containerization | Docker (Dockerfile, docker-compose.yml) | |
| Environment | WSL2 (Windows Subsystem for Linux) | |

## Prerequisites (Windows 10/11)
* Open **PowerShell as Administrator** and run:
```powershell
dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart  
dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart  
Enable-WindowsOptionalFeature -Online -FeatureName Microsoft-Hyper-V -All
If prompted, type “y” and press Enter. Ignore any error messages.
The first command enables WSL (Windows Subsystem for Linux)
The second enables Virtual Machine Platform required for WSL2 and Docker
The third activates the Hyper-V hypervisor
Restart your PC after this step.
```

**Ubuntu & Windows Terminal (Windows 10/11)**  
Download and install WSL2  
Download, install & launch Ubuntu from Microsoft Store  
 - Set username and password (not visible) and close Ubuntu terminal  
Download, install & launch Windows Terminal from Microsoft Store  
 - In Settings → Startup set Ubuntu as the default profile  
 - Click Save and close Windows Terminal  
 - Open Windows Terminal again and run:
```bash
sudo apt-get update
sudo apt-get install build-essential
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.7/install.sh | bash
exec bash
nvm install 20.10.0
```

(Optional) Add VS Code alias and working directory:
```bash
echo "alias code='"/mnt/c/Program Files/Microsoft VS Code/Code.exe" --unity-launch'" >> ~/.bashrc
mkdir ~/code && echo "cd ~/code" >> ~/.bashrc
```

### Installation and local setup
```bash
# 1. Clone the repository
git clone https://github.com/Niftie27/dex.git

# 2. Navigate into the project directory
cd dex

# 3. Install dependencies
npm install

# 4. Start the local Ethereum node (Hardhat)
npx hardhat node
# Starts local blockchain (Hardhat Network)

# 5. In a new terminal window: Deploy contracts
npx hardhat run --network localhost scripts/1_deploy.js
# Deploy Token and Exchange contracts

# 6. Seed the exchange
npx hardhat run --network localhost scripts/2_seed-exchange.js
# Creates initial token balances for 2 accounts to trade on the exchange

# 7. Start the front-end
npm start
# Opens React front-end at http://localhost:3000
```

**Note for Linux/macOS**  
Skip the Prerequisites for Windows and continue directly to Installation and local setup:
```bash
nvm install 20  # latest LTS
git clone https://github.com/Niftie27/dex.git
cd dex
npm install
npx hardhat node
npx hardhat run --network localhost scripts/1_deploy.js
npx hardhat run --network localhost scripts/2_seed-exchange.js
npm start
```

#### MetaMask (crypto wallet)
Install the MetaMask Chrome extension and create a new account to interact with the DEX.

##### (optional) Redux extension
For inspecting actions and state. Reselect is used for memoized selectors (`createSelector`).

## Features
* 3 tokens: **DAPP, mETH, mDAI**
* Order book (deposit, withdraw, limit orders, cancel, fill, transaction status)
* Real-time candlestick chart from on-chain events
* Wallet connect (MetaMask)
