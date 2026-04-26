# 🪙 JeevApp — Blockchain Wallet

> Send & receive crypto like UPI. No gas fees. Ever.

A non-custodial **JEEV token** wallet on Polygon — built with Flutter. Pay anyone by just their **@username**, no wallet addresses needed.

[![Android](https://img.shields.io/badge/Android-APK_Download-brightgreen?style=for-the-badge&logo=android)](../../releases)
[![Polygon](https://img.shields.io/badge/Blockchain-Polygon_Amoy-8247E5?style=for-the-badge&logo=polygon)](https://amoy.polygonscan.com/address/0xc14F8AdC5Af65eAdb3a0A27403C43f3357c3253e)
[![Vercel](https://img.shields.io/badge/Backend-Vercel-black?style=for-the-badge&logo=vercel)](https://vercel.app)
[![Flutter](https://img.shields.io/badge/Built_with-Flutter-02569B?style=for-the-badge&logo=flutter)](https://flutter.dev)

---

## 📸 Screenshots

### 📱 iPhone
<p align="center">

  <img src="https://github.com/user-attachments/assets/88a60a5d-8ba9-488c-bcc2-69f299a240e2" width="180" />
  <img src="https://github.com/user-attachments/assets/cd7935b1-8f9c-4ba2-8267-6a4f02e1de0b" width="180" />
  <img src="https://github.com/user-attachments/assets/ed944acb-e983-42b8-bef9-89c3e4707b95" width="180" />
  <img src="https://github.com/user-attachments/assets/9aaede72-f184-4dda-bebc-6a1deae941b7" width="180" />
  <img src="https://github.com/user-attachments/assets/9dfe31c1-383e-4a64-824a-eff622054db0" width="180" />
</p>

### 🤖 Android
<p align="center">
  <img src="https://github.com/user-attachments/assets/4ba397c0-0d3b-40d4-b607-863d6957163b" width="180" />
  <img src="https://github.com/user-attachments/assets/5f86a068-f503-4cae-8801-91b07f22f318" width="180" />
</p>

---

## ✨ Features

- 🔐 **Non-custodial** — your keys, your crypto. Always.
- ⚡ **Zero gas fees** — admin relay pays all gas, users pay nothing
- 👤 **Pay by @username** — no more copy-pasting wallet addresses
- 📷 **Scan & Pay** — scan any QR code and send instantly
- 📜 **Transaction history** — full on-chain record
- 🔑 **12-word recovery phrase** — restore your wallet anywhere
- 🌙 **Dark UI** — clean, minimal, frosted glass design

---

## 🔭 How It Works

```
┌─────────────────────────────────────────────────────────────────┐
│                      📱 USER (Mobile App)                       │
│                                                                 │
│   Create Wallet  ──►  BIP39 Mnemonic  ──►  Ethereum Address    │
│   Register @username  ──►  Vercel API  ──►  Blob Storage        │
│                                                                 │
│   Send JEEV:                                                    │
│   Enter @username  ──►  Lookup Address  ──►  POST /api/transfer │
│                                                ▼                │
│                                   🤖 Admin Wallet (Operator)    │
│                                                ▼                │
│                                  operatorTransfer(from, to, amt)│
│                                                ▼                │
│                                  🔗 Polygon Amoy Blockchain     │
│                                     JEEV Token Contract         │
└─────────────────────────────────────────────────────────────────┘
```

---

## 💸 Payment Flow (Gasless)

```
User A                  Vercel Backend              Blockchain
  │                           │                         │
  │── Send 10 JEEV ──────────►│                         │
  │   to @jeevan              │                         │
  │                           │── Lookup @jeevan ──────►│
  │                           │◄─ address: 0xABC... ────│
  │                           │                         │
  │                           │── Check A's balance ───►│
  │                           │◄─ 50 JEEV ──────────────│
  │                           │                         │
  │                           │── operatorTransfer ─────►│
  │                           │   (A → B, 10 JEEV)      │
  │                           │   Admin pays gas ✓       │
  │                           │◄─ txHash: 0x... ─────────│
  │                           │                         │
  │◄─ ✅ Success + txHash ────│                         │
```

---

## 👛 Wallet Creation Flow

```
📱 App Opens
      │
      ▼
  Existing wallet?
      │
      ├── ✅ YES ──► Load address + username ──► 🏠 Home
      │
      └── ❌ NO ──► Onboarding
                        │
                        ├── 🆕 Create New
                        │       ▼
                        │   Generate 12-word BIP39 mnemonic
                        │   Derive private key + Ethereum address
                        │   Store encrypted in Android Keystore
                        │   Register @username via API
                        │   🎁 Mint welcome JEEV tokens
                        │       ▼
                        │   🏠 Home Screen
                        │
                        └── 🔄 Restore Wallet
                                ▼
                            Enter 12-word mnemonic
                            Re-derive address
                            Register @username via API
                                ▼
                            🏠 Home Screen
```

---

## 🛠 Tech Stack

### 📱 Mobile App
| Technology | Purpose |
|---|---|
| Flutter (Dart) | Android app |
| web3dart | Ethereum wallet & blockchain reads |
| bip39 | 12-word mnemonic generation |
| flutter_secure_storage | Encrypted key storage (Android Keystore) |
| mobile_scanner | QR code camera scanner |
| qr_flutter | Generate receive QR codes |
| google_fonts | UI typography |

### ⚙️ Backend (Vercel Serverless)
| Technology | Purpose |
|---|---|
| Node.js | Serverless API functions |
| ethers.js | Signs & submits blockchain transactions |
| Vercel Blob | Username → address persistent storage |
| Vercel | Hosting + deployment |

### ⛓️ Blockchain
| Technology | Purpose |
|---|---|
| Solidity ^0.8.20 | Smart contract |
| OpenZeppelin ERC-20 | Standard token base |
| Polygon Amoy Testnet | Low-fee blockchain network |
| Hardhat | Compile, test, deploy |

---

## 🧠 Smart Contract Design

The key innovation is `operatorTransfer` — admin moves tokens between users with **no approval, no signature, no gas from the user**.

```
❌  Standard ERC-20:
    User calls transfer() → User pays gas

✅  JeevApp:
    Admin calls operatorTransfer(from, to, amount)
    → Tokens move from User A to User B
    → Admin pays gas
    → User pays nothing
```

**📄 Contract:** `0xc14F8AdC5Af65eAdb3a0A27403C43f3357c3253e`  
**🌐 Network:** Polygon Amoy Testnet (Chain ID: 80002)  
**🔍 Explorer:** [View on PolygonScan](https://amoy.polygonscan.com/address/0xc14F8AdC5Af65eAdb3a0A27403C43f3357c3253e)

---

## 🔌 API Endpoints

| Endpoint | Method | Purpose |
|---|---|---|
| `/api/transfer` | POST | 💸 Send JEEV — gasless relay |
| `/api/username` | GET | 🔍 Lookup address by @username |
| `/api/username` | POST | 📝 Register new @username |
| `/api/mint` | POST | 🎁 Mint welcome JEEV to new wallet |
| `/api/history` | GET | 📜 Fetch transaction history |
| `/api/health` | GET | 🟢 Service health check |

---

## 🔒 Security

- 🔑 Private keys **never leave the device** — stored in Android Keystore
- 🚫 Backend never sees or stores any private keys
- 🛡️ Admin is the only operator — rate limited to 5 tx/min per address
- 📋 Recovery phrase accessible only in settings

---

## 🚀 Status

| Platform | Status |
|---|---|
| 🤖 Android | ✅ Available — [Download APK](../../releases) |
| 🍎 iOS | 🔜 Coming soon |
| ⚙️ Backend | ✅ Live on Vercel |
| ⛓️ Smart Contract | ✅ Deployed on Polygon Amoy |

---

## 👨‍💻 Author

**Jeevan** — built solo, end-to-end.  
Smart contract → serverless backend → mobile app. All of it.

[![LinkedIn](https://img.shields.io/badge/LinkedIn-Connect-0A66C2?style=for-the-badge&logo=linkedin)](https://linkedin.com/in/jeevanj76)
[![GitHub](https://img.shields.io/badge/GitHub-Follow-181717?style=for-the-badge&logo=github)](https://github.com/jjgowda)
