# 🛡️ SafeVault – Decentralized Encrypted Storage using Walrus + Seal

## 🌍 Overview
**SafeVault** enables secure, decentralized file storage and controlled data sharing using **Walrus** and **Seal**, ensuring encryption, privacy, and verifiable access via the **Sui blockchain**.

Users can upload files, encrypt them locally, and store the encrypted data on **Walrus**. Access permissions are enforced on-chain using **Sui Move smart contracts**, while **Seal** manages decentralized encryption and decryption through threshold key servers.

---

## 🚀 Tech Stack
- **Frontend:** React, TailwindCSS, Walrus SDK, Seal SDK
- **Backend:** Node.js + Express (optional)
- **Blockchain Layer:** Sui (Move)
- **Storage:** Walrus decentralized storage
- **Encryption / Access:** Seal DSM

---

## ⚙️ Features
✅ Client-side encryption using Seal SDK  
✅ Decentralized storage on Walrus  
✅ On-chain access rules using Sui Move  
✅ Threshold key decryption (t-out-of-n)  
✅ Verifiable and tamper-proof data availability  
✅ Works for private files, gated media, or AI datasets  

---

## 🧠 Architecture
User → Seal SDK (Encrypt) → Walrus SDK (Store)
↓
Sui Move (Access Policy)
↓
Authorized User → Seal Key Servers (Decrypt) → View File

---

## 🧩 How It Works

1. **Upload:**  
   File is encrypted locally using Seal SDK before upload.  
   The encrypted blob is then uploaded to Walrus.

2. **Store:**  
   Walrus returns a Blob ID, which is recorded on Sui along with metadata.

3. **Access Control:**  
   A Sui Move smart contract defines which users (addresses) can decrypt.

4. **Retrieve:**  
   Authorized users fetch the Blob ID from Walrus and request decryption from Seal key servers.

5. **Decrypt:**  
   Seal verifies access policy on Sui → generates decryption key shares → file is decrypted locally.

---

## 🔒 Example Flow (React)
```javascript
import { encryptFile, decryptFile } from "./utils/seal";
import { uploadToWalrus, fetchFromWalrus } from "./utils/walrus";

async function handleUpload(file) {
  const encrypted = await encryptFile(file);
  const blobId = await uploadToWalrus(encrypted);
  console.log("Stored on Walrus with Blob ID:", blobId);
}

async function handleAccess(blobId) {
  const encryptedBlob = await fetchFromWalrus(blobId);
  const decrypted = await decryptFile(encryptedBlob);
  console.log("Decrypted file:", decrypted);
}
```



## Sui Move Example

module safevault::access_control {

    public fun can_access(user: address, file_id: ID): bool {
        // Simple allowlist check
        let owner = file_owner(file_id);
        let allowlist = get_allowlist(file_id);
        return user == owner || user_in_allowlist(user, allowlist);
    }
}

