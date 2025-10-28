# 🔐 Password Strength Checker and Brute Force Analysis

[![License](https://img.shields.io/badge/License-MIT-blue.svg)](https://opensource.org/licenses/MIT)
[![Repo Status](https://img.shields.io/badge/Status-Lab%20Notes-brightgreen)]()

---

## 📝 Overview

This repository contains documentation, tools, and a concise lab record for a hands-on exercise that explores password strength analysis and controlled brute-force/cracking demonstrations.  
All testing described was performed on **synthetic, self-generated passwords only**. The workflow combined **online password strength meters** with local cracking tools (John The Ripper and Hashcat) to compare scores, feedback, and real-world crackability.

---

## 🛠️ Online Password Strength Tools

These web services were used to evaluate password strength and check exposure against known breaches:

- **Password Meter** — https://passwordmeter.com/  
- **Have I Been Pwned — Passwords** — https://haveibeenpwned.com/Passwords

> ⚠️ **Safety note:** Only synthetic/test passwords were submitted to online tools. Never submit real account passwords to third-party services.

---

## 💻 Brute-Force Tool Setup: John The Ripper (JTR)

The steps below were used to install and configure **John The Ripper (magnum-jumbo)** and **Rexgen** on a Linux environment for controlled cracking demonstrations.

### 1. Install prerequisites for JTR & Rexgen
```bash
# Essential build tools & libraries
sudo apt-get update
sudo apt-get install -y build-essential libssl-dev yasm libgmp-dev libpcap-dev libnss3-dev libkrb5-dev pkg-config libopenmpi-dev openmpi-bin zlib1g-dev libbz2-dev

# Prereqs for Rexgen
sudo apt-get install -y flex cmake bison git
```

### 2. Install Rexgen
```bash
cd /opt
sudo git clone https://github.com/teeshop/rexgen.git
cd rexgen
sudo ./install.sh
sudo ldconfig
```

### 3. Download and compile John The Ripper (magnum-jumbo)
```bash
cd /opt
sudo git clone https://github.com/magnumripper/JohnTheRipper.git

cd JohnTheRipper/src/
./configure --enable-mpi

# compile (use -jN where N = number of cores)
make -s clean && make -sj4

# test the build
cd ../run
./john --test
```

---

## 🧪 Lab Exercise: Methodology & Phases

> **Note:** All experiments used only synthetic passwords created for this lab. No real credentials were used.

### Phase 1 — Preparation & Password Generation
- Created a working folder for the lab and generated a diverse set of synthetic passwords:
  - Weak numeric strings
  - Medium alpha-numeric samples
  - Long complex strings with symbols
  - Several 3–4 word passphrases
- Saved all generated samples into a single test list for analysis.

- Generating Passwords via CLI Tools like pwgen can be useful for quickly generating random strings. 
# Install pwgen
```
sudo apt update
sudo apt install -y pwgen
```

# simple numeric (weak)
```
pwgen -1 6 5 > passwords_simple.txt
```
# medium (letters+numbers)
```
pwgen -1 10 5 > passwords_medium.txt
```
# complex (upper+lower+digits+symbols) via openssl
```
for i in {1..5}; do openssl rand -base64 18 | tr -dc 'A-Za-z0-9!@#$%&*()-_+=' | head -c 16 >> passwords_complex.txt; echo >> passwords_complex.txt; done
```
# passphrases (3-4 random words)
```
for i in {1..5}; do shuf -n4 /usr/share/dict/words | tr '\n' ' ' | sed 's/ $//' >> passphrases.txt; done

```
### Phase 2 — Local Baseline Checks
- Performed local checks using tools like `cracklib` and the `zxcvbn` library to obtain initial scores and detect obvious weak patterns.
- This step helped filter out trivially weak samples before online testing.

### Phase 3 — Online Strength Analysis
- Evaluated each synthetic password using reputable online strength meters (listed above).
- Recorded score, feedback, and recommended changes from each tool for comparison and later correlation with cracking results.

### Phase 4 — Hashcat & John (Controlled Cracking Demo)
- Created hashes for selected test passwords (MD5, SHA1, NTLM as appropriate) and ran controlled cracking sessions:
  - Dictionary attacks (rockyou.txt)
  - Mask attacks for known patterns
  - Rule-based and hybrid attacks where relevant
- Documented which samples were cracked, the attack type used, and time-to-crack estimates.

### Phase 5 — Analysis & Best-Practice Identification
- Correlated results from local checks, online tools, and cracking attempts to identify patterns:
  - **Length** had the largest impact on resistance to brute-force.
  - **Passphrases** offered high strength while remaining memorable.
  - **Mixed-character long strings** resisted standard wordlists and masks.
- Compiled actionable recommendations: minimum length guidance, avoid dictionary words, use unique passwords per account, and store them in a password manager.

### Phase 6 — Tips, Documentation & Next Steps
- Consolidated practical tips:
  - Prefer long passphrases or 16+ character randomly generated passwords.
  - Enable Multi-Factor Authentication (MFA) wherever possible.
  - Avoid reusing passwords across sites.
- Saved experiment outputs, notes, and a short report for lab records.
- Suggested follow-ups: extend testing with targeted wordlists, run more rule-based cracking sessions, and practice MFA recovery flows on test accounts.

---

## ✅ Findings & Recommendations (Short)
- Password **length** is the single most important factor for resisting brute-force attacks.  
- **Passphrases** strike an excellent balance between security and memorability.  
- Use a **password manager** to store unique, high-entropy passwords and to avoid reuse.  
- Always enable **MFA** to mitigate password compromise.

---

## 📁 Files & Artifacts (Lab)
- `all_passwords.txt` — List of synthetic test passwords used in the lab.  
- `results_zxcvbn.csv` — Local strength scores and feedback (zxcvbn).  
- `cracklib_results.txt` — Local heuristic checks.  
- `results_hashcat.txt` — Hashcat output / cracked pairs.  
- `PASSWORD_LAB.md` — This document.

---

## ⚠️ Legal & Ethical Notice

All cracking demonstrations were performed on self-generated, non-production data. Do **not** run cracking tools against systems or data you do not own or have explicit permission to test. Unauthorized access or cracking attempts are illegal.
