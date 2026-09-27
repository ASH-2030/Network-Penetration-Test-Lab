# Network Penetration Test Lab

## Week 3 — Professional Advanced Build

An authorized network penetration testing project conducted in an isolated laboratory environment modeled on a family-owned investment advisory.

The assessment focused on network discovery, TCP port enumeration, service identification, manual validation, vulnerability documentation, remediation, and retesting.

> **Important:** This project was performed exclusively against an isolated laboratory target. No public, third-party, or real-world systems were tested.

---

## Project Objective

The objective of this project was to perform a structured network penetration test against an intentionally isolated lab target.

The assessment was designed to identify exposed network services and security weaknesses, validate findings using manual and automated techniques, document their security impact, apply remediation, and verify the fixes through retesting.

---

## Scope

### Authorized Target

| Item            | Details                             |
| --------------- | ----------------------------------- |
| Target          | `pentest-target`                    |
| IP Address      | `192.168.155.30`                    |
| Tester          | Kali Linux                          |
| Network         | Isolated laboratory network         |
| Assessment Type | Authorized network penetration test |

### In Scope

* Host discovery
* TCP port scanning
* Service enumeration
* HTTP service testing
* Directory and file exposure testing
* Banner enumeration
* Manual service validation
* Remediation verification
* Final full-port verification

### Out of Scope

* Public or third-party systems
* Real financial infrastructure
* Real customer information
* Real credentials
* Internet-facing targets
* Social engineering
* Denial-of-service testing
* Destructive exploitation
* Unauthorized access

---

## Laboratory Architecture

The testing environment consisted of a Kali Linux virtual machine and an isolated Linux network namespace used as the target.

```text
                    Isolated Lab Network
                           |
                           |
                    +-------------+
                    |  Kali Linux |
                    |   Tester    |
                    +-------------+
                           |
                     Virtual Link
                           |
                           |
                  +------------------+
                  |  pentest-target  |
                  |  192.168.155.30  |
                  +------------------+
```

The target was intentionally isolated so that all testing remained within the authorized laboratory boundary.

---

## Tools Used

### Nmap

Used for:

* Host discovery
* Full TCP port scanning
* Service/version enumeration
* HTTP directory enumeration
* Banner enumeration

### cURL

Used for:

* HTTP response testing
* Header inspection
* Manual resource validation
* Retesting remediated resources

### Netcat

Used for:

* Manual TCP service connection
* Validation of the simulated administrative service
* Confirming unauthenticated service behavior

### Socat

Used to create a controlled simulated TCP service for the laboratory scenario.

### Linux Network Namespaces

Used to create the isolated target environment without relying on an external or public system.

---

## Methodology

The assessment followed a structured workflow:

```text
Scope Definition
       ↓
Environment Setup
       ↓
Host Discovery
       ↓
TCP Port Scanning
       ↓
Service Enumeration
       ↓
Manual Validation
       ↓
Finding Documentation
       ↓
Remediation
       ↓
Retesting
       ↓
Final Verification
```

Detailed methodology is available in:

`documentation/methodology.md`

---

# Findings

## Finding 01 — Exposed Backup Directory / Sensitive File Disclosure

### Affected Asset

* **Target:** `192.168.155.30`
* **Port:** `8080/tcp`
* **Service:** HTTP
* **Path:** `/backup/`

A controlled backup directory was exposed through the HTTP service.

The directory contained a fabricated demonstration file named:

`config-backup.txt`

### Validation

Nmap HTTP enumeration identified:

```text
/backup/: Possible backup
```

Manual cURL testing confirmed that the directory and demonstration file were accessible.

### Severity

**Medium — Lab Context**

The finding demonstrates a security control weakness. The demonstration data was fabricated and did not contain real sensitive information.

### Remediation

The exposed backup directory was removed from the web-accessible document root.

### Retest

The same resource was requested again and returned:

```text
HTTP/1.0 404 File not found
```

### Status

**Remediated and verified**

Full details:

`findings/finding-01-backup-exposure.md`

---

## Finding 02 — Unauthenticated Plaintext Legacy Administrative Service

### Affected Asset

* **Target:** `192.168.155.30`
* **Port:** `2323/tcp`
* **Service:** Custom simulated legacy administrative service
* **Protocol:** Plaintext TCP
* **Authentication:** None

A controlled simulated administrative-style service accepted connections without authentication and returned a plaintext status banner.

### Validation

Netcat returned:

```text
ADVISORY-LEGACY-ADMIN
STATUS: ONLINE
AUTHENTICATION: NONE
ENVIRONMENT: LAB-ONLY
```

Nmap banner enumeration independently retrieved the same laboratory response.

Nmap service fingerprinting was inconclusive. The uncertain `3d-nfsd?` fingerprint was not treated as the actual service identity.

### Severity

**Medium — Lab Context**

The finding demonstrates the risk associated with an unauthenticated administrative-style network service.

No real credentials, administrative functions, or sensitive information were exposed.

### Remediation

The simulated service was disabled.

### Retest

A subsequent Nmap scan reported:

```text
2323/tcp closed
```

### Status

**Remediated and verified**

Full details:

`findings/finding-02-unauthenticated-service.md`

---

## Final Verification

After remediation of the identified services, a final full TCP scan was performed:

```text
sudo nmap -sS -sV -p- 192.168.155.30
```

The final scan confirmed that no open TCP ports remained on the target.

This provided final verification that the intentionally exposed laboratory services had been removed or disabled.

---

## Evidence

The project includes screenshots documenting major stages of the assessment.

Evidence covers:

* Kali network configuration
* Isolated lab network
* Target creation
* Connectivity verification
* Host discovery
* Initial port scan
* HTTP service verification
* Service enumeration
* HTTP baseline testing
* Backup exposure
* Finding validation
* Remediation and retesting
* Legacy service validation
* Legacy service enumeration
* Final verification

Screenshots can be placed in the:

`evidence/`

directory.

---

## Project Documentation

```text
Network-Penetration-Test-Lab/
│
├── README.md
│
├── evidence/
│   └── Screenshots and assessment evidence
│
├── findings/
│   ├── finding-01-backup-exposure.md
│   └── finding-02-unauthenticated-service.md
│
└── documentation/
    ├── scope.md
    ├── methodology.md
    └── professional-references.md
```

---

## Key Security Practices Demonstrated

This project demonstrated the following practical security practices:

* Defining a testing scope before assessment
* Restricting testing to an authorized target
* Establishing a network baseline
* Performing systematic port enumeration
* Identifying network services
* Combining automated tools with manual validation
* Recording evidence for findings
* Distinguishing uncertain scanner output from confirmed service behavior
* Applying remediation
* Retesting after remediation
* Performing final verification
* Maintaining professional project documentation

---

## Improvement With More Time

With additional development time, the project could be extended with:

* Automated report generation
* Structured JSON/CSV finding output
* Additional network-service test cases
* Automated remediation verification
* CVSS-based severity calculations
* Expanded evidence collection
* A repeatable scan-and-report workflow
* Additional isolated target services for testing

The current implementation intentionally prioritizes controlled testing, manual validation, and clear evidence over automation complexity.

---

## Professional References

The methodology was informed by established security-testing resources, including:

* NIST SP 800-115 — Technical Guide to Information Security Testing and Assessment
* OWASP Web Security Testing Guide
* Penetration Testing Execution Standard (PTES)
* CREST penetration testing guidance

Detailed references are available in:

`documentation/professional-references.md`

---

## Authorization and Responsible Testing

This repository documents an educational security assessment performed against an intentionally isolated laboratory environment.

The target address `192.168.155.30` was created specifically for this controlled assessment.

No public, third-party, production, or real financial systems were targeted.

All testing activities were performed within the defined authorization boundary.

---

## Project Status

**Assessment Completed**

* [x] Scope defined
* [x] Lab environment configured
* [x] Host discovery completed
* [x] Full TCP scan completed
* [x] Services enumerated
* [x] Findings manually validated
* [x] Findings documented
* [x] Remediation performed
* [x] Retesting completed
* [x] Final verification completed
* [x] Documentation prepared
