# ✅ n8n Credentials Migration & Encryption Key Setup (Docker)

This guide explains how to **safely export and import n8n credentials** between machines using Docker, **without breaking secrets**.

---

## ⚠️ VERY IMPORTANT CONCEPT (Read First)

* n8n **encrypts all credentials**
* Credentials **ONLY work with the same encryption key**
* If the key is missing or different → **credentials are permanently broken**
* By default, n8n auto-generates a key and stores it **inside the volume**

👉 **Best practice**: explicitly set `N8N_ENCRYPTION_KEY`

---

# 🔹 PART 1: Identify the Correct Volume (CRITICAL)

### Check which volume n8n is actually using:

```bash
docker inspect n8n-puppeteer --format '{{ json .Mounts }}'
```

### Correct output example:

```json
{
  "Name": "n8n-docker_n8n_storage",
  "Destination": "/home/node/.n8n"
}
```

✅ This is the **ONLY volume that matters**
❌ Any other similarly named volume is irrelevant

---

# 🔹 PART 2: Extract the Existing Encryption Key

Run inside the running container:

```bash
docker exec -it n8n-puppeteer cat /home/node/.n8n/config
```

### Output example:

```json
{
  "encryptionKey": "G204euj9MgMh9vduXRVJPH2iy9pBtGU8"
}
```

🔐 **Save this key safely** — you cannot recover it later.

---

# 🔹 PART 3: Set the Encryption Key in docker-compose.yml

Add this line to your service `environment` section:

```yaml
- N8N_ENCRYPTION_KEY=G204euj9MgMh9vduXRVJPH2iy9pBtGU8
```

📌 The key **must be EXACTLY the same**
📌 No quotes, no spaces, no changes

---

# 🔹 PART 4: Fix “Mismatching Encryption Keys” Error (MOST COMMON ISSUE)

### ❌ Error you may see:

```
Error: Mismatching encryption keys.
```

### ❌ Why it happens:

* n8n already saved an old key inside the volume
* You later added a new key via environment variable
* Now n8n sees **two different keys**

---

## ✅ Correct Fix (Safe Method)

### 1️⃣ Stop n8n:

```bash
docker compose down
```

### 2️⃣ Delete ONLY the config file from the **correct volume**:

```bash
docker run --rm \
  -v n8n-docker_n8n_storage:/home/node/.n8n \
  alpine sh -c "rm -f /home/node/.n8n/config"
```

✔ This deletes ONLY the encryption config
✔ Workflows, credentials, database are safe

### 3️⃣ Restart n8n:

```bash
docker compose up -d
```

---

## ✅ Correct Startup Output (GOOD SIGN)

```text
User settings loaded from: /home/node/.n8n/config
(no encryption error)
```

---

# 🔹 PART 5: Export Credentials (Old Machine)

```bash
docker exec -it n8n-puppeteer n8n export:credentials --all --output=/home/node/credentials.json
```

### Expected output:

```
Successfully exported 33 credentials.
```

Copy to host:

```bash
docker cp n8n-puppeteer:/home/node/credentials.json .
```

---

# 🔹 PART 6: Import Credentials (New Machine)

### ❌ Common mistake:

```
ENOENT: no such file or directory
```

### ❌ Cause:

The file exists on the host, **not inside the container**

---

## ✅ Correct Steps

### 1️⃣ Copy file into container:

```bash
docker cp credentials.json n8n-puppeteer:/home/node/credentials.json
```

### 2️⃣ Import credentials:

```bash
docker exec -it n8n-puppeteer n8n import:credentials --input=/home/node/credentials.json
```

### ✅ Correct output:

```
Successfully imported 33 credentials.
```

---

# 🔹 PART 7: Final Verification

1. Open n8n UI → **Credentials**
2. Open any credential
3. Click **Test**

### ✅ Success:

* Test passes
* No errors

### ❌ Failure:

* Test fails
* Encryption key mismatch or missing key

---

# ❌ COMMON MISTAKES & THEIR ERRORS

| Mistake                      | Error                         |
| ---------------------------- | ----------------------------- |
| Different encryption key     | `Mismatching encryption keys` |
| Wrong volume cleaned         | Error persists                |
| File not copied to container | `ENOENT: no such file`        |
| Key lost                     | ❌ Permanent credential loss   |
| Assuming rebuild is needed   | ❌ Not required                |

---

# ✅ WHEN TO REBUILD IMAGE?

| Change               | Rebuild? |
| -------------------- | -------- |
| Dockerfile changed   | ✅ Yes    |
| Installed packages   | ✅ Yes    |
| Environment variable | ❌ No     |
| Encryption key       | ❌ No     |

---

# 🧠 FINAL BEST PRACTICES

✔ Always set `N8N_ENCRYPTION_KEY`
✔ Backup the **storage volume**
✔ Keep the key in a password manager
✔ Test restore once
✔ Never delete volumes blindly

---

## 🎉 FINAL RESULT

You now have:

* Portable credentials
* Safe backups
* Repeatable migration process
* Zero encryption surprises

