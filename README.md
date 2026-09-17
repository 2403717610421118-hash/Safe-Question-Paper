# Safe-Question-Paper
Safe-Question-Paper project aims at simulating a system that minimizes question paper leaks drastically.

This is a purely demonstrative/simulation project built for educational purposes. It showcases concepts of cryptographic security, tamper-evident audit trails, and secure document handling in an exam context. It is **not** intended for real-world deployment.

This Flutter Web application simulates a comprehensive, cryptographically secured exam question paper lifecycle. It demonstrates how modern security principles — including asymmetric encryption, deterministic randomization, biometric access control, and AI-assisted fairness verification — can be applied to prevent question paper leaks in high-stakes examinations.

The system covers the **entire pipeline** from question submission to print center delivery, with full audit logging and chain-of-custody tracking at every step.

To run the web application:
  - Download the web.zip and extract it.
  - Open command prompt and navigate to the extracted folder.
  - Run the command python -m http.server
  - open http://localhost:8000 in a web browser

# Key Concepts Demonstrated
| Concept | What the App Shows |
|---|---|
| **Hybrid Encryption (RSA + AES)** | Each question is AES-256-GCM encrypted; the AES key is wrapped with the setter's RSA public key |
| **Verifiable Randomization** | Paper question order is determined by a seed derived from all setters' key fingerprints via XOR-hash — making it publicly verifiable post-exam |
| **Chain of Custody** | Every action (submission, generation, sealing, dispatch, unlock) is logged to an immutable audit trail with timestamps and actor IDs |
| **Biometric Access Control** | Simulated fingerprint/face ID gates for Admin and Print Officer roles |
| **AI / OCR Fairness Check** | Simulated automated scan verifying print quality, question count, topic distribution, and difficulty balance |
| **Sealed Paper Dispatch** | Papers are "sealed" with a cryptographic signature and can only be opened via OTP + biometric at the print center |
| **Backup Paper Generation** | Emergency alternate paper generated from a derived seed — same question pool, different ordering |
---

---
## Demo Walkthrough
Follow these steps to see the full simulation:
1. **Open the app** → Explore the landing page and system overview
2. **Enter as Question Setter** → Biometric simulation → View your key fingerprint
3. **Click "Load Sample Questions"** → 15 pre-filled questions auto-encrypt and upload
4. **Repeat** for 2 more setters (or use the Admin shortcut to load all 3 setters' questions)
5. **Switch to Admin** → See the encrypted question vault (45 questions)
6. **Click "Generate Paper"** → Watch the card shuffle animation with live seed computation
7. **Click "Seal Paper"** → Paper receives a cryptographic signature and is locked
8. **Navigate to Security Monitor** → Watch the OCR fairness scan → Get the Fairness Certificate
9. **Switch to Print Officer** → Enter OTP `123456` → Biometric scan
10. **Watch the paper decrypt** → View the formatted question paper
11. **Click "Print / Preview"** → See the print-ready PDF with watermarks
12. **Back to Admin → "Backup Paper"** → Generate alternate-seeded paper
---
## Tech Stack
| Layer | Technology |
|---|---|
| **UI Framework** | Flutter 3.x (Web) |
| **State Management** | Provider |
| **Navigation** | go_router |
| **Backend / DB** | Firebase Firestore + Firebase Auth |
| **Cryptography** | `pointycastle` (RSA, AES-GCM), `crypto` (SHA-256) |
| **PDF Generation** | `pdf` + `printing` |
| **Charts** | `fl_chart` |
| **Animations** | Flutter AnimationController + `lottie` |
| **Fonts** | `google_fonts` (Inter + JetBrains Mono) |
---

## Simulated Cryptographic Details
### Key Generation
Each setter gets a simulated RSA-2048 key pair represented as base64-encoded strings. In a real system, these would be hardware-backed keys stored in a HSM.
### Question Encryption
```
plaintext_question → AES-256-GCM(random_nonce) → ciphertext
random_aes_key    → RSA-OAEP(setter_public_key) → wrapped_key
stored = { ciphertext, wrapped_key, nonce, setter_fingerprint }
```
### Paper Seed Derivation
```
seed = SHA256(k₁) XOR SHA256(k₂) XOR SHA256(k₃) ... (all setter public key fingerprints)
Random(seed) → shuffle indices → question order
```
This makes the randomization **verifiable**: anyone with the public keys can independently reproduce the same ordering.
### Paper Signing
The assembled paper is hashed (SHA-256) and the hash is stored as the admin's "signature" — simulating a digital signature over the final document.
---
## Security Concepts vs. Simulation
| What's Real | What's Simulated |
|---|---|
| SHA-256 hash computation | RSA key operations (simplified for browser performance) |
| AES-GCM encryption logic | Biometric authentication |
| Deterministic PRNG seeding | OCR scan results |
| Audit log timestamps | AI fairness scoring |
| Chain of custody tracking | Print center OTP verification |
---
