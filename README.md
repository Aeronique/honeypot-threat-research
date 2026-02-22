# honeypot-threat-research

I deployed a T-Pot honeypot on Google Cloud for 28 days and left it exposed to the internet to see what would show up. This repo documents the attack campaigns that emerged, the CVEs being actively exploited, where the traffic was coming from, and what patterns kept repeating across the full deployment.

All data was pulled directly from Elasticsearch using DSL queries through the Kibana Dev Tools console. Every number in this project is verifiable and sourced from raw query output saved in the `data/` directory.

---

## Deployment at a Glance

| | |
|---|---|
| **Platform** | T-Pot Community Edition on Ubuntu 24.04 LTS |
| **Infrastructure** | Google Cloud Platform |
| **Period** | February 1 to February 28, 2026 |
| **Total Events** | TBD (final count March 1, 2026) |
| **Days Captured** | 28 of 28 |
| **Collection Method** | Elasticsearch DSL queries, daily |

---

## What's in Here

```
honeypot-threat-research/
│
├── README.md
├── DATA_COLLECTION_METHODOLOGY.md
│
├── data/
│   ├── tpot_day01_2026-02-01.json
│   ├── tpot_day02_2026-02-02.json
│   └── ... (one file per day)
│
├── reports/
│   ├── 01_top_cves.md
│   ├── 02_most_recent_cve.md
│   ├── 03_campaign_[name].md
│   ├── 04_geographic_attribution.md
│   ├── 05_threat_actor_assessment.md
│   ├── 06_future_attack_trends.md
│   ├── 07_credential_attack_patterns.md
│   ├── 08_legacy_vulnerability_exploitation.md
│   ├── 09_cloud_infrastructure_abuse.md
│   ├── 10_port_targeting_analysis.md
│   └── 11_suricata_signature_analysis.md
│
└── screenshots/
    └── final_dataset_2026-03-01.png
```

---

## Reports

### Vulnerability Exploitation

| Report | Description |
|--------|-------------|
| [Top CVEs](reports/01_top_cves.md) | The three highest-volume CVEs across the full deployment, with daily counts and campaign context |
| [Most Recent CVE](reports/02_most_recent_cve.md) | How fast attackers picked up a newly disclosed CVE and started using it |
| [Legacy Vulnerability Exploitation](reports/08_legacy_vulnerability_exploitation.md) | CVEs from 2006 and earlier that were still showing up daily, and what that says about the state of unpatched systems |

### Campaign Analysis

| Report | Description |
|--------|-------------|
| [Campaign Reports](reports/) | One report per significant campaign, covering emergence, targeting, infrastructure, and withdrawal |
| [Suricata Signature Analysis](reports/11_suricata_signature_analysis.md) | What the top alert signatures tell us about attacker tooling and how they correlate with specific campaigns |

### Infrastructure and Attribution

| Report | Description |
|--------|-------------|
| [Geographic Attribution](reports/04_geographic_attribution.md) | Where attacks originated, with discussion of how cloud provider and VPN exit node usage affects attribution confidence |
| [Cloud Infrastructure Abuse](reports/09_cloud_infrastructure_abuse.md) | How much attack traffic came from DigitalOcean, Google Cloud, and Amazon, and what attackers are doing with rented infrastructure |
| [Threat Actor Assessment](reports/05_threat_actor_assessment.md) | What CVE selection, tooling signatures, and operational patterns suggest about the actors involved |

### Behavioral Analysis

| Report | Description |
|--------|-------------|
| [Credential Attack Patterns](reports/07_credential_attack_patterns.md) | The usernames and passwords that showed up most often, what they target, and which credential pattern dominated the deployment |
| [Port Targeting Analysis](reports/10_port_targeting_analysis.md) | Which services attracted the most traffic and how port targeting shifted as campaigns changed |

### Strategic Assessment

| Report | Description |
|--------|-------------|
| [Future Attack Trends](reports/06_future_attack_trends.md) | What the observed patterns suggest about where the threat landscape is heading, which vulnerabilities are gaining traction, and what defenders should be watching |

---

## A Note on Methodology

Data was collected daily using six Elasticsearch queries scoped to individual 24-hour windows. Internal Google Cloud VM traffic was excluded from all queries to keep counts clean. Everything is saved as raw JSON exactly as Elasticsearch returned it.

On March 1, 2026 a final full-dataset screenshot was taken before log retention started removing older indices. That screenshot serves as the cumulative reference for totals across the full deployment.

Full details are in [DATA_COLLECTION_METHODOLOGY.md](DATA_COLLECTION_METHODOLOGY.md).

---

## Tools Used

- [T-Pot Community Edition](https://github.com/telekom-security/tpotce)
- Elasticsearch 8.x / Kibana 8.x
- Suricata IDS
- Google Cloud Platform
- [Claude](https://claude.ai) (Anthropic) — mathematical and statistical analysis

---

## Disclaimer

This honeypot ran on infrastructure I own and operated for research purposes. Everything in this repo represents attack attempts against a system I deliberately exposed. No third-party systems were accessed or affected.
