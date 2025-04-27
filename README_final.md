# Dapp Token Exchange (Sepolia test‑net)

**Živá ukázka:** https://dex-sepolia.fleek.co  
**Repo:** https://github.com/tpazout/dex-exchange

## Architektura
| Vrstva | Technologie | Složka / soubor |
|--------|-------------|-----------------|
| Smart‑contract | Solidity 0.8 – `Token.sol`, `Exchange.sol` | `/contracts` |
| Test‑net deploy | Hardhat skripty (`1_deploy.js`, `2_seed-exchange.js`) | `/scripts` |
| Front‑end | React + Redux, ethers.js, ApexCharts | `/src` |
| CI/CD | Fleek hosting – auto‑deploy z branch `main` | `.fleek.json` |

## Lokální spuštění
```bash
git clone https://github.com/tpazout/dex-exchange.git
cd dex-exchange
npm install
# spustí lokální Hardhat chain
npx hardhat node
# v dalším okně nasadí kontrakty + seed data
npx hardhat run scripts/1_deploy.js --network localhost
npx hardhat run scripts/2_seed-exchange.js --network localhost
# front‑end
npm start
```

## Funkce
* 3 tokeny: **DAPP, mETH, mDAI**
* Order‑book (limit orders, cancel, fill)
* Realtime graf svíček z on‑chain eventů
* Wallet connect (MetaMask), multi‑account demo
