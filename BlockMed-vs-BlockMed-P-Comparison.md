# BlockMed → BlockMed-P Evolution

## Previous BlockMed vs Proposed BlockMed-P

| বিষয় | Previous BlockMed | New BlockMed-P |
|---|---|---|
| মূল উদ্দেশ্য | Blockchain-based prescription | **Privacy + security + interoperability + fraud resistance** |
| Doctor | Prescription তৈরি | Prescription তৈরি + **digital signature** |
| Patient | Prescription receiver | **Data-access controller** |
| Pharmacy | Prescription verify | **Cryptographic + blockchain verification** |
| Blockchain | Prescription integrity | **Hash + consent + provenance + audit trail** |
| Medical data | সম্ভাব্যভাবে centralized/on-chain architecture | **Encrypted off-chain storage** |
| Access control | Basic role-based | **Patient-controlled grant/revoke** |
| Prescription forgery | Blockchain integrity | **Digital signature + hash verification** |
| Duplicate prescription | Basic/limited | **Blockchain-based dispensing-status verification** |
| Privacy | Basic | **Privacy-preserving architecture** |
| Interoperability | Standalone system | **HL7 FHIR-compatible** |
| Fraud detection | নেই/limited | **AI anomaly detection** |
| Advanced privacy | নেই | **ZKP optional/advanced layer** |
| Security evaluation | Functional testing | **Threat model + attack simulations** |
| Scalability evaluation | Limited | **100 → 50K+ transaction benchmark** |
| Performance metrics | Basic | **Latency, throughput, storage, cost** |
| Research comparison | Limited | **State-of-the-art comparison** |
| Contribution | Application | **Research framework + experiments** |

---

## Architecture Difference

### Previous BlockMed

```text
Doctor
  ↓
Prescription
  ↓
Blockchain
  ↓
Patient / Pharmacy
```

মূল contribution ছিল:

> **“Blockchain can make prescriptions tamper-resistant.”**

---

### New BlockMed-P

```text
                 Doctor
                   ↓
          Digital Signature
                   ↓
            Prescription
                   ↓
             Encryption
                   ↓
        ┌──────────┴──────────┐
        ↓                     ↓
Encrypted Off-chain      Blockchain
Medical Data             Hash + Consent
                         + Audit Trail
        │                     │
        └──────────┬──────────┘
                   ↓
             Patient Consent
                   ↓
          ┌────────┴────────┐
          ↓                 ↓
       Pharmacy          Hospital
          ↓                 ↓
       Verify          Verify
          └────────┬────────┘
                   ↓
          Fraud/Anomaly Engine
                   ↓
             Risk / Alert
```

---

## Research Positioning

### Previous Project

> **Blockchain-based secure prescription system**

### Proposed Research Version

> **A patient-controlled, privacy-preserving and interoperable prescription framework with cryptographic verification and fraud detection, validated through security, scalability and performance experiments.**

---

## Feature Priority

সব feature একসাথে ঢোকানোর পরিবর্তে research value অনুযায়ী priority রাখা হবে।

### Must-have

1. Patient-controlled consent
2. Encrypted off-chain storage
3. Digital signature
4. Duplicate/forgery prevention
5. FHIR interoperability
6. Rigorous benchmark + security evaluation

### Research-enhancing

7. AI anomaly detection

### High-risk / Advanced

8. Zero-Knowledge Proof (ZKP)

> **Design principle:** ZKP বা AI শুধু feature count বাড়ানোর জন্য যোগ করা উচিত নয়। যেসব component-এর measurable research hypothesis এবং experiment থাকবে, সেগুলোই final system-এ রাখা হবে।

---

## Existing BlockMed → BlockMed-P Migration

Existing BlockMed codebase থাকলে implementation-কে তিন ভাগে map করা হবে:

### 1. Keep

Existing modules যেগুলো BlockMed-P architecture-এর সাথে compatible, সেগুলো retain করা হবে।

### 2. Modify

Existing prescription, blockchain, authentication, role-based access এবং verification modules প্রয়োজন অনুযায়ী modify করা হবে।

### 3. Add

নতুন research requirements অনুযায়ী নতুন modules যোগ করা হবে, যেমন:

- Patient consent management
- Encrypted off-chain storage
- Digital signature service
- Prescription lifecycle enforcement
- FHIR interoperability layer
- Security attack testing
- Scalability benchmarking
- Ablation experiments
- Optional AI anomaly detection
- Optional ZKP research layer

---

## Core Research Transformation

```text
Previous BlockMed
        ↓
Blockchain + Prescription
        ↓
Tamper Resistance
        ↓
Functional Prototype
```

becomes:

```text
                    BlockMed-P
                        ↓
        ┌───────────────┼────────────────┐
        ↓               ↓                ↓
   Privacy          Security       Interoperability
        ↓               ↓                ↓
Encrypted          Signature +        FHIR-oriented
Off-chain          Hash + Consent     Representation
Storage            + Lifecycle
        └───────────────┼────────────────┘
                        ↓
               Experimental Validation
                        ↓
        ┌───────────────┼────────────────┐
        ↓               ↓                ↓
     Security       Performance      Scalability
     Experiments    Benchmarking      Evaluation
        └───────────────┼────────────────┘
                        ↓
                 Research Evidence
```

---

## Final Research Goal

The objective is to transform the existing **BlockMed** prototype from a demonstration-oriented blockchain prescription application into **BlockMed-P**, a systematically evaluated research framework focused on:

- **Privacy**
- **Patient-controlled authorization**
- **Cryptographic provenance**
- **Prescription integrity**
- **Forgery resistance**
- **Replay and duplicate-dispensing prevention**
- **FHIR-oriented interoperability**
- **Security evaluation**
- **Performance benchmarking**
- **Scalability**
- **Reproducibility**

The existing research proposal already frames BlockMed-P around encrypted off-chain clinical data, minimal blockchain state, digital signatures, patient grant/revoke control, FHIR-oriented interoperability, threat modeling, attack simulations, benchmarking, ablation studies, and reproducible artifacts.
