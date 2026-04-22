# M.I.C.R.O — Microsoft Interactive Cloud Response Oracle

> **A single-file, self-contained IR reference tool for Microsoft 365, Entra ID, Azure, and Copilot incidents.**

<h3 align="center">
  <a href="https://YOUR-USERNAME.github.io/micro/" target="_blank" rel="noopener noreferrer">
    🟢 LAUNCH M.I.C.R.O 🟢
  </a>
</h3>

No build step. No server. No dependencies to install. Drop `index.html` anywhere and open it.

---

## What's Inside

**42 threat scenarios** across four Microsoft cloud pillars — each with IOCs/IOAs, MITRE ATT&CK mappings, ready-to-run Sentinel KQL, prioritized mitigations, and interactive IR runbooks with progress tracking.

| Pillar | Threats | Focus |
|---|---|---|
| **Entra ID** | 19 | Identity, authentication, MFA, CAP, privilege escalation, workload identity, hybrid |
| **Microsoft 365** | 14 | BEC, email rules, Teams phishing, SharePoint ransomware, Graph exfil, OAuth apps, eDiscovery |
| **Azure** | 4 | RBAC escalation, Key Vault exfiltration, Storage abuse, Automation runbook abuse |
| **Copilot & AI** | 3 | Copilot abuse, indirect prompt injection, oversharing/insider risk |

---

## Views

| View | Description |
|---|---|
| **Home** | Stats dashboard, highest-risk scenarios, quick navigation |
| **Threat Matrix** | Filterable card grid — filter by pillar, severity, or free-text search |
| **Table View** | Compact sortable list for rapid scanning with risk scores |
| **Attack Chains** | 7 pre-built kill chains linking threats end-to-end (BEC, AiTM, Golden SAML, Cloud Ransomware, Workload Identity Pivot, Azure Lateral, Copilot chain) |
| **Risk Heat Map** | 5×5 Likelihood × Impact matrix — click any cell to inspect |
| **Acquisition** | **New in v1.0** — Artifact collection guide, log sources, license matrix, rogue app signatures, dangerous permissions, IR tools |

---

## Acquisition Guide

The **Acquisition** view covers what no threat map usually does — *how to actually collect the evidence*:

- **Log Sources** — UAL, MailItemsAccessed, Sign-in Logs, Mailbox Audit Log, Message Trace, Entra ID Audit Logs, Graph Activity Logs, SP Sign-in Logs, plus Microsoft-Extractor-Suite commands for each
- **License Matrix** — Which logs exist at E3 vs E5 vs P1/P2 vs Audit Premium (the most critical practical gap in IR)
- **Rogue Apps** — Known Traitorware and Stealthware OAuth apps (EM Client, PerfectData, SigParser, rclone, CloudSponge) with permissions and AppIds
- **Dangerous Permissions** — Reference of high-risk Graph API permission scopes (Mail.ReadWrite, RoleManagement.ReadWrite.Directory, AppRoleAssignment.ReadWrite.All, etc.)
- **IR Tools** — Microsoft-Extractor-Suite, Hawk, Untitled Goose Tool, Cazadora, RogueApps, Microsoft-Analyzer-Suite, ScubaGear

---

## Threat Scenarios (42)

<details>
<summary>Entra ID (19 scenarios)</summary>

- User Account Compromise
- AiTM — Adversary-in-the-Middle
- Token Theft / Session Hijacking
- MFA Fatigue / Push Bombing
- Device Code Phishing
- Legacy Authentication Exploitation
- Conditional Access Policy Gaps
- Entra ID Tenant Reconnaissance
- PIM Eligible Role Abuse
- Privilege Escalation via Role Assignment
- Ghost Device Registration
- Federated Identity / Golden SAML
- Hybrid Identity Attack (AADConnect / MSOL)
- Identity Protection Evasion / Impossible Travel Bypass
- Suspicious Credential Addition to OAuth App *(Workload Identity)*
- Service Principal Privilege Escalation *(Workload Identity)*
- Rogue / Shadow Application Registration *(Workload Identity)*
- Anomalous API Calls by Service Principal *(Workload Identity)*
- Managed Identity Token Exfiltration / SSRF / IMDS *(Workload Identity)*
- Malicious Guest Account Creation
- Service Principal / Workload Identity Abuse (incl. FOCI)
</details>

<details>
<summary>Microsoft 365 (14 scenarios)</summary>

- BEC — Internal Impact
- BEC — External Impact
- BEC: Inbox Rule Manipulation
- Malicious Email Forwarding Rules
- Teams-Based Phishing & External Access Abuse
- Exchange Transport Rule Backdoor
- SharePoint / OneDrive Ransomware via Versioning Abuse
- Data Exfiltration via Microsoft Graph API
- Power Platform Abuse
- Illicit Consent Grant (OAuth Phishing)
- Abnormal User Agent / API Abuse
- Illicit OAuth Consent Grant (Consent Phishing)
- App Governance Policy Gap / Dormant Permissions Abuse
- eDiscovery / Content Search Abuse
</details>

<details>
<summary>Azure (4 scenarios)</summary>

- Azure RBAC Privilege Escalation
- Azure Key Vault Secret / Certificate Exfiltration
- Azure Storage Account Data Exfiltration
- Azure Automation Runbook Abuse
</details>

<details>
<summary>Copilot & AI (3 scenarios)</summary>

- Microsoft Copilot for M365 Abuse
- Copilot Indirect Prompt Injection
- Copilot Oversharing / Excessive Data Surfacing
</details>

---

## Attack Chains (7)

1. **BEC Kill Chain** — Account compromise → inbox rules → internal fraud → external wire transfer
2. **AiTM → Token Theft → App Persistence** — Phishing proxy → token theft → credential addition → bulk exfil
3. **Recon → Escalation → Golden SAML** — Tenant recon → PIM abuse → role escalation → federated identity persistence
4. **Cloud Ransomware Kill Chain** — Account compromise → Graph exfil → SharePoint versioning wipe
5. **Workload Identity Pivot & Persistence** — AiTM → credential addition → SP privilege escalation → anomalous API abuse
6. **Azure Lateral Movement Chain** — RBAC escalation → Key Vault dump → managed identity pivot → M365 exfil
7. **Copilot-Assisted Exfiltration** — Account compromise → Copilot data surfacing → prompt injection → Graph exfil

---

## Runbook Phase Order

All 42 runbooks follow containment-first IR practice:

```
Triage → Contain → Investigate → Recover → Escalate If
```

Contain before Investigate — limiting attacker dwell time takes priority over full scope assessment.

---

## Deploying to GitHub Pages

1. Fork or clone this repository
2. Go to **Settings → Pages**
3. Set source to **Deploy from branch → main → / (root)**
4. The `.nojekyll` file ensures GitHub Pages serves the HTML as-is without Jekyll processing

The site will be live at `https://YOUR-USERNAME.github.io/REPO-NAME/`

---

## KQL Requirements

Queries target **Microsoft Sentinel**. Required connectors:

| Connector | Tables |
|---|---|
| Microsoft Entra ID | `SigninLogs`, `AuditLogs`, `AADNonInteractiveUserSignInLogs` |
| Microsoft Entra ID (Diagnostic Settings) | `AADServicePrincipalSignInLogs`, `MicrosoftGraphActivityLogs` |
| Microsoft 365 Defender | `CloudAppEvents`, `EmailEvents` |
| Office 365 | `OfficeActivity` |
| Azure Activity | `AzureActivity` |
| Azure Diagnostics | `AzureDiagnostics` (Key Vault), `StorageBlobLogs` |

> **Graph Activity Logs and Service Principal Sign-in Logs require manual Diagnostic Settings configuration — they are NOT enabled by default.**

---

## References

- [MITRE ATT&CK for Enterprise](https://attack.mitre.org/matrices/enterprise/)
- [Azure Threat Research Matrix (MSTIC)](https://microsoft.github.io/Azure-Threat-Research-Matrix/)
- [Microsoft-Extractor-Suite by Invictus IR](https://github.com/invictus-ir/Microsoft-Extractor-Suite)
- [RogueApps by Huntress Labs](https://huntresslabs.github.io/rogueapps/)
- [M365 OAuth Apps — randomaccess3](https://github.com/randomaccess3/detections/blob/main/M365_Oauth_Apps/MaliciousOauthAppDetections.json)
- [Cazadora — HuskyHacks](https://github.com/HuskyHacks/cazadora)
- [Tenable — Dangerous Delegated Permissions](https://www.tenable.com/blog/microsoft-entra-dangerous-permissions)
- [CISA ScubaGear](https://github.com/cisagov/ScubaGear)
- [Invictus IR — Advanced Incident Response in the Microsoft Cloud](https://academy.invictus-ir.com/advanced-incident-response-in-the-microsoft-cloud)
- [Microsoft Incident Response Playbooks](https://learn.microsoft.com/en-us/security/operations/incident-response-playbooks)
- [GIAC FOR608 / FOR509](https://www.giac.org/certifications/cloud-forensics-responder-gcfr/)

---

## License

GNU General Public License v3.0 — see [LICENSE](LICENSE) for details.

> For defensive and incident response use only.
