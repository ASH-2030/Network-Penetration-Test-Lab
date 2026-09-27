# Professional References and Methodology Examples

## Purpose

Professional penetration testing is normally performed using structured methodologies and documented testing practices. The following references were reviewed to understand how professional security assessments are planned, conducted, documented, and validated.

## 1. NIST SP 800-115 — Technical Guide to Information Security Testing and Assessment

The National Institute of Standards and Technology (NIST) published SP 800-115 to provide guidance for planning and conducting technical information security testing and assessments.

The guide covers technical testing, analysis of findings, and development of mitigation strategies. It also discusses the benefits and limitations of different testing techniques.

### Relevance to This Project

The Week 3 assessment followed similar principles by:

* Defining the assessment scope before testing
* Identifying the authorized target
* Performing technical network testing
* Analyzing observed findings
* Applying remediation
* Retesting affected services
* Performing final verification

**Official Reference:**
NIST SP 800-115 — Technical Guide to Information Security Testing and Assessment

https://csrc.nist.gov/pubs/sp/800/115/final

## 2. OWASP Web Security Testing Guide (WSTG)

The OWASP Web Security Testing Guide is a comprehensive framework and collection of practical techniques for testing web applications and web services.

The guide emphasizes systematic security testing and validation of security controls rather than relying only on automated tools.

### Relevance to This Project

The HTTP portion of the Week 3 assessment used a similar validation-oriented approach:

* Identify the web service
* Review HTTP responses
* Enumerate accessible resources
* Manually validate discovered behavior
* Document the security impact
* Apply remediation
* Retest the affected resource

The project did not treat automated Nmap output alone as sufficient evidence. Manual cURL testing was used to confirm the exposed backup directory.

**Official Reference:**
OWASP Web Security Testing Guide

https://owasp.org/www-project-web-security-testing-guide/

## 3. Penetration Testing Execution Standard (PTES)

The Penetration Testing Execution Standard is a professional framework describing phases commonly associated with penetration testing engagements.

It provides a structured way to think about penetration tests, including preparation, intelligence gathering, threat modeling, vulnerability analysis, exploitation, post-exploitation, and reporting.

### Relevance to This Project

The Week 3 laboratory assessment applied the general principle of performing penetration testing as a structured engagement rather than running isolated commands.

The project included:

* Scope definition
* Target discovery
* Network enumeration
* Service identification
* Validation of security weaknesses
* Remediation verification
* Documentation and reporting

The laboratory exercise intentionally avoided destructive exploitation because its objective was controlled vulnerability identification and remediation.

## 4. CREST Penetration Testing Guidance

CREST provides professional guidance and standards for penetration testing and technical security assessments.

Professional penetration testing guidance emphasizes controlled testing, appropriate methodology, evidence collection, and clear reporting of security findings.

### Relevance to This Project

The Week 3 project incorporated these general professional practices through:

* Explicit scope definition
* Authorized-only testing
* Evidence-based findings
* Clear affected assets and ports
* Severity assessment
* Remediation recommendations
* Retesting after remediation
* Final verification

## Comparison with the Week 3 Assessment

| Professional Practice  | Week 3 Implementation                       |
| ---------------------- | ------------------------------------------- |
| Define scope           | `scope.md` documented the authorized target |
| Identify assets        | Target `192.168.155.30` was identified      |
| Network discovery      | Nmap host discovery                         |
| Port enumeration       | Full TCP port scans                         |
| Service enumeration    | Nmap service detection                      |
| Manual validation      | cURL and Netcat testing                     |
| Evidence collection    | Nmap, HTTP, banner, and retest outputs      |
| Findings documentation | Individual finding Markdown files           |
| Remediation            | Exposed resources/services removed          |
| Retesting              | Affected ports and resources tested again   |
| Final verification     | Full TCP scan after remediation             |
| Reporting              | Findings and methodology documented         |

## Key Lesson

The main professional practice adopted from these references was to treat penetration testing as a complete assessment lifecycle rather than simply running scanning tools.

The Week 3 workflow therefore followed:

**Scope → Discovery → Enumeration → Validation → Finding → Remediation → Retest → Final Verification**

## References

1. National Institute of Standards and Technology (NIST), **SP 800-115: Technical Guide to Information Security Testing and Assessment**
   https://csrc.nist.gov/pubs/sp/800/115/final

2. OWASP Foundation, **Web Security Testing Guide (WSTG)**
   https://owasp.org/www-project-web-security-testing-guide/

3. Penetration Testing Execution Standard (PTES)
   https://www.pentest-standard.org/

4. CREST, **Penetration Testing Guidance and Standards**
   https://www.crest-approved.org/
