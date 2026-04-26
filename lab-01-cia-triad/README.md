# 🧪 Lab 01 - CIA Triad 

**Program:** Cybersecurity (3-Months Program)  
**Difficulty:** Beginner  
**Environment:** Windows 11 (Home Lab)  
**Date Completed:** 24 April 2026  

---

## 1 · GOAL

To practically demonstrate the **CIA Triad (Confidentiality, Integrity, Availability)** using simple, real-world tools and observe how each principle behaves in a controlled environment.

---

## 2 · TOOLS USED

- 7-Zip (file encryption)  
- HashCalc (SHA-256 hashing)  
- Windows Services (`services.msc`)  

---

## 3 · WHAT I DID (HIGH-LEVEL)

This lab was divided into three independent experiments:

- Tested file encryption to enforce confidentiality  
- Verified file integrity using cryptographic hashing  
- Simulated service disruption and recovery to observe availability  

Each part focused on observing system behaviour before and after changes.

---

## 4 · WHAT I SAW (OVERALL OUTCOME)

- Encrypted files could not be accessed without the correct password  
- A single-character modification completely changed the file hash  
- Stopping a service caused immediate loss of functionality, restored after restart  

These results confirmed that the CIA Triad principles are observable in real systems.

---

## 5 · WHAT THIS MEANS IN REAL LIFE

- **Confidentiality:** Protects sensitive data through encryption  
- **Integrity:** Ensures data has not been tampered with  
- **Availability:** Ensures systems remain accessible when needed  

These principles are used in banking systems, enterprise infrastructure, and cloud environments.

---

# 🔐 Part A - Confidentiality (File Encryption)

## Objective
Demonstrate how encryption prevents unauthorized access to data.

## Tools Used
- 7-Zip  

## Steps
1. Created secret.txt with sample content
2. Compressed using 7-Zip
3. Applied AES-256 encryption with password
4. Double-clicked the archive - Windows threw an extraction error
5. Opened correctly via right-click → 7-Zip → Open archive
6. Attempted access without password
7. Reopened using correct password 

## Observations
- File was completely inaccessible without password  
- Access restored instantly with correct password  

## Evidence
- screenshots/Lab01_PartA_access_denied.png
- screenshots/Lab01_PartA_access_denied.png
- screenshots/Lab01_PartA_access_granted.png  

## Real-World Meaning
Used to protect sensitive data such as financial records and customer information. If an attacker steals an encrypted file from a server, they still can't read it without the key.

---

# 🧾 Part B - Integrity (Hash Verification)

## Objective
Detect file modification using SHA-256 hashing.

## Tools Used
- HashCalc  
- SHA-256 algorithm  

## Steps
1. Generated hash of original file  
2. Saved hash output  
3. Modified file (added one character)  
4. Generated new hash  
5. Compared both hashes  

## Observations
- Hash output changed completely after minor edit  
- Demonstrated avalanche effect  

## Evidence
- screenshots/Lab01_PartB_hash_before.png  
- screenshots/Lab01_PartB_hash_after.png  
- screenshots/Lab01_PartB_comparison.png  

## Real-World Meaning
Used in software verification and download validation — this is how you confirm a file hasn't been tampered with or replaced with malware.

---

# ⚙️ Part C - Availability (Service Control)

## Objective
Observe system behaviour when a service is stopped.

## Tools Used
- Windows Services (`services.msc`)  
- Windows Update service  

## Steps
1. Opened Services manager  
2. Located Windows Update service  
3. Stopped service  
4. Attempted update check  
5. Restarted service  
6. Retested functionality  

## Observations
- Service became unavailable when stopped  
- Functionality restored after restart  

## Evidence
- screenshots/Lab01_PartC_service_stopped.png  
- screenshots/Lab01_PartC_service_restored.png  

## Real-World Meaning
Mirrors denial-of-service scenarios where systems become unavailable due to disruption.

---

# 🧠 LAB SUMMARY

## Environment
- Windows 11 (16GB RAM)  
- No VM required  

## Skills Demonstrated
- Confidentiality via encryption  
- Integrity via hashing  
- Availability via service control  

## Difficulties / Errors
When I double-clicked the encrypted archive, Windows threw an extraction error — its built-in tool doesn't support AES-256. Fixed it by opening through 7-Zip directly (right-click → 7-Zip → Open archive). Lesson: always open encrypted archives with the tool that created them.

## Key Insight
Even a single-character change completely alters a cryptographic hash, highlighting how sensitive integrity systems are.

---

## 🚀 FINAL NOTE

This lab demonstrates that the CIA Triad is not theoretical - it is directly observable using basic system tools and controlled actions.

---

## 👤 AUTHOR

Tolu Akinyele  
Cybersecurity Student | Learning in Public  
GitHub: https://github.com/CikA-triX  
LinkedIn: https://linkedin.com/in/toluakinyele
