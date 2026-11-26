# Microsoft Sentinel SOC Investigations

A hands-on Microsoft Sentinel and Azure lab portfolio focused on SIEM operations, KQL-based threat hunting, Linux authentication logs, and incident investigation.

> **Evidence note:** This repository documents a lab completed using material retained from April 2026, including screenshots, KQL queries, notes, and module documentation. The original lab environment is no longer available. Findings are limited to that retained material; no new queries or retrospective validation are claimed.

## Project overview

The investigation examined SSH authentication activity against an Azure-hosted Linux VM. Syslog data was queried in Microsoft Sentinel to identify failed SSH login attempts, extract usernames and source IP addresses, and compare attack patterns across sources.

### Recorded findings

| Source IP | Failed SSH attempts | Targeted usernames / account detail |
|---|---:|---|
| `138.124.7.7` | 30 | `root` |
| `87.106.91.226` | 37 | 14 usernames |
| `192.252.215.125` | 22 | 15 usernames |
| **Total** | **89** | Multiple accounts targeted |

A hunting bookmark was created for `138.124.7.7`.

**Assessment:** The retained evidence shows repeated failed SSH authentication attempts, including activity consistent with username enumeration. A successful SSH login was **not verified in the available material**. No unauthorized access or host compromise can therefore be confirmed from this investigation record.

## Investigation workflow

1. Review Syslog events collected from the Linux lab VM.
2. Filter failed SSH password messages.
3. Parse usernames and source IP addresses from message text.
4. Aggregate attempts by source IP and count distinct usernames.
5. Compare observed activity and identify sources for further investigation.
6. Create a hunting bookmark for the selected source IP.
7. Document findings, evidence limitations, and the incident assessment.

## KQL example

The query below reflects the parsing and aggregation approach documented for the lab:

```kusto
Syslog
| where SyslogMessage contains "Failed password"
| parse SyslogMessage with * "for " User " from " SrcIP " port " *
| summarize Attempts = count(), UsersTargeted = dcount(User) by SrcIP
| order by Attempts desc
```

The query is an example of the investigation method. The result counts above are transcribed from the retained lab notes; they are not represented as a fresh execution of this query.

## Repository structure

```text
.
├── README.md
├── investigations/
│   └── ssh-brute-force/
│       ├── README.md
│       ├── incident-report.md
│       └── screenshots/
│           └── README.md
├── kql-queries/
│   ├── README.md
│   └── ssh-failed-login-analysis.kql
└── documentation/
    ├── investigation-methodology.md
    └── lab-modules.md
```

## Lab coverage

The broader Microsoft Sentinel learning path covered ten modules: environment setup and onboarding; data connectors and log ingestion; KQL fundamentals; analytics rules and detection engineering; incident management and investigation; threat hunting; workbooks and visualization; SOAR playbooks and automation; threat intelligence integration; and a simulated APT case study.

The module list describes the learning scope shared for this lab. This repository does not claim detailed outcomes for modules where supporting artifacts have not been retained.

## Tools and technologies

- Microsoft Sentinel
- Microsoft Azure
- Log Analytics Workspace
- Linux / Ubuntu
- Syslog
- Kusto Query Language (KQL)

## Evidence and limitations

- The lab was simulated and conducted in an Azure learning environment.
- The original lab environment is no longer accessible.
- Documentation is based on material retained from April 2026.
- Screenshots should be added only when they directly support the section they illustrate.
- The available record does not verify a successful SSH login, unauthorized access, or compromise.
- The source IPs are reproduced as recorded in the lab notes; they are not independently attributed to a real-world actor.

## Further reading

- [Microsoft Sentinel documentation](https://learn.microsoft.com/azure/sentinel/)
- [Kusto Query Language documentation](https://learn.microsoft.com/azure/data-explorer/kusto/query/)
- [Analyze security events with Microsoft Sentinel](https://learn.microsoft.com/azure/sentinel/)
