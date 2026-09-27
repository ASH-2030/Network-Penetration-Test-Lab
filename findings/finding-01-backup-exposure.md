# Finding 01 — Exposed Backup Directory / Sensitive File Disclosure

## Finding Overview

During the authorized network penetration test, the simulated investment advisory web service was found to expose a `/backup/` directory through HTTP on TCP port 8080.

The directory was accessible without authentication and contained a controlled demonstration file named `config-backup.txt`.

All data used for this test was fabricated and existed only within the isolated laboratory environment.

## Affected Asset

* **Target IP:** 192.168.155.30
* **Service:** HTTP
* **Port:** 8080/tcp
* **Affected Path:** `/backup/`
* **File:** `config-backup.txt`

## Discovery

The HTTP service was first identified through Nmap service enumeration:

```text
8080/tcp open http SimpleHTTPServer 0.6 (Python 3.13.12)
```

Manual HTTP testing was then performed using `curl`.

The `/backup/` directory returned a successful HTTP response and displayed its directory contents. The `config-backup.txt` file was also directly accessible.

## Evidence

The finding was independently corroborated using Nmap's HTTP enumeration script:

```text
sudo nmap -p 8080 --script http-enum 192.168.155.30
```

Nmap identified:

```text
/backup/: Possible backup
```

Manual validation also confirmed that the backup file was listed in the directory.

## Security Impact

Exposing backup or configuration files through a web-accessible directory can disclose information that should not be publicly available. In a real production environment, such files could potentially contain configuration details, credentials, internal hostnames, or other sensitive information.

The demonstration file used in this laboratory contained only fabricated test data and did not contain real credentials or sensitive information.

## Severity

**Medium — Lab Context**

The severity reflects the security control weakness demonstrated by the exposed backup directory. It does not indicate that real sensitive data was compromised during this laboratory exercise.

## Remediation

The exposed backup directory was removed from the web-accessible document root:

```text
sudo ip netns exec pentest-target rm -rf /tmp/advisory-public/backup
```

The target directory was then checked to confirm that the backup directory had been removed.

## Retest

The previously accessible path was requested again:

```text
curl -i http://192.168.155.30:8080/backup/
```

The response changed from a successful `200 OK` response to:

```text
HTTP/1.0 404 File not found
```

This confirmed that the previously exposed resource was no longer accessible.

## Final Status

**Remediated and verified.**

The finding lifecycle was:

**Discovery → Manual Validation → Nmap Corroboration → Remediation → Retest → Fixed**
