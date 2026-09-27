# Finding 02 — Unauthenticated Plaintext Legacy Administrative Service

## Finding Overview

During the authorized network penetration test, a controlled simulated legacy administrative service was exposed on TCP port 2323.

The service accepted a connection without authentication and returned a plaintext administrative-style status banner. The service was intentionally created for this isolated laboratory exercise and contained no real credentials or sensitive information.

## Affected Asset

* **Target IP:** 192.168.155.30
* **Port:** 2323/tcp
* **Service:** Custom simulated legacy administrative service
* **Protocol:** Plaintext TCP
* **Authentication:** None

## Discovery and Manual Validation

The service was tested from the Kali tester using Netcat:

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

This demonstrated that the service could be accessed without authentication and that its response was transmitted as plaintext.

## Nmap Enumeration

Nmap service detection was also performed:

```text
sudo nmap -sS -sV -p 2323 192.168.155.30
```

Nmap's service fingerprinting was inconclusive and produced an uncertain `3d-nfsd?` identification. This should **not** be interpreted as evidence that a real 3d-nfsd service was running.

A banner enumeration test was then performed:

```text
sudo nmap -p 2323 --script banner 192.168.155.30
```

The banner script successfully retrieved the custom laboratory response, confirming the service behavior.

## Security Impact

An unauthenticated administrative-style network service can create unnecessary attack surface. In a real environment, an administrative interface that does not require authentication could potentially expose administrative functionality or operational information to unauthorized users.

The laboratory service contained only fabricated status information and did not provide access to real administrative functions or sensitive data.

## Severity

**Medium — Lab Context**

The severity reflects the security weakness demonstrated by an unauthenticated administrative-style service. It does not represent a confirmed compromise of a real system.

## Remediation

Because the simulated legacy service was unnecessary for the lab's intended target functionality, the service was disabled:

```text
sudo ip netns exec pentest-target pkill socat
```

The target namespace was then checked to confirm that no listening service remained.

## Retest

The service was tested again from Kali:

```text
sudo nmap -sS -p 2323 192.168.155.30
```

The result showed:

```text
2323/tcp closed
```

This confirmed that the unauthenticated service was no longer exposed.

## Final Status

**Remediated and verified.**

The finding lifecycle was:

**Service Discovery → Manual Validation → Nmap Enumeration → Finding → Service Disabled → Retest → Closed**

## Important Enumeration Note

Nmap's uncertain `3d-nfsd?` fingerprint was not treated as the identity of the service. The assessment records the service accurately as a **custom simulated legacy administrative service**, with the Nmap banner output providing corroborating evidence of the actual response.
