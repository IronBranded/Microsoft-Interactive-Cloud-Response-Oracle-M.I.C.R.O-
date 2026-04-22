# Contributing to M.I.C.R.O

M.I.C.R.O is a single-file tool — all threat data, components, and logic live in `index.html`. This keeps deployment trivially simple (just host the file) but means all contributions go into one place.

## Adding a New Threat

Threats are JavaScript objects in the `ALL_THREATS` array inside `index.html`. Each object follows this schema:

```javascript
{
  id: 'unique-kebab-case-id',       // must be unique, used in attack chains
  pillar: 'entra-id',               // entra-id | m365 | azure | copilot
  sub: 'Authentication',            // subcategory label shown in card
  severity: 'critical',             // critical | high | medium | low
  title: 'Human-readable title',
  shortDesc: 'One sentence summary shown in card view.',
  description: 'Full paragraph description for the Overview tab.',
  vectors: [                        // array of strings
    'How attackers execute this technique',
  ],
  iocs: [                           // Indicators of Compromise
    'Specific log entry or artifact to hunt for',
  ],
  ioas: [                           // Indicators of Attack (behavioral)
    'Behavioral pattern indicating active attack',
  ],
  mitre: [
    { id: 'T1078', name: 'Valid Accounts', tactic: 'Initial Access' },
  ],
  azureMatrix: [
    { phase: 'Initial Access', technique: 'Description' },
  ],
  kql: `// Your KQL query here\nTableName\n| where ...`,
  mitigations: [
    'Specific, actionable mitigation step',
  ],
  runbook: {
    triage:     ['Step 1', 'Step 2'],
    contain:    ['Step 1', 'Step 2'],
    investigate:['Step 1', 'Step 2'],
    recover:    ['Step 1', 'Step 2'],
    escalate:   ['Condition for escalation'],
  },
  risk: { l: 3, i: 4 },            // Likelihood (1-5) × Impact (1-5)
}
```

After adding the threat object, add its risk score to the `RISK_MAP` object:

```javascript
'your-new-threat-id': { l: 3, i: 4 },
```

## Runbook Phase Order

Always use: **Triage → Contain → Investigate → Recover → Escalate If**

Containment before investigation is intentional — limiting dwell time takes priority.

## KQL Guidelines

- Target **Microsoft Sentinel** tables by default
- Add a comment if a query requires a specific connector or Diagnostic Settings configuration:
  ```kql
  // Requires: MicrosoftGraphActivityLogs via Diagnostic Settings
  ```
- Include the table name on the first data line (not in comments) so the table is obvious at a glance
- Adjust threshold values with comments explaining the rationale

## Adding to an Attack Chain

If your threat logically belongs in or extends an existing attack chain, add its `id` to the `steps` array in `ATTACK_CHAINS`. If you're adding a new end-to-end chain, add a new object:

```javascript
{ 
  id: 'my-chain', 
  title: 'My Attack Chain', 
  color: '#hex', 
  steps: ['threat-id-1', 'threat-id-2', 'threat-id-3'] 
},
```

## Adding to the Acquisition View

The Acquisition view has five sub-sections:

- **logSources** — Log source definitions with MES commands, retention, caveats, anti-forensics notes
- **licenseMatrix** — License availability table rows
- **rogueApps** — Known Traitorware/Stealthware apps with permissions and AppIds
- **dangerousPermissions** — Graph API permission scopes with risk rating
- **irTools** — IR and hunting tools with URLs

Each section has its own data structure inside `ACQUISITION_DATA` in `index.html`. The format is self-documenting — copy an existing entry and modify it.

## Submitting

1. Fork the repo
2. Make your changes to `index.html`
3. Test by opening the file locally in a browser
4. Open a Pull Request with a description of what you added and your evidence/sources

## Quality Bar

- IOCs and IOAs should be specific enough to query — avoid vague entries like "unusual activity"
- KQL should be tested against real data where possible, or clearly labeled as untested
- Runbook steps should be actionable commands, not general advice
- MITRE technique IDs should be verified against attack.mitre.org
- Risk scores (L × I) should be calibrated against real-world observation frequency and blast radius

## ⚠️ Do Not Submit

- Private or proprietary incident data
- Personally identifiable information
- Unverified threat intelligence without clear sourcing
- Content that could facilitate offensive use against non-consenting targets

---

For defensive and incident response use only.
