# Security Reality Check: SSO, VPN, and Centralized Credentials

> **Note:** These risks, attack flows, and mitigations apply to ALL major identity providers (Azure AD, Google Cloud Identity, AWS IAM Identity Center, Okta, Ping, etc.).

> **These are not all the factors you need for designing secure architectures. This is a blueprint/starting point.**

---

## The Harsh Truth About Centralized Credentials & "Mitigations"

**Even with MFA, Conditional Access, device compliance, network segmentation, and certificates, a single set of credentials that unlocks VPN, LOB apps, and cloud apps creates a massive risk if compromised.**

### Where Defenses *Help*—and Where They Fail:

- **MFA isn’t perfect:** Push fatigue attacks (users mindlessly accepting prompts) are common. Attackers exploit weak MFA habits and can trick users into authorizing bad logins.
- **Conditional Access:** Only as strong as the rules and IP ranges you allow. Attackers can use their own VPN/IP that matches an allowed location, especially if geo/range controls are loose.
- **Network Segmentation:** Limits what an attacker can reach, but only if user access is minimal and roles are enforced.
- **Auditing & Rapid Response:** Useful for catching unusual post-login activity, but may be too slow to stop initial damage if attacker moves quickly.
- **Certificates & Device Compliance:** Adds friction—attackers need your device, registered cert, and (maybe) access to a managed endpoint. Not foolproof: attackers on the device (via RDP/remote control) bypass this barrier.
- **Blast radius:** When a single identity is used for everything (VPN + cloud + LOB), compromise = wide open doors.

### What *Actually* Reduces Risk (but is rarely perfect):

- **Certificates for VPN** (not just username/pass)
- **MFA—preferably number matching, OTP, or FIDO2/Yubikeys**
- **Device compliance—block access from unregistered/unhealthy devices**
- **Least-privilege design—users get access only to what they truly need**
- **Zero-trust behavior analytics—monitor for strange actions, not just strange logins**
- **Privileged access workstations (PAW), JIT access, just enough admin**

### What Does NOT Truly Solve the Problem:

- "Accept" style MFA prompts (push fatigue risk)
- Relying solely on geo or IP allow lists
- Overly broad conditional access rules
- Trusting audits/alerts to catch everything fast enough
- Certificates *alone* without device posture/MFA

---

## Real-World Attack Scenarios & Weakest Links

- **Attacker phishes credentials, then triggers MFA push:** User accidentally approves. Attacker uses VPN and SSO to access internal/cloud apps.
- **Attacker gains RDP to victim’s endpoint:** Has cert and SSO cookies/tokens. Now indistinguishable from the real user.
- **Allowed IP range too broad:** Attacker’s VPN endpoint matches—bypasses location-based rules.

---

## Takeaway for Architects

> **Design as if a breach WILL happen.**
>
> - Assume any one control can and will fail—layer controls, use defense-in-depth, and segment everything.
> - If you must use centralized identity, minimize privileges and segment access. Push for device certs, number-matching or phishing-resistant MFA, and ongoing behavioral monitoring.
> - Communicate the real limits of each “mitigation” to both security teams and business leaders—avoid a false sense of security.

---
