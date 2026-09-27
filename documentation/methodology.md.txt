# Week 3 Network Penetration Test — Methodology

## 1. Assessment Approach

The assessment followed a structured penetration-testing workflow designed for an isolated laboratory environment.

The overall process was:

**Scope Definition → Environment Setup → Host Discovery → Port Scanning → Service Enumeration → Manual Validation → Finding Documentation → Remediation → Retesting → Final Verification**

The assessment focused on identifying exposed network services and validating whether those services introduced security weaknesses.

## 2. Laboratory Environment

The testing environment consisted of:

* **Tester:** Kali Linux virtual machine
* **Target:** Isolated Linux network namespace named `pentest-target`
* **Target IP:** 192.168.155.30
* **Network:** Host-only laboratory network
* **Testing Model:** Kali Linux acting as the authorized tester

A virtual Ethernet pair was used to provide connectivity between the Kali testing environment and the isolated target namespace.

## 3. Host Discovery

The first technical step was confirming that the authorized target was reachable.

Command:

```text
sudo nmap -sn 192.168.155.30
```

The scan confirmed that the target host was active.

## 4. Initial Port Scanning

A full TCP SYN scan was performed before intentionally enabling demonstration services.

Command:

```text
sudo nmap -sS -p- 192.168.155.30
```

The initial scan showed no open TCP services.

This established a baseline for the target before service-based testing.

## 5. HTTP Service Deployment and Enumeration

A controlled HTTP service was introduced into the laboratory environment to provide a realistic network-service testing scenario.

The service was hosted on TCP port 8080.

Service enumeration was performed using:

```text
sudo nmap -sS -sV -p 8080 192.168.155.30
```

The service was identified as:

```text
8080/tcp open http SimpleHTTPServer 0.6 (Python 3.13.12)
```

Manual HTTP requests were then performed using cURL.

## 6. HTTP Baseline Testing

The HTTP response headers and publicly accessible content were reviewed using:

```text
curl -i http://192.168.155.30:8080/
```

The baseline confirmed that the HTTP service was responding successfully.

This provided a reference point for subsequent testing.

## 7. Finding 01 — Backup Directory Exposure

A controlled backup directory containing a fabricated configuration file was introduced into the web-accessible directory.

Manual testing confirmed that:

* `/backup/` was accessible
* Directory contents were exposed
* `config-backup.txt` was directly accessible

Nmap HTTP enumeration was then used for corroboration:

```text
sudo nmap -p 8080 --script http-enum 192.168.155.30
```

Nmap reported:

```text
/backup/: Possible backup
```

The finding was manually validated before being documented.

## 8. Finding 01 Remediation and Retest

The exposed backup directory was removed:

```text
sudo ip netns exec pentest-target rm -rf /tmp/advisory-public/backup
```

The resource was then requested again:

```text
curl -i http://192.168.155.30:8080/backup/
```

The server returned:

```text
HTTP/1.0 404 File not found
```

This confirmed that the exposed backup resource had been successfully removed.

## 9. Post-Remediation Port Verification

A full TCP scan was performed after the first remediation:

```text
sudo nmap -sS -sV -p- 192.168.155.30
```

The scan confirmed that TCP port 8080 was the only remaining open service at that stage.

## 10. Service Exposure Review

The HTTP service banner was also reviewed:

```text
curl -I http://192.168.155.30:8080/
```

The response disclosed:

```text
Server: SimpleHTTP/0.6 Python/3.13.12
```

The service version information was documented as an observation rather than a primary vulnerability finding.

Instead of treating banner hiding as the main remediation, the unnecessary demonstration HTTP service was disabled to reduce the exposed attack surface.

## 11. Finding 02 — Unauthenticated Legacy Administrative Service

A controlled simulated legacy administrative service was then introduced on TCP port 2323.

Manual validation was performed using:

```text
nc -nv 192.168.155.30 2323
```

The service returned:

```text
ADVISORY-LEGACY-ADMIN
STATUS: ONLINE
AUTHENTICATION: NONE
ENVIRONMENT: LAB-ONLY
```

This demonstrated that the service accepted connections without authentication and returned information over plaintext TCP.

## 12. Finding 02 Enumeration

Nmap service enumeration was performed:

```text
sudo nmap -sS -sV -p 2323 192.168.155.30
```

Nmap's service fingerprinting was inconclusive.

A banner script was then used:

```text
sudo nmap -p 2323 --script banner 192.168.155.30
```

The custom laboratory banner was retrieved successfully.

The uncertain Nmap `3d-nfsd?` fingerprint was not treated as the actual identity of the service.

The service was documented accurately as a custom simulated legacy administrative service.

## 13. Finding 02 Remediation and Retest

The unnecessary simulated service was disabled:

```text
sudo ip netns exec pentest-target pkill socat
```

The service was then retested:

```text
sudo nmap -sS -p 2323 192.168.155.30
```

The result showed:

```text
2323/tcp closed
```

This confirmed that the service was no longer exposed.

## 14. Final Verification

A final full TCP scan was performed:

```text
sudo nmap -sS -sV -p- 192.168.155.30
```

The final scan confirmed that no open TCP ports remained on the target.

This provided final verification that the intentionally exposed laboratory services had been removed or disabled.

## 15. Validation Philosophy

Findings were not treated as confirmed solely from automated scanner output.

The assessment used a combination of:

* Automated discovery
* Service enumeration
* Manual network validation
* HTTP request testing
* Banner inspection
* Remediation
* Retesting

This approach helped distinguish actual observed behavior from uncertain automated service identification.

## 16. Final Assessment Workflow

The completed workflow was:

**1. Define Scope**

**2. Confirm Target Connectivity**

**3. Establish Port Baseline**

**4. Enumerate Services**

**5. Manually Validate Behavior**

**6. Document Findings**

**7. Apply Remediation**

**8. Retest Affected Services**

**9. Perform Final Full-Port Verification**

This methodology was designed to keep the assessment controlled, reproducible, evidence-based, and limited to the authorized laboratory target.
