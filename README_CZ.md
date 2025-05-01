
# Dapp Token Exchange (lokální spuštění)

**Živá ukázka:** https://low-coffeeshop-big.on-fleek.app/  
**Repozitář:** https://github.com/Niftie27/DEX

## Architektura
| Vrstva | Technologie | Složka / soubor |
|--------|-------------|-----------------|
| Backend (Smart-kontrakt) | Solidity 0.8 – `Token.sol`, `Exchange.sol` | `/contracts` |
| Test-net deploy | Hardhat skripty (`1_deploy.js`, `2_seed-exchange.js`) | `/scripts` |
| Front-end | React + Redux, ethers.js, ApexCharts | `/src` |
| CI/CD | Fleek hosting – automatické nasazení z větve `main` | `.fleek.json` |
| Kontejnerizace | Docker (Dockerfile, docker-compose.yml) | |
| Prostředí | WSL2 (Windows Subsystem for Linux) | |

## Požadavky (Windows 10/11)
* Otevři **PowerShell jako správce** a spusť:
```powershell
dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart  
dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart  
Enable-WindowsOptionalFeature -Online -FeatureName Microsoft-Hyper-V -All
Pokud budeš vyzván, napiš “y” a stiskni Enter. Případné chyby ignoruj.
První příkaz zapne WSL (Windows Subsystem for Linux).
Druhý příkaz zapne Virtual Machine Platform, nutnou pro WSL2 a Docker.
Třetí příkaz aktivuje Hyper-V hypervisor.
Po dokončení restartuj počítač.
```

**Ubuntu a Windows Terminal (Windows 10/11)**  
Nainstaluj a spusť WSL2  
Nainstaluj a spusť Ubuntu z Microsoft Store  
 - Nastav uživatelské jméno a heslo (není vidět), poté zavři terminál Ubuntu  
Nainstaluj a spusť Windows Terminal z Microsoft Store  
 - V Nastavení → Startup nastav Ubuntu jako výchozí profil  
 - Ulož nastavení a zavři Windows Terminal  
 - Otevři Windows Terminal znovu a spusť:
```bash
sudo apt-get update
sudo apt-get install build-essential
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.7/install.sh | bash
exec bash
nvm install 20.10.0
```

(Volitelné) Přidej alias pro VS Code a pracovní adresář:
```bash
echo "alias code='"/mnt/c/Program Files/Microsoft VS Code/Code.exe" --unity-launch'" >> ~/.bashrc
mkdir ~/code && echo "cd ~/code" >> ~/.bashrc
```

### Instalace a lokální spuštění
```bash
# 1. Klonuj repozitář
git clone https://github.com/Niftie27/dex.git

# 2. Přejdi do složky s projektem
cd dex

# 3. Nainstaluj závislosti
npm install

# 4. Spusť lokální Ethereum uzel (Hardhat)
npx hardhat node
# Spustí lokální blockchain (Hardhat síť)

# 5. V novém terminálu nasaď smart kontrakty
npx hardhat run --network localhost scripts/1_deploy.js
# Nahrání Token a Exchange kontraktů

# 6. Naplň burzu daty
npx hardhat run --network localhost scripts/2_seed-exchange.js
# Vytvoří počáteční zůstatky tokenů pro 2 účty, aby mohly mezi sebou obchodovat

# 7. Spusť front-end
npm start
# Otevře React front-end na http://localhost:3000
```

**Poznámka pro Linux/macOS**  
Přeskoč Windows požadavky a pokračuj přímo na instalaci a spuštění:
```bash
nvm install 20  # nejnovější LTS verze
git clone https://github.com/Niftie27/dex.git
cd dex
npm install
npx hardhat node
npx hardhat run --network localhost scripts/1_deploy.js
npx hardhat run --network localhost scripts/2_seed-exchange.js
npm start
```

#### MetaMask (krypto peněženka)
Nainstaluj rozšíření MetaMask do Chrome a vytvoř nový účet pro interakci s DEX.

##### (volitelné) Redux extension
Pro sledování akcí a stavů. Reselect se používá pro memoizované selektory (`createSelector`).

## Funkce
* 3 tokeny: **DAPP, mETH, mDAI**
* Order book (vklady, výběry, limitní příkazy, zrušení, vyplnění, stav transakce)
* Reálný časový graf svíček z on-chain událostí
* Připojení peněženky přes MetaMask
