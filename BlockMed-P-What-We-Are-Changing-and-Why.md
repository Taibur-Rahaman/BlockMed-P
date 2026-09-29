# BlockMed-P — কী কী Change করছি এবং কেন?

## 1. BlockMed-P আসলে কী?

**BlockMed-P হলো আমাদের previous BlockMed project-এর research-level evolution।**

আগের BlockMed-এর main idea ছিল:

> **Blockchain ব্যবহার করে electronic prescription-কে secure এবং verifiable করা।**

এখন আমরা project-টাকে আরও research-oriented করছি।

BlockMed-P-এর main focus হলো:

- **Privacy**
- **Security**
- **Patient Control**
- **Prescription Authenticity**
- **Interoperability**
- **Performance**
- **Scalability**
- **Experimental Validation**

সহজভাবে:

> **আগে আমরা system বানিয়েছি। এখন আমরা explain, test এবং experiment করে দেখাতে চাই systemটা কেন দরকার, কীভাবে কাজ করে এবং এর actual performance কেমন।**

---

# 2. Previous BlockMed থেকে BlockMed-P-তে কী কী Change?

| বিষয় | Previous BlockMed | BlockMed-P |
|---|---|---|
| Main focus | Blockchain-based prescription system | Privacy + Security + Patient Control + Research Evaluation |
| Medical data | Blockchain-centered | Sensitive data encrypted off-chain |
| Blockchain | Integrity / verification | Hash, commitment, consent, lifecycle, audit evidence |
| Doctor | Prescription তৈরি করে | Prescription তৈরি + digitally sign করে |
| Patient | Prescription receive করে | Data access control করতে পারে |
| Pharmacy | Prescription verify করে | Signature + hash + authorization + lifecycle verify করে |
| Access | Mainly RBAC | RBAC + Patient-controlled Grant/Revoke |
| Prescription status | Basic | Lifecycle + state enforcement |
| Forgery protection | Blockchain integrity | Digital signature + hash verification |
| Replay protection | Limited/basic | Lifecycle-based protection |
| Duplicate dispensing | Limited/basic | Dispensing-state enforcement |
| Privacy | Basic | Encrypted off-chain medical data |
| Interoperability | Standalone-oriented | FHIR-oriented |
| Security testing | Mainly functional | Threat model + attack simulation |
| Performance | Basic functionality | Latency + throughput + storage + crypto overhead |
| Scalability | Limited | 100 → 50,000+ workload testing |
| Research validation | Functional demo | Baseline + experiments + ablation + statistics |

---

# 3. Change 1 — Sensitive Medical Data Blockchain-এ রাখব না

## আগে কী ছিল?

Previous system বেশি blockchain-centered ছিল।

## এখন কী করছি?

আমরা system-টাকে দুই ভাগে ভাগ করছি।

### Sensitive Medical Data

```text
Prescription / Clinical Data
          ↓
      Encryption
          ↓
Encrypted Off-Chain Database
```

### Verification Data

```text
Hash
Signature Reference
Consent
Status
Audit Information
          ↓
      Blockchain
```

## Blockchain-এ কী থাকবে?

- Prescription ID
- Hash / Cryptographic Commitment
- Doctor/Issuer ID বা pseudonym
- Timestamp
- Signature reference
- Prescription lifecycle state
- Consent event reference
- Verification metadata
- Revocation status
- Audit ID

## Off-chain-এ কী থাকবে?

Sensitive information যেমন:

- Medication
- Dosage
- Clinical instructions
- Patient identifying information
- Diagnosis/context
- Prescription document
- Sensitive metadata

## কেন?

Medical data খুব sensitive।

Raw medical information blockchain-এ রাখলে:

- Privacy problem হতে পারে
- Storage অনেক বাড়তে পারে
- Replication overhead হতে পারে
- Scalability problem হতে পারে
- Data retention/deletion নিয়ে সমস্যা হতে পারে

তাই আমাদের main principle:

> **Blockchain should verify the medical data, not become the primary storage for raw medical data.**

---

# 4. Change 2 — Doctor-এর Digital Signature যোগ করছি

## আগে

Blockchain থেকে আমরা mainly বুঝতে পারি data change হয়েছে কিনা।

কিন্তু একটা বড় question থাকে:

> **এই prescription আসলে কি authorized doctor তৈরি করেছে?**

## এখন

Doctor prescription digitally sign করবে।

Basic flow:

```text
Prescription
      ↓
Canonicalization
      ↓
SHA-256 Hash
      ↓
Doctor Digital Signature
      ↓
Blockchain / Verification Metadata
```

Pharmacy বা verifier check করবে:

```text
Doctor signature valid?
        +
Hash match করছে?
        +
Prescription lifecycle valid?
        +
Requester authorized?
```

## কেন?

এতে আমরা আরও ভালোভাবে handle করতে পারব:

- Fake prescription
- Forged prescription
- Unauthorized modification
- Prescription provenance
- Authenticity verification
- Non-repudiation

সহজভাবে:

> **Blockchain বলছে data পরিবর্তন হয়েছে কিনা, আর Digital Signature help করছে কে prescription তৈরি করেছে সেটা prove করতে।**

---

# 5. Change 3 — Patient-Controlled Consent

এটা BlockMed-P-এর সবচেয়ে important conceptual changes-এর একটা।

## আগে

Access mainly role/permission-এর উপর নির্ভর করত।

যেমন:

```text
Doctor → Access
Pharmacy → Access
Admin → Permission
```

## এখন

Patient নিজেই decide করতে পারবে:

> **কে আমার prescription access করতে পারবে?**

Example:

```text
Patient
   ↓
Doctor-কে Access Grant
   ↓
Doctor prescription access করে
   ↓
Pharmacy-কে Access Grant
   ↓
Pharmacy prescription verify করে
   ↓
Patient চাইলে Access Revoke করে
```

Consent lifecycle:

```text
GRANT
  ↓
ACCESS
  ↓
REVOKE
```

## কেন?

কারণ patient শুধু prescription-এর receiver না।

Patient হবে:

> **Data access-এর active controller।**

এখানে:

**RBAC** বলে কোন role কী করতে পারবে।

আর:

**Patient Consent** বলে patient নিজে access allow/revoke করতে পারবে।

---

# 6. Change 4 — Prescription Lifecycle যোগ করছি

Prescription তৈরি হওয়ার পর সেটাকে permanently reusable রাখা ঠিক না।

তাই prescription-এর state থাকবে।

Example:

```text
CREATED
   ↓
SIGNED
   ↓
ACTIVE
   ↓
VERIFIED
   ↓
DISPENSED
```

## কেন?

এতে আমরা prevent/detect করতে পারব:

- Replay attack
- Duplicate dispensing
- Invalid state transition
- Unauthorized status change

Example:

যদি:

```text
Prescription = DISPENSED
```

তাহলে কেউ আবার একই prescription দিয়ে medicine নিতে চাইলে system সেটা reject করতে পারবে।

সহজভাবে:

> **Prescription শুধু একটা document না; এর একটা verifiable lifecycle থাকবে।**

---

# 7. Change 5 — FHIR-Oriented Interoperability

Healthcare-এর বিভিন্ন system সবসময় একই format ব্যবহার নাও করতে পারে।

তাই BlockMed-P-তে আমরা **FHIR-oriented representation** ব্যবহার করছি।

Relevant resource examples:

```text
Patient
Practitioner
Medication
MedicationRequest
MedicationDispense
Organization
Provenance
Consent
```

Concept:

```text
FHIR
 ↓
Structured Healthcare Information
 ↓
BlockMed-P
 ↓
Blockchain Verification
+
Consent
+
Audit
```

## কেন?

আমাদের goal হলো future healthcare system-এর সাথে structured data exchange সহজ করা।

তবে একটা important বিষয়:

> **FHIR-oriented design মানেই live hospital interoperability already proven — এমন claim আমরা এখন করব না।**

Research-এ আমরা test করব:

- FHIR transformation success
- FHIR validation success
- Information-loss rate
- Conversion latency
- Compatibility

---

# 8. Change 6 — শুধু “Secure” বলব না, Attack করে Test করব

Previous project-এ আমরা system কাজ করছে দেখাতে পারতাম।

কিন্তু research-এ শুধু:

> **“Our system is secure.”**

বললে যথেষ্ট না।

আমাদের actual attack/test করতে হবে।

## 1. Prescription Tampering

Prescription-এর information পরিবর্তন করে দেখব modification detect হয় কিনা।

Expected:

```text
TAMPER DETECTED
```

## 2. Forged Prescription

Invalid/fake digital signature ব্যবহার করে test করব।

Expected:

```text
REJECT
```

## 3. Replay Attack

পুরনো বা already-used prescription আবার ব্যবহার করার চেষ্টা।

Expected:

```text
REJECT
```

## 4. Duplicate Dispensing

একই prescription দিয়ে দ্বিতীয়বার medicine নেওয়ার চেষ্টা।

Expected:

```text
REJECT
```

## 5. Unauthorized Access

Permission ছাড়া prescription access করার চেষ্টা।

Expected:

```text
REJECT
```

## 6. Privilege Escalation

Low-privilege user higher-privilege action করার চেষ্টা করবে।

Expected:

```text
REJECT
```

## 7. Unauthorized State Change

Invalid lifecycle state change করার চেষ্টা।

Expected:

```text
REJECT
```

---

# 9. Change 7 — Performance Measure করব

শুধু secure হলেই system ভালো বলা যাবে না।

System যদি অনেক slow হয়, তাহলে practical problem হবে।

তাই আমরা measure করব:

| Metric | কী জানতে চাই? |
|---|---|
| Transaction Latency | Blockchain operation complete হতে কত সময় লাগে? |
| Verification Latency | Prescription verify করতে কত সময় লাগে? |
| Throughput / TPS | প্রতি সময় কত transaction handle করতে পারে? |
| API Response Time | Application কত দ্রুত response দেয়? |
| Crypto Overhead | Encryption/signature-এর extra cost কত? |
| Smart Contract Cost | Blockchain operation-এর cost কত? |
| Storage Overhead | System-এর storage কত বাড়ে? |

**Important:** এগুলো measured result না হওয়া পর্যন্ত আমরা কোনো number claim করব না।

---

# 10. Change 8 — Scalability Test করব

System শুধু 10টা prescription handle করতে পারলেই যথেষ্ট না।

তাই proposed workload হবে:

```text
100
 ↓
1,000
 ↓
5,000
 ↓
10,000
 ↓
25,000
 ↓
50,000+
```

প্রতিটি workload-এ measure করব:

- Latency
- Throughput
- Storage growth
- Blockchain growth
- Verification performance
- API performance

## কেন?

আমরা দেখতে চাই:

> **Prescription সংখ্যা বাড়ার সাথে সাথে system-এর performance কীভাবে change করে।**

Important:

> **100 থেকে 50,000+ হলো proposed experimental workload, already-measured result না।**

---

# 11. Change 9 — Baseline Comparison

Research-এ শুধু নিজের system-এর result দেখালে যথেষ্ট না।

আমাদের comparison দরকার।

## Baseline A — Centralized System

```text
Central Database
      +
Application Authentication
```

## Baseline B — Blockchain-Oriented System

এখানে substantially বেশি prescription representation blockchain-এ store/anchor করা হবে।

## Proposed — BlockMed-P

```text
Encrypted Off-Chain Data
        +
Minimal On-Chain Commitment
        +
Digital Signature
        +
Patient Consent
        +
Lifecycle Enforcement
        +
FHIR-Oriented Representation
```

## কেন?

এতে আমরা compare করতে পারব:

> **BlockMed-P কী security, privacy, storage এবং performance-এর মধ্যে কী ধরনের trade-off তৈরি করছে?**

---

# 12. Change 10 — Ablation Study

Ablation Study মানে:

> **একটা একটা component remove করে দেখি system-এর performance/security কীভাবে change হয়।**

### Full System

```text
Encryption
+
Signature
+
Consent
+
Blockchain
+
Lifecycle
+
FHIR
```

তারপর variants:

```text
A — Full System
B — Consent বাদ
C — Digital Signature বাদ
D — Encryption বাদ
E — Blockchain Commitment বাদ
F — Lifecycle বাদ
G — Interoperability বাদ
```

তারপর compare করব:

- Security
- Latency
- Throughput
- Storage
- Verification cost
- Interoperability

## কেন?

এতে আমরা scientifically answer করতে পারব:

> **“কোন component আসলে কতটা contribution করছে?”**

অর্থাৎ শুধু feature add করব না।

**Feature-এর actual impact measure করব।**

---

# 13. সবচেয়ে বড় Change — Project → Research

এটাই পুরো BlockMed-P-এর main story।

## Previous BlockMed

```text
Problem
   ↓
Build System
   ↓
Blockchain
   ↓
Demo
```

## BlockMed-P

```text
Healthcare Problem
       ↓
Research Gap
       ↓
Threat Model
       ↓
Architecture
       ↓
Implementation
       ↓
Security Testing
       ↓
Baseline Comparison
       ↓
Performance Testing
       ↓
Scalability Testing
       ↓
Ablation Study
       ↓
Statistical Analysis
       ↓
Evidence
       ↓
Conclusion
```

### সহজ ভাষায়

আগে:

> **“আমরা একটা blockchain prescription system বানিয়েছি।”**

এখন:

> **“আমরা একটা privacy-preserving এবং patient-controlled prescription architecture design করছি এবং experiment-এর মাধ্যমে এর security, performance, scalability এবং interoperability evaluate করছি।”**

---

# 14. কোনগুলো এখন Core?

## Core BlockMed-P

```text
1. Hybrid On-chain / Off-chain Architecture
2. Encrypted Off-chain Medical Data
3. Digital Signature
4. Cryptographic Hash / Commitment
5. Patient-Controlled Consent
6. RBAC
7. Prescription Lifecycle
8. Replay / Duplicate Protection
9. FHIR-Oriented Representation
10. Threat Model
11. Security Attack Testing
12. Performance Evaluation
13. Scalability Evaluation
14. Baseline Comparison
15. Ablation Study
16. Reproducible Experiments
```

---

# 15. কোনগুলো Optional?

## AI / Anomaly Analysis

AI/anomaly analysis research-enhancing feature হতে পারে।

কিন্তু এটাকে core contribution বলার আগে আমাদের define করতে হবে:

- কোন algorithm?
- কোন data/workload?
- কোন anomaly detect করবে?
- কোন evaluation metric?
- কত accuracy/precision/recall?
- Baseline কী?

তাই:

> **AI = Optional / Research-Enhancing**

---

## ZKP — Zero-Knowledge Proof

ZKP advanced privacy feature হতে পারে।

কিন্তু এখনই এটাকে main feature হিসেবে claim না করাই ভালো।

কারণ আগে:

- Concrete use case
- Implementation
- Performance measurement
- Privacy evaluation

দরকার।

তাই:

> **ZKP = Future / Experimental Extension**

---

# 16. কী কী এখনো Claim করব না?

Research scientifically clean রাখতে আমরা এমন কিছু claim করব না যেগুলো এখনো experiment দিয়ে prove করা হয়নি।

### এখনই claim করব না:

- Live hospital interoperability
- Production-level scalability
- Clinical deployment readiness
- Complete/absolute privacy
- Zero security vulnerabilities
- AI fraud detection proven
- ZKP production-ready
- Measured performance results

### বরং বলব:

- **FHIR-oriented**
- **Proposed architecture**
- **To be experimentally evaluated**
- **Security testing planned**
- **Scalability to be measured**
- **ZKP as a future/experimental extension**

---

# 17. পুরো Research Story একসাথে

```text
Previous BlockMed
      ↓
Blockchain Prescription System
      ↓
Research Limitations Identify
      ↓
Privacy Problem
      ↓
Authenticity Problem
      ↓
Patient-Control Problem
      ↓
Replay / Duplicate Problem
      ↓
Interoperability Problem
      ↓
Performance / Scalability Questions
      ↓
          BLOCKMED-P
      ↓
Hybrid Architecture
      +
Encryption
      +
Digital Signature
      +
Patient Consent
      +
Lifecycle
      +
FHIR-Oriented Design
      ↓
Security Experiments
      +
Performance Experiments
      +
Scalability Experiments
      +
Ablation Study
      +
Baseline Comparison
      ↓
Evidence-Based Research Conclusion
```

---

# 18. One-Line Definition

> **BlockMed-P হলো এমন একটি privacy-preserving এবং patient-controlled electronic prescription framework, যেখানে sensitive medical data encrypted অবস্থায় off-chain থাকে, আর blockchain ব্যবহার করা হয় integrity, provenance, consent, lifecycle এবং audit evidence verify করার জন্য; এরপর security, performance, scalability এবং interoperability experiment-এর মাধ্যমে system-টি evaluate করা হয়।**

---

# 19. Teacher-এর সামনে সহজে বলার মতো Version

> **Sir, previous BlockMed ছিল mainly একটা functional blockchain-based electronic prescription system। এখন আমরা সেটাকে BlockMed-P হিসেবে research-level project-এ evolve করছি।**
>
> **First**, sensitive medical data blockchain-এ না রেখে encrypted off-chain storage ব্যবহার করছি। Blockchain-এ শুধু প্রয়োজনীয় verification এবং audit information রাখছি।
>
> **Second**, doctor-এর digital signature যোগ করছি, যাতে prescription আসলেই authorized doctor তৈরি করেছে কিনা verify করা যায়।
>
> **Third**, patient-controlled consent যোগ করছি, যাতে patient decide করতে পারে কে তার prescription access করতে পারবে এবং প্রয়োজন হলে access revoke করতে পারে।
>
> **Fourth**, prescription lifecycle যোগ করছি, যাতে replay এবং duplicate dispensing-এর মতো problem handle করা যায়।
>
> **Fifth**, FHIR-oriented representation ব্যবহার করছি, যাতে future healthcare systems-এর সাথে structured data exchange করা সহজ হয়।
>
> **Finally**, শুধু system কাজ করছে সেটা দেখাব না। আমরা tampering, forgery, replay এবং unauthorized access-এর মতো security attack test করব; latency, throughput, storage এবং scalability measure করব; এবং ablation study দিয়ে দেখব কোন component কতটা contribution করছে।
>
> **So, মূল পরিবর্তন হলো — previous BlockMed ছিল mainly functional blockchain project, আর BlockMed-P হচ্ছে privacy, security, interoperability এবং experimental evaluation-focused research framework।**

---

# 20. Core Research Question

> **Can a hybrid blockchain architecture provide verifiable prescription integrity, provenance, patient-controlled authorization and auditability while keeping sensitive clinical data off-chain, without introducing unacceptable performance, storage and scalability overhead?**

সহজভাবে:

> **“Sensitive medical data private রেখে blockchain-এর verification, patient control এবং audit সুবিধা নেওয়া যাবে কি, এবং এতে performance/scalability-এর overhead কতটা হবে?”**

এই question-এর সাথে পুরো research connect করবে:

```text
Privacy
+
Security
+
Patient Control
+
Blockchain
+
Interoperability
+
Performance
+
Scalability
```

---

## Final Idea

**BlockMed-P-এর story এক লাইনে:**

> **Old BlockMed → Working Prototype**
>
> **BlockMed-P → Research Framework + Experimental Evidence**

অর্থাৎ আমরা শুধু **আরও feature add করছি না**।

আমরা previous system-এর limitations identify করে:

**Architecture → Security → Privacy → Patient Control → Interoperability → Testing → Measurement → Evidence**

এই পুরো research pipeline তৈরি করছি।
