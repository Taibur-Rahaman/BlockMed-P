---
title: "BlockMed-P: A Privacy-Preserving and Interoperable Blockchain Framework for Secure, Patient-Controlled Electronic Prescription Management"
author: "Md Taibur Rahaman"
date: "2026"
geometry: margin=1in
fontsize: 11pt
---

# BlockMed-P

## A Privacy-Preserving and Interoperable Blockchain Framework for Secure, Patient-Controlled Electronic Prescription Management

**Research Proposal / Extended System Design**

**Project:** BlockMed — Blockchain-Based Prescription System  
**Proposed Research Version:** BlockMed-P  
**Repository:** `Taibur-Rahaman/BlockMed--Blockchain-Prescription-System`

---

## Abstract

Electronic prescriptions improve the speed and accessibility of healthcare delivery, but conventional centralized systems can expose prescription records to unauthorized modification, fragmented data silos, weak auditability, and inconsistent verification between healthcare providers and pharmacies. Blockchain can provide tamper-evident auditability, yet storing sensitive medical information directly on-chain creates substantial privacy, scalability, and storage problems.

This proposal introduces **BlockMed-P**, a privacy-preserving and interoperable blockchain framework for electronic prescription management. The proposed architecture separates sensitive clinical data from blockchain state: prescription content is encrypted and stored off-chain, while cryptographic commitments, authorization events, consent state, prescription status, and audit evidence are recorded on a permissioned blockchain layer. Patients are given explicit control over access grants and revocations, while doctors and pharmacies use authenticated digital identities and role-based authorization. Cryptographic signatures allow prescription provenance to be verified, and a standardized interoperability layer based on **HL7 FHIR concepts** is designed to support exchange with external healthcare systems.

The framework will be evaluated experimentally against a conventional centralized baseline and a blockchain-oriented baseline. Evaluation will cover integrity, unauthorized-access resistance, prescription forgery and replay resistance, latency, throughput, storage overhead, transaction cost, scalability, and interoperability. The study will also include threat modeling, attack simulations, ablation experiments, and reproducibility artifacts.

**Keywords:** blockchain, electronic prescription, healthcare security, privacy, patient consent, smart contracts, digital signatures, FHIR, interoperability, auditability.

---

# 1. Introduction

Healthcare information systems increasingly depend on electronic prescriptions and digital clinical workflows. A prescription is a security-sensitive clinical artifact whose authenticity, integrity, authorization, and lifecycle status must be established.

Traditional centralized prescription systems provide efficient storage and access control, but a single administrative or technical domain can become a point of failure. Records may be altered without a sufficiently transparent audit trail, organizations may maintain incompatible data formats, and patients may have limited visibility into who accessed their information.

Blockchain provides a tamper-evident distributed ledger and programmable authorization rules. However, a naïve design that stores complete medical records on-chain is unsuitable for sensitive healthcare data because medical information is confidential, blockchain data is difficult to remove, and replicated storage increases cost and scalability pressure.

**BlockMed-P** addresses this design tension through a hybrid architecture:

1. Sensitive prescription data remains encrypted off-chain.
2. Blockchain stores minimal verifiable metadata and audit evidence.
3. The prescribing clinician signs the prescription.
4. Patients can grant and revoke access.
5. Pharmacies can verify authenticity and lifecycle status.
6. Interoperable representations are designed using FHIR-compatible concepts.
7. Security and performance are evaluated experimentally rather than assumed.

---

# 2. Problem Statement

Existing electronic prescription workflows may face several related problems:

- Weak or fragmented provenance verification.
- Unauthorized access to sensitive prescription data.
- Insufficient patient visibility over data sharing.
- Prescription tampering or forged prescription artifacts.
- Replay or duplicate-dispensing risks.
- Fragmented healthcare information systems.
- Limited cross-system interoperability.
- Centralized audit logs that require trust in one operator.
- Scalability and storage problems when blockchain is used for complete medical records.

### Research Problem

> **How can a blockchain-supported electronic prescription framework provide verifiable integrity and provenance while preserving sensitive-data privacy, enabling patient-controlled access, supporting healthcare interoperability, and remaining practical under realistic performance constraints?**

---

# 3. Research Gap

The proposed study focuses on the gap between three commonly separated objectives:

| Requirement | Conventional Centralized Approach | Naïve Blockchain Approach | BlockMed-P |
|---|---|---|---|
| Tamper-evident auditability | Operator dependent | Strong | Strong |
| Sensitive-data privacy | Application dependent | Weak if data is on-chain | Encrypted off-chain |
| Patient-controlled consent | Often organization-centric | Not inherent | Explicit grant/revoke |
| Prescription provenance | Application dependent | Ledger-assisted | Signature + commitment |
| Replay / duplicate protection | Application dependent | Possible | Explicit lifecycle state |
| Interoperability | System dependent | Not inherent | FHIR-oriented |
| Scalability | Generally high | Potential bottleneck | Minimal on-chain state |
| Empirical security evaluation | Variable | Often conceptual | Threat + attack experiments |

The research gap is therefore positioned at the **architecture and empirical-validation level**, rather than claiming that blockchain itself is novel.

The study investigates whether combining privacy-preserving storage, patient consent, cryptographic provenance, lifecycle enforcement, and interoperability produces measurable benefits at acceptable overhead.

---

# 4. Research Objectives

## 4.1 Primary Objective

To design and experimentally evaluate a privacy-preserving, patient-controlled, interoperable blockchain framework for secure electronic prescription management.

## 4.2 Specific Objectives

1. Design a hybrid on-chain/off-chain prescription architecture.
2. Implement cryptographic prescription provenance using digital signatures and hashes.
3. Implement patient-controlled access grant and revocation.
4. Implement role-based authorization for patients, doctors, pharmacies, and administrators.
5. Prevent prescription replay and unauthorized status transitions.
6. Design a FHIR-compatible interoperability representation.
7. Develop a threat model covering realistic prescription-system attacks.
8. Benchmark latency, throughput, storage, transaction cost, and scalability.
9. Conduct ablation experiments to measure the contribution of major components.
10. Produce reproducible implementation and evaluation artifacts.

---

# 5. Research Questions

### RQ1

Can blockchain-based commitments and digital signatures provide reliable prescription integrity and provenance verification?

### RQ2

Can sensitive prescription content remain private while its integrity and lifecycle remain independently verifiable?

### RQ3

How effectively does patient-controlled authorization reduce unauthorized access compared with an organization-controlled baseline?

### RQ4

What performance overhead is introduced by the proposed security and consent mechanisms?

### RQ5

How does the proposed hybrid architecture scale as prescription transaction volume increases?

### RQ6

To what extent can a FHIR-oriented representation improve interoperability without requiring sensitive clinical content to be stored on-chain?

---

# 6. Proposed Contributions

The research is expected to contribute:

1. **Hybrid privacy-preserving architecture** separating encrypted clinical data from blockchain verification state.
2. **Patient-controlled consent management** with auditable grant and revoke events.
3. **Cryptographic prescription provenance** using digital signatures and cryptographic commitments.
4. **Prescription lifecycle protection** against unauthorized state changes, replay, and duplicate processing.
5. **FHIR-oriented interoperability design** for structured exchange with external healthcare systems.
6. **Formalized threat model** and security test suite.
7. **Quantitative evaluation framework** covering security, performance, storage, scalability, and interoperability.
8. **Reproducible research artifacts** including source code, experiment configurations, synthetic-data generation, and evaluation scripts.

---

# 7. Proposed System Architecture

## 7.1 Architecture Overview

The proposed system follows a layered hybrid architecture.

Sensitive clinical content is encrypted and stored off-chain. The blockchain stores only the minimum state required for integrity verification, authorization, provenance, consent evidence, lifecycle management, and auditability.

```mermaid
flowchart TB
    P[Patient]
    D[Doctor]
    PH[Pharmacy]
    H[Hospital / EHR]

    API[API / Application Gateway]
    IAM[Identity & Access Management]
    PS[Prescription Service]
    FHIR[FHIR Interoperability Layer]

    SIG[Digital Signature Service]
    ENC[Encryption / Key Management]
    CONS[Consent Engine]

    SC[Smart Contracts]
    BC[(Permissioned Blockchain)]
    DB[(Encrypted Off-Chain Storage)]

    AUD[Audit & Verification Engine]
    ANOM[Security / Anomaly Analysis]

    P --> API
    D --> API
    PH --> API
    H --> FHIR
    FHIR --> API

    API --> IAM
    API --> PS

    PS --> SIG
    PS --> ENC
    API --> CONS

    SIG --> SC
    CONS --> SC
    SC --> BC

    ENC --> DB

    BC --> AUD
    DB --> AUD
    AUD --> ANOM
````

**Figure 1.** High-level BlockMed-P architecture.

---

## 7.2 Architectural Principles

The architecture follows these principles:

- **Data minimization:** raw clinical data is not placed on-chain. 
- **Least privilege:** users receive only the permissions required for their role. 
- **Patient control:** patient consent is an explicit authorization mechanism. 
- **Cryptographic provenance:** prescriptions are digitally signed and hash-anchored. 
- **Lifecycle enforcement:** prescriptions cannot arbitrarily move between states. 
- **Interoperability:** healthcare information is represented through FHIR-oriented structures. 
- **Auditability:** security-sensitive events create verifiable audit evidence. 
- **Reproducibility:** architecture and evaluation parameters are documented. 

---

# 8. Prescription Lifecycle

```
```

**Figure 2.** End-to-end prescription lifecycle.

---

# 9. Prescription State Model

```
```

**Figure 3.** Prescription and authorization state machine.

The state machine prevents arbitrary lifecycle transitions. For example, a prescription that has already been dispensed should not be accepted as a new dispensing transaction.

---

# 10. On-Chain and Off-Chain Data Model

## 10.1 On-Chain Data

Only minimal data required for verification and auditability should be stored on-chain:

-  Prescription identifier. 
-  Cryptographic hash / commitment. 
-  Issuing provider identifier or pseudonymous identifier. 
-  Timestamp. 
-  Signature reference. 
-  Prescription lifecycle state. 
-  Consent event references. 
-  Access / verification event metadata. 
-  Revocation status. 
-  Audit-event identifiers. 

## 10.2 Off-Chain Data

Sensitive information remains encrypted off-chain:

-  Medication details. 
-  Dosage. 
-  Clinical instructions. 
-  Patient-identifying information. 
-  Diagnosis or clinical context where applicable. 
-  Prescription document representation. 
-  Other sensitive clinical metadata. 

> **Design rule:** The blockchain should not become the primary repository for raw clinical data.

---

# 11. Identity and Access Control

The framework defines the following logical roles:

| Role           | Core Responsibility                                                  |
| -------------- | -------------------------------------------------------------------- |
| Patient        | Controls consent and views prescription history                      |
| Doctor         | Creates and signs prescriptions                                      |
| Pharmacy       | Verifies and processes prescriptions                                 |
| Hospital / EHR | Exchanges authorized healthcare information                          |
| Administrator  | Operates infrastructure without unrestricted clinical-data authority |

Authorization follows the principle of least privilege.

## 11.1 Consent Lifecycle

```
```

```
Prescription Created
        |
        v
Prescription Signed
        |
        v
Prescription Active
        |
        +------> Patient Grants Access
        |               |
        |               v
        |        Authorized Verification
        |               |
        |               v
        |           Dispensing
        |
        +------> Patient Revokes Access
                        |
                        v
                 Access Restricted
```

---

# 12. Cryptographic Integrity and Provenance

A prescription payload can be represented conceptually as:

```math
M = Canonicalize(Prescription)
```

```math
H = SHA256(M)
```

The authorized clinician signs the prescription commitment:

```math
\sigma = Sign_{Doctor}(H)
```

The verifier checks:

```math
Verify_{Doctor}(H,\sigma)=True
```

The system then compares the locally computed commitment with the blockchain commitment:

```math
H_{local}=H_{chain}
```

A successful verification therefore requires:

1.  A valid issuer signature. 
2.  A matching cryptographic commitment. 
3.  A valid lifecycle state. 
4.  Valid authorization / consent where required. 

This provides stronger provenance evidence than relying on an editable database record alone.

---

# 13. Privacy Model

BlockMed-P follows a **data-minimization** principle.

### Sensitive Data

Stored encrypted off-chain.

### Verification Data

Stored on-chain only when necessary for integrity, authorization, provenance, or audit.

### Access

Controlled through authenticated identities, role-based permissions, and patient consent.

### Metadata

Transaction identifiers, timestamps, and access patterns may still reveal information. Metadata leakage is therefore treated as an explicit research limitation.

## 13.1 Advanced Privacy Extension

A future or experimental module may investigate **zero-knowledge proofs (ZKP)** for limited claims, such as proving authorization or prescription validity without exposing unnecessary underlying information.

ZKP should be evaluated experimentally rather than assumed to be efficient or automatically privacy-preserving in every deployment.

---

# 14. Interoperability Strategy

The proposed interoperability layer uses **FHIR-oriented resources and mappings**.

Potential mappings include:

- `Patient` 
- `Practitioner` 
- `Medication` 
- `MedicationRequest` 
- `MedicationDispense` 
- `Organization` 
- `Provenance` 
- `Consent` 

The relationship is:

```
```

```
FHIR
  |
  |  Structured healthcare information
  v
Interoperability Layer
  |
  +-----------------------------+
  |                             |
  v                             v
Encrypted Clinical Data     Blockchain Evidence
                              |
                              +-- Integrity
                              +-- Provenance
                              +-- Consent
                              +-- Lifecycle
                              +-- Audit
```

> **FHIR describes interoperable healthcare information; blockchain provides verifiable state, provenance, consent evidence, and auditability.**

---

# 15. Security and Threat Model

The evaluation should consider realistic threats.

| Threat                     | Security Property           | Proposed Defense                     |
| -------------------------- | --------------------------- | ------------------------------------ |
| Prescription tampering     | Integrity                   | Hash + signature                     |
| Forged prescription        | Authenticity                | Issuer signature verification        |
| Replay of old prescription | Freshness                   | Unique ID + status / nonce           |
| Duplicate dispensing       | Lifecycle integrity         | Smart-contract state                 |
| Unauthorized access        | Confidentiality             | Encryption + RBAC + consent          |
| Privilege escalation       | Authorization               | Role checks                          |
| Audit manipulation         | Accountability              | Append-only ledger evidence          |
| Database compromise        | Confidentiality / Integrity | Encryption + commitment verification |
| Key misuse                 | Authenticity                | Key management + rotation            |
| Malicious API request      | Authorization               | Authenticated API + contract checks  |

## 15.1 Security Properties

The target properties are:

-  Confidentiality. 
-  Integrity. 
-  Authenticity. 
-  Authorization. 
-  Accountability. 
-  Freshness. 
-  Non-repudiation. 
-  Auditability. 

---

# 16. Experimental Methodology

## 16.1 Baselines

### Baseline A — Centralized System

A conventional database and application-level authorization architecture.

### Baseline B — Blockchain-Oriented System

A blockchain design that stores or anchors a substantially larger prescription representation directly on-chain.

### Proposed System — BlockMed-P

Encrypted off-chain prescription data combined with:

-  Minimal on-chain commitments. 
-  Digital signatures. 
-  Patient consent. 
-  Lifecycle enforcement. 
-  FHIR-oriented interoperability. 

The comparison should measure both security properties and system overhead.

---

# 17. Evaluation Metrics

## 17.1 Security Metrics

-  Unauthorized-access success rate. 
-  Tampered-prescription detection rate. 
-  Forged-signature rejection rate. 
-  Replay rejection rate. 
-  Duplicate-dispensing rejection rate. 
-  Unauthorized state-transition rejection rate. 

## 17.2 Performance Metrics

-  Transaction latency. 
-  End-to-end verification latency. 
-  Throughput in transactions per second. 
-  API response time. 
-  Smart-contract execution cost. 
-  Cryptographic processing overhead. 

## 17.3 Storage Metrics

-  On-chain bytes per prescription. 
-  Off-chain storage per prescription. 
-  Total storage growth. 
-  Blockchain growth under increasing workloads. 

## 17.4 Scalability

Suggested workload levels:

-  100 transactions. 
-  1,000 transactions. 
-  5,000 transactions. 
-  10,000 transactions. 
-  25,000 transactions. 
-  50,000+ transactions. 

Actual limits should be determined experimentally.

## 17.5 Interoperability Metrics

Measure:

-  FHIR transformation success rate. 
-  FHIR validation success rate. 
-  Information-loss rate. 
-  Conversion latency. 
-  Compatibility with selected external test systems. 

---

# 18. Ablation Study

To demonstrate which components produce measurable benefits, controlled experiments will remove individual components.

| Configuration | Removed Component      |
| ------------- | ---------------------- |
| A             | None — Full BlockMed-P |
| B             | Patient consent        |
| C             | Digital signatures     |
| D             | Encryption             |
| E             | Blockchain commitment  |
| F             | Lifecycle protection   |
| G             | Interoperability layer |

The study should report how each removal affects:

-  Security. 
-  Latency. 
-  Throughput. 
-  Storage. 
-  Interoperability. 
-  Verification cost. 

This prevents the paper from claiming that every component is necessary without empirical evidence.

---

# 19. Statistical Evaluation

Where repeated measurements are available:

-  Run multiple independent trials. 
-  Report mean and standard deviation. 
-  Report median and percentile latency where appropriate. 
-  Report confidence intervals. 
-  Use suitable statistical tests when comparing groups. 
-  Report effect sizes where applicable. 
-  Document hardware and software configuration. 
-  Document workload and transaction parameters. 

---

# 20. Research Workflow

```
```

**Figure 4.** Research methodology workflow.

---

# 21. Implementation Architecture

A prototype can be organized into the following layers:

```
```

```
Presentation Layer
        |
        v
Application / API Layer
        |
  +-----+----------------+
  |     |                |
  v     v                v
 IAM  Prescription     FHIR
      Service          Adapter
        |
  +-----+------------------+
  |                        |
  v                        v
Crypto Service          Smart Contracts
  |                        |
  v                        v
Encrypted DB           Blockchain
        |
        v
Audit / Evaluation Layer
```

Suggested implementation technologies can include the existing React/Vite frontend and Solidity/Hardhat blockchain stack. The exact production architecture should remain modular enough to support alternative databases, identity providers, and blockchain networks.

---

# 22. Research Data Model

## 22.1 Prescription Object

```
```

```
{
  "prescriptionId": "PRESCRIPTION-UUID",
  "patientRef": "PSEUDONYMOUS-ID",
  "practitionerRef": "PRACTITIONER-ID",
  "medicationRef": "MEDICATION-ID",
  "issuedAt": "TIMESTAMP",
  "expiresAt": "TIMESTAMP",
  "status": "ACTIVE",
  "contentHash": "SHA256",
  "signature": "DIGITAL-SIGNATURE"
}
```

## 22.2 Consent Event

```
```

```
{
  "consentId": "CONSENT-UUID",
  "patientRef": "PSEUDONYMOUS-ID",
  "grantee": "PHARMACY-ID",
  "scope": "PRESCRIPTION-VERIFICATION",
  "action": "GRANT",
  "issuedAt": "TIMESTAMP",
  "expiresAt": "TIMESTAMP"
}
```

---

# 23. Reproducibility Strategy

The repository should contain:

```
```

```
BlockMed/
├── README.md
├── PROPOSAL.md
├── docs/
│   ├── architecture.md
│   ├── methodology.md
│   └── evaluation.md
├── figures/
├── contracts/
├── frontend/
├── backend/
├── tests/
├── experiments/
├── datasets/
└── references/
    └── references.bib
```

Experiments should record:

-  Software versions. 
-  Blockchain configuration. 
-  Machine specifications. 
-  Dataset or synthetic-data generation parameters. 
-  Transaction workload. 
-  Random seeds where applicable. 
-  Test commands. 
-  Raw measurements. 
-  Processed results. 
-  Plotting scripts. 

No real patient-identifying information should be used in public experiments.

---

# 24. Ethical and Privacy Considerations

Healthcare data is highly sensitive.

The research prototype should:

-  Use synthetic or appropriately de-identified data. 
-  Minimize personally identifiable information. 
-  Avoid placing raw clinical data on public ledgers. 
-  Document key-management assumptions. 
-  Define data retention and deletion behavior for off-chain data. 
-  Clearly separate research prototype claims from clinical deployment claims. 

If human-subject data are introduced later, required institutional and ethical approvals must be obtained before collection or analysis.

---

# 25. Expected Results and Hypotheses

The study does **not** assume that BlockMed-P will outperform every baseline in every metric.

The following hypotheses will be tested:

### H1

Cryptographic commitments and digital signatures will detect unauthorized prescription modification and forgery.

### H2

Encrypted off-chain storage will substantially reduce on-chain storage compared with storing complete prescription data on-chain.

### H3

Patient-controlled consent will reduce unauthorized access paths relative to a baseline without explicit patient-controlled authorization.

### H4

Minimal on-chain state will improve scalability relative to a design that stores complete prescription payloads on-chain.

### H5

The security and privacy mechanisms will introduce measurable computational and latency overhead.

### H6

FHIR-oriented representation will enable structured interoperability while preserving the separation between clinical content and blockchain verification state.

These hypotheses must be validated experimentally.

---

# 26. Novelty Positioning

The paper should avoid the weak claim:

> "Blockchain makes healthcare secure."

Instead, the research contribution should be framed as:

> **An experimentally validated architecture for privacy-preserving electronic prescription management that combines minimal on-chain commitments, encrypted off-chain clinical data, patient-controlled consent, cryptographic provenance, lifecycle enforcement, and interoperable healthcare representation, followed by systematic security, scalability, and ablation evaluation.**

The novelty should be demonstrated through:

1.  A rigorous literature review. 
2.  Explicit comparison with prior systems. 
3.  Clearly defined research gaps. 
4.  Quantitative baseline comparison. 
5.  Security attack experiments. 
6.  Scalability experiments. 
7.  Ablation studies. 
8.  Reproducible artifacts. 

---

# 27. Proposed Manuscript Structure

1.  Introduction 
2.  Background and Related Work 
3.  Research Gap and Problem Formulation 
4.  System and Threat Model 
5.  BlockMed-P Architecture 
6.  Cryptographic and Consent Mechanisms 
7.  Interoperability Design 
8.  Security Analysis 
9.  Experimental Setup 
10.  Results 
11.  Ablation and Scalability Analysis 
12.  Discussion 
13.  Limitations and Ethical Considerations 
14.  Reproducibility 
15.  Conclusion 

---

# 28. Success Criteria

The project should be considered research-ready only after the following are demonstrated experimentally:

-  Prescription integrity verification. 
-  Signature authenticity verification. 
-  Replay prevention. 
-  Duplicate-dispensing prevention. 
-  Patient consent grant. 
-  Patient consent revocation. 
-  Encrypted off-chain storage. 
-  Blockchain audit trail. 
-  FHIR-oriented exchange. 
-  Security attack tests. 
-  Baseline comparison. 
-  Scalability benchmark. 
-  Ablation study. 
-  Reproducible experiment scripts. 
-  Complete results tables and figures. 

---

# 29. Conclusion

BlockMed-P proposes a hybrid approach to secure electronic prescription management in which blockchain is used selectively for verifiable state, provenance, consent evidence, and auditability rather than as a repository for sensitive medical records.

By combining encrypted off-chain storage, digital signatures, patient-controlled consent, lifecycle enforcement, and FHIR-oriented interoperability, the framework targets the practical tension between security, privacy, interoperability, and scalability.

The central research question is empirical: whether these mechanisms provide measurable security and privacy benefits at an acceptable performance and storage cost. Therefore, the proposed evaluation emphasizes baselines, threat-driven attack experiments, scalability benchmarks, ablation studies, statistical evaluation, and reproducibility.

The goal is to transform the existing BlockMed prototype from a demonstration-oriented blockchain application into a rigorously evaluated research platform.

---

# Appendix A — Figure Inventory

| Figure   | Purpose                          | Recommended Format            |
| -------- | -------------------------------- | ----------------------------- |
| Figure 1 | Complete BlockMed-P architecture | SVG/PDF + Mermaid source      |
| Figure 2 | Prescription lifecycle           | Sequence diagram              |
| Figure 3 | Authorization/state machine      | State diagram                 |
| Figure 4 | Research workflow                | Flowchart                     |
| Figure 5 | Threat model                     | Security architecture diagram |
| Figure 6 | Experimental evaluation pipeline | Flowchart                     |

---

# Appendix B — Experimental Result Tables

## B.1 Security Results

| Attack               | Baseline A | Baseline B | BlockMed-P | Detection / Rejection Rate |
| -------------------- | ---------- | ---------- | ---------- | -------------------------- |
| Tampering            | TBD        | TBD        | TBD        | TBD                        |
| Forgery              | TBD        | TBD        | TBD        | TBD                        |
| Replay               | TBD        | TBD        | TBD        | TBD                        |
| Duplicate dispensing | TBD        | TBD        | TBD        | TBD                        |
| Unauthorized access  | TBD        | TBD        | TBD        | TBD                        |
| Revoked access       | TBD        | TBD        | TBD        | TBD                        |

## B.2 Performance Results

| Workload | Architecture | Avg. Latency | P95 Latency | Throughput | Storage |
| -------- | ------------ | ------------ | ----------- | ---------- | ------- |
| 100      | Baseline A   | TBD          | TBD         | TBD        | TBD     |
| 100      | Baseline B   | TBD          | TBD         | TBD        | TBD     |
| 100      | BlockMed-P   | TBD          | TBD         | TBD        | TBD     |
| 1,000    | Baseline A   | TBD          | TBD         | TBD        | TBD     |
| 1,000    | Baseline B   | TBD          | TBD         | TBD        | TBD     |
| 1,000    | BlockMed-P   | TBD          | TBD         | TBD        | TBD     |
| 10,000   | Baseline A   | TBD          | TBD         | TBD        | TBD     |
| 10,000   | Baseline B   | TBD          | TBD         | TBD        | TBD     |
| 10,000   | BlockMed-P   | TBD          | TBD         | TBD        | TBD     |

> **Note:** `TBD` values must be replaced only with measured experimental results.

---

# Appendix C — Threat Model Summary

```
```

```
                  ┌───────────────────┐
                  │ External Attacker │
                  └─────────┬─────────┘
                            |
              +-------------+-------------+
              |             |             |
              v             v             v
          API Attack    Data Tamper    Replay
              |             |             |
              +-------------+-------------+
                            |
                            v
                    BlockMed-P System
                            |
             +--------------+--------------+
             |                             |
             v                             v
       Off-chain Store              Blockchain Layer
       Encryption                   Hash / Signature
       Access Control               Consent / State
             |                             |
             +--------------+--------------+
                            |
                            v
                     Audit / Detection
```

---

# Appendix D — Research Positioning Note

This proposal intentionally avoids claiming:

-  guaranteed publication; 
-  guaranteed security; 
-  guaranteed superiority; 
-  universal regulatory compliance; 
-  clinical readiness. 

A strong journal submission requires a defensible literature review, precise novelty claims, rigorous experiments, statistically supported results, appropriate state-of-the-art baselines, and reproducible evidence.

---

# References

The final manuscript should use peer-reviewed and authoritative sources for:

1.  Blockchain and distributed ledger technology. 
2.  Electronic prescription security. 
3.  Healthcare privacy and access control. 
4.  Digital signatures and cryptographic commitments. 
5.  Smart-contract security. 
6.  HL7 FHIR interoperability. 
7.  Healthcare blockchain systems. 
8.  Threat modeling and security evaluation. 
9.  Privacy-preserving healthcare architectures. 
10.  Relevant regulations and ethical frameworks. 

A separate `references/references.bib` file should be maintained for the final manuscript.

