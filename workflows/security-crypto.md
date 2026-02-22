---
description: Apply cryptography and secrets management — field encryption, HashIDs, secure hashing, and environment secrets.
---

# 🔐 Security — Cryptography & Secrets

## What to Implement

### 1. Field Encryption (PII Protection)
Encrypt personally identifiable information (PII) before saving to the database.
- Target fields: SSN, national ID, phone numbers, birth dates, medical info.
- **Do not** encrypt fields you need to search against (unless using blind indexing).
- Use AES-256-GCM or XChaCha20-Poly1305.
- Keys must be rotated periodically.

### 2. Password Hashing
- **Never** use MD5, SHA-1, or plain SHA-256/512 for passwords.
- Use **Argon2id** (preferred) or **Bcrypt** (minimum cost 12).
- Ensure salts are randomly generated per user (handled automatically by modern frameworks).

### 3. HashIDs (ID Obfuscation)
Prevent Insecure Direct Object Reference (IDOR) and resource enumeration by hiding numeric IDs.
- Convert `/users/15` → `/users/X9b2Rp`.
- Use a unique salt for the HashID configuration (do not leave empty).
- Minimum length: 6-8 characters.

### 4. Secrets Management
- Add `.env` to `.gitignore` immediately.
- Never hardcode API keys, passwords, or encryption keys in the codebase.
- Warn if sensitive keys are found in the codebase:
  - `AKIA[0-9A-Z]{16}` (AWS)
  - `sk_live_[0-9a-zA-Z]{24}` (Stripe)
- Use a dot-env schema (`.env.example`) with dummy values for new developers.

### 5. Random String Generation
- Use cryptographically secure pseudorandom number generators (CSPRNG) for tokens, OTPs, and temporary passwords.
- `random_bytes()` in PHP, `crypto.randomBytes()` in Node, `secrets` module in Python.

### 6. Asymmetric Payload Encryption (E2EE for Login)
Secure highly sensitive payloads (like passwords during login) by encrypting them on the frontend before sending them over the network, providing an extra layer of defense even if HTTPS/TLS is compromised.
- **Backend (BE)**: Generates an RSA/ECC Key Pair. Sends the **Public Key** to the Frontend.
- **Frontend (FE)**: Uses the Public Key to encrypt the password before sending the login POST request.
- **Backend (BE)**: Receives the encrypted payload and decrypts it using the **Private Key** (stored securely in memory/vault) before hashing and comparing it with the database.
- *Note:* This prevents MITM attacks on load balancers or intermediate proxies from seeing the plaintext password.

## Implementation by Tech

| Tech | Field Encryption | Hashing | HashID | CSPRNG | Asymmetric Payload (BE) |
|------|------------------|---------|--------|--------|-------------------------|
| **Laravel** | `encrypt()` / Casts `encrypted` | `Hash::make()` (Bcrypt/Argon2) | `vinkla/hashids` | `Str::random()` | `openssl_pkey_new()` / `phpseclib` |
| **Express.js** | custom crypto module | `bcrypt` / `argon2` npm | `hashids` npm | `crypto.randomBytes()` | `crypto.generateKeyPairSync('rsa')` |
| **Next.js** | `crypto` or Iron Session | `bcryptjs` / `argon2` | `hashids` npm | `crypto.randomBytes()` | `crypto` module (Node) |
| **Django** | `django-cryptography` | `make_password` (PBKDF2/Argon2) | `django-hashid-field` | `secrets.token_hex()` | `cryptography.hazmat.primitives` |
| **FastAPI** | `cryptography.fernet` | `passlib[bcrypt]` | `hashids` pip | `secrets.token_urlsafe()` | `cryptography` Python package |
| **React/Vue (FE)**| N/A | N/A | N/A | N/A | `jsencrypt` / `node-forge` (encrypt only) |
