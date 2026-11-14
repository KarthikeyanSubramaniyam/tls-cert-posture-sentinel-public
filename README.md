# TLS Certificate Posture Sentinel

<p align="center">
  <img src="https://img.shields.io/badge/Security-TLS%2FSSL%20Posture-red?style=flat" alt="Security Domain">
  <img src="https://img.shields.io/badge/Compliance-RFC%205280%20%7C%20CA%2FB%20Forum-blue?style=flat" alt="Compliance">
  <img src="https://img.shields.io/badge/Python-3.10+-3776AB?style=flat&logo=python&logoColor=white" alt="Python">
</p>

An enterprise-grade, defensive TLS/SSL certificate and cipher suite security posture auditor. Inspects live endpoints or local certificates for lifecycle anomalies, Subject Alternative Name (SAN) discrepancies, obsolete handshake protocols, and cryptographic cipher weakness.

---

## 🎯 The Problem It Solves

Expired certificates, obsolete protocols (SSLv3, TLS 1.0/1.1), and weak cipher suites are leading causes of service outages, cryptographic downgrade attacks (e.g., POODLE, BEAST), and compliance non-conformance (PCI-DSS 4.0, HIPAA).

**TLS Certificate Posture Sentinel** provides automated defensive auditing that:
1. **Prevents Certificate Outages**: Tracks validity windows with proactive escalation intervals (30-day warning, 15-day critical) and flags non-compliant validity periods exceeding the 398-day CA/Browser Forum baseline.
2. **Eliminates Downgrade Risks**: Identifies deprecated protocols and enforces pure TLS 1.2 / TLS 1.3 policy adherence.
3. **Audits Cryptographic Agility**: Validates AEAD cipher suites (GCM/Poly1305), detects legacy CBC/RC4/DES algorithms, and flags inadequate symmetric key lengths (< 128-bit).
4. **Calculates Objective Posture Scores**: Emits a quantitative posture score (0–100) and letter grades (A+ through F) with explicit remediation recipes.

---

## 🏗️ System Architecture & Workflow

```
[ Target Domain / Endpoint ]
             |
             v
+-----------------------------+
|    1. TLS Handshake Engine  | -> Establishes TLS socket context, retrieves peer
|   (Safe Probing & SNI)      |    X.509 certificate, protocol, and cipher spec
+--------------+--------------+
               |
               v
+-----------------------------+
|    2. Lifecycle Analyzer    | -> Calculates remaining validity, flags near-expiry,
|  (RFC 5280 & CA/B Baseline) |    and detects historical expired certificates
+--------------+--------------+
               |
               v
+-----------------------------+
|    3. SAN & Identity Match  | -> Verifies FQDN & wildcard domain matching
|   (Subject Alternative Name)|    against SAN extension list (mitigates spoofing)
+--------------+--------------+
               |
               v
+-----------------------------+
|  4. Cipher & Protocol Audit | -> Evaluates symmetric bit strength, AEAD integrity,
|    (Downgrade Protection)   |    and flags deprecated handshake suites
+--------------+--------------+
               |
               v
+-----------------------------+
|    5. Scoring & Reporting   | -> Aggregates findings into quantitative score,
| (DevSecOps JSON & CLI Table)|    assigns letter grade (A+..F), and outputs JSON
+-----------------------------+
```

---

## 🚀 Technical Specifications & Rules

| Rule Identifier | Severity | Category | Description | Remediation Standard |
| :--- | :--- | :--- | :--- | :--- |
| **`TLS-EXP-001`** | `CRITICAL` | Lifecycle | Certificate has expired | Immediately renew certificate via ACME/Let's Encrypt or enterprise CA |
| **`TLS-EXP-002`** | `HIGH` | Lifecycle | Certificate expiring in $\le$ 15 days | Trigger emergency automated renewal workflow |
| **`TLS-SAN-001`** | `HIGH` | Identity | FQDN mismatch in Subject Alternative Names | Re-issue certificate with explicit SAN inclusion |
| **`TLS-PRT-001`** | `HIGH` | Protocol | Deprecated protocol negotiated (SSLv3, TLS 1.0, TLS 1.1) | Enforce TLS 1.2 minimum on web servers and load balancers |
| **`TLS-CPH-001`** | `HIGH` | Cryptography | Weak cipher detected (RC4, DES, 3DES, MD5, CBC) | Restrict ciphers to modern AEAD suites (AES-GCM, CHACHA20) |
| **`TLS-LEN-001`** | `HIGH` | Cryptography | Symmetric cipher strength < 128 bits | Enforce minimum 128-bit or 256-bit encryption keys |

---

## 💻 CLI Usage & Quickstart

### Audit Remote Endpoint
```bash
python -m src.tls_sentinel.cli github.com --port 443
```

### Structured Output for CI/CD Gates
```bash
python -m src.tls_sentinel.cli internal.api.corp --json
```

### Run Automated Test Suite
```bash
python -m unittest discover -s tests -p "test_*.py"
```

---

## 🔒 Production Code & Architecture Notice

* **Public Repository**: Contains comprehensive architectural blueprints, compliance specifications, and detection rule standards.
* **Private Repository**: Maintained in `KarthikeyanSubramaniyam/tls-cert-posture-sentinel` with full socket inspection modules, automated certificate renewal webhooks, and enterprise SIEM export pipelines.
