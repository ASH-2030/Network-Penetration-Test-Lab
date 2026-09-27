# Network Penetration Test on Lab Environment for a Family-Owned Investment Advisory

## Project

**Network Penetration Test on Lab Environment for a Family-Owned Investment Advisory**

## Assessment Type

Authorized network penetration testing and security validation of an isolated laboratory environment.

## Objective

The objective of this assessment was to identify exposed network services and security weaknesses within a controlled lab environment, validate findings through manual and automated testing, document their security impact, and verify remediation through retesting.

## Authorized Target

* **Target IP:** 192.168.155.30
* **Target Name:** `pentest-target`
* **Network:** Isolated laboratory network
* **Tester:** Kali Linux virtual machine

## Testing Scope

The assessment included:

* Host discovery
* TCP port scanning
* Service enumeration
* HTTP service testing
* HTTP directory and file exposure testing
* Manual validation of discovered services
* Banner enumeration
* Identification of unauthenticated network services
* Remediation verification
* Final full-port verification

## Tools Used

* Nmap
* cURL
* Netcat
* Socat
* Linux network namespaces
* Kali Linux

## Out of Scope

The following were not included:

* Public or third-party systems
* Real financial systems
* Real investment advisory infrastructure
* Real credentials or customer information
* Internet-facing targets
* Social engineering
* Denial-of-service testing
* Destructive exploitation
* Unauthorized access to external systems

## Authorization and Safety

All testing was performed against an intentionally isolated laboratory target created for this assessment.

The IP address `192.168.155.30` belonged to the controlled lab environment. Demonstration data used during testing was fabricated and did not represent real customer, financial, authentication, or organizational data.

Testing was limited to the authorized target and was performed for defensive security assessment and learning purposes.

## Assessment Boundary

The assessment boundary was limited to the network services exposed by the isolated `pentest-target` namespace.

The Kali Linux system acted as the authorized tester. No external hosts were intentionally targeted.

## Scope Summary

**In Scope:** `192.168.155.30`

**Out of Scope:** All external, public, third-party, and real-world systems.

**Testing Principle:** Identify → Validate → Document → Remediate → Retest
