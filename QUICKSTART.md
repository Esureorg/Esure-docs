# Five-Minute Local Quick Start

This guide runs the Esure frontend and backend from clean clones on Windows,
macOS, or Linux. It uses Stellar Testnet only and does not require real
credentials.

## Prerequisites

- Git
- Node.js 20+
- npm

## 1. Clone the two repositories

Open two terminals.

Terminal 1:

```bash
git clone https://github.com/Esureorg/Esure-backend.git
cd Esure-backend
```

Terminal 2:

```bash
git clone https://github.com/Esureorg/Esure-frontend.git
cd Esure-frontend
```

## 2. Start the backend

Windows PowerShell:

```powershell
npm install
Copy-Item .env.example .env
npm run dev
```

macOS / Linux:

```bash
npm install
cp .env.example .env
npm run dev
```

The backend listens on `http://localhost:3001`.

## 3. Start the frontend

Windows PowerShell:

```powershell
npm install
Copy-Item .env.example .env.local
npm run dev
```

macOS / Linux:

```bash
npm install
cp .env.example .env.local
npm run dev
```

The frontend listens on `http://localhost:3000`.

## 4. Verify both services

Backend health:

```bash
curl http://localhost:3001/health
```

Expected response:

```json
{"status":"ok","network":"testnet"}
```

Scenario catalogue:

```bash
curl http://localhost:3001/api/v1/scenarios
```

Expected response contains an `items` array with bundled scenario identifiers
such as `issued-asset-payment`.

Open `http://localhost:3000`, select a scenario, and run it on Testnet.

## 5. Stop both services

Press `Ctrl+C` in each terminal.

## Notes

- The backend stores run state in memory and loses it on restart.
- The frontend never requests, receives, or stores Stellar secret keys.
- Use Testnet only. Do not add real network credentials or provider secrets to
  local environment files or commits.
