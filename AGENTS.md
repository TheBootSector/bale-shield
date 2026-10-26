# AGENTS.md — Development Guidelines for Bale Web Encryption Extension

Welcome to the **Bale Web Encryption Extension** project. This file establishes the behavioral rules, architecture principles, and constraints for all AI coding agents working in this repository.

---

## 1. Project Overview & Vision
This project is an open-source browser extension (Chrome / Firefox / Edge) designed to introduce a client-side **End-to-End Encryption (E2EE) / Symmetric Encryption layer** on top of the **Bale Web** messenger (`web.bale.ai`).

### Primary Goal
- **Zero-Knowledge Transport:** The server (Bale infrastructure) must only see encrypted ciphertext envelopes.
- **Client-Side Cryptography:** All encryption and decryption operations occur strictly within the client's browser using audited cryptographic standards (e.g., standard Web Crypto API).
- **Seamless UX:** Intercept outgoing messages to encrypt them before dispatch, and intercept/render incoming encrypted messages seamlessly in the chat interface.

---

## 2. Team & Collaboration Model
- **Ownership:** Joint 50/50 partnership between Yasin Saeedi and co-developer.
- **Visibility:** Public open-source repository on GitHub.
- **Standard of Work:** Production-grade modular code, comprehensive commit messages, clear documentation, and clean pull requests.

---

## 3. Strict Operating Rules for AI Agents

### 🚨 Rule 1: No Unapproved Executable Code
- **DO NOT** generate or scaffold executable application files (e.g., `background.js`, `content.js`, UI components, build manifests) until the architectural interview and technical specification are approved by the user.
- Focus strictly on documentation, threat modeling, architecture decision records (ADRs), and configuration during initial phases.

### 🔒 Rule 2: Absolute Secret & Key Protection
- The `.env` file and all credentials (including GitHub tokens, API keys, private keys, passphrases) **MUST NEVER** be committed to Git, logged to console, or transmitted outside the local machine.
- Verify that `.gitignore` always excludes `.env*`, `*.token`, `*.secret`, `*.key`, and any temporary dump files.
- Cryptographic keys must never be hardcoded or stored in persistent unencrypted browser storage (`localStorage` without encryption).

### 🛡️ Rule 3: Cryptographic Integrity
- **Never Roll Your Own Crypto:** Always utilize the native, hardware-accelerated **Web Crypto API** (`crypto.subtle`) or battle-tested audited libraries (e.g., `@noble/ciphers`).
- **Initial Mode:** Robust Symmetric Encryption (AES-GCM 256-bit with unique 96-bit IV per message, or ChaCha20-Poly1305) with key derivation via PBKDF2 / Argon2id.
- **Payload Integrity:** Every ciphertext packet must carry authentication tags (AEAD) to prevent tampering, bit-flipping, or injection attacks.
- **Message Format:** Standardized envelope format (e.g., base64 or prefixed magic byte tag `[ENC:...v1...]`) so non-extension clients display a graceful fallback notice instead of crashing.

### 🧩 Rule 4: Extension Architecture (Manifest V3)
- Adhere strictly to Chrome Extension **Manifest V3**.
- Keep Content Scripts lightweight and defensive against DOM mutations (using `MutationObserver` safely without memory leaks or race conditions).
- Ensure safe isolation between extension context and page context (`world: "ISOLATED"` vs `"MAIN"`).

---

## 4. Repository Structure & Conventions (Planned)

```text
├── docs/                      # Architectural specs, threat models, ADRs
│   ├── THREAT_MODEL.md
│   └── ARCHITECTURE.md
├── .env                       # Strictly ignored local secrets (tokens, keys)
├── .gitignore                 # Git ignore rules
├── AGENTS.md                  # Instructions for AI agents (this file)
└── README.md                  # Project introduction & user/developer guide
```

---

## 5. Decision Making & Workflow
- Use `/grill-me` style inquiry to resolve ambiguities before major architectural decisions.
- Maintain a changelog and update documentation whenever a design decision is made.
