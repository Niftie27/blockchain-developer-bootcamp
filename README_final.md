# Dapp Token Exchange (Sepolia testnet only (zatím))

**Živá ukázka:** https://low-coffeeshop-big.on-fleek.app/  
**Repo:** https://github.com/Niftie27/blockchain-developer-bootcamp/tree/master

## Architektura
| Vrstva | Technologie | Složka / soubor |
|--------|-------------|-----------------|
| Smart‑contract | Solidity 0.8 – `Token.sol`, `Exchange.sol` | `/contracts` |
| Test‑net deploy | Hardhat skripty (`1_deploy.js`, `2_seed-exchange.js`) | `/scripts` |
| Front‑end | React + Redux, ethers.js, ApexCharts | `/src` |
| CI/CD | Fleek hosting – auto‑deploy z branch `main` | `.fleek.json` |

## Lokální spuštění
```bash
git clone https://github.com/Niftie27/blockchain-developer-bootcamp/tree/master
cd dex-exchange
npm install
# spustí lokální Hardhat chain
npx hardhat node
# v dalším okně nasadí kontrakty + seed data
npx hardhat run --network localhost scripts/1_deploy.js
npx hardhat run --network localhost scripts/2_seed-exchange.js
# front‑end
npm start
```

## Funkce
* 3 tokeny: **DAPP, mETH, mDAI**
* Order‑book (deposit, withdraw, limit orders, cancel, fill, tx status)
* Realtime graf svíček z on‑chain eventů
* Wallet connect (MetaMask), multi‑account demo