---
layout: writeup
title: "T-Pot Honeypot: Most Recently Disclosed CVE Observed - February 2026"
date: 2026-03-05
category: THREAT INTELLIGENCE
tags:
  - honeypot
  - T-Pot
  - GCP
  - threat-intelligence
  - cve-analysis
  - telnet
  - authentication-bypass
  - vulnerability-research
excerpt: "Analysis of CVE-2026-24061, the most recently disclosed CVE observed in the February 2026 T-Pot honeypot deployment, including observed activity, scoring data, and defender guidance."
---

# CVE-2026-24061: GNU InetUtils telnetd Authentication Bypass

**Deployment Period:** 2026/02/01 to 2026/02/28
**Honeypot:** T-Pot Community Edition, Google Cloud Platform
**Data Source:** Elasticsearch DSL queries, Kibana Dev Tools
**TLP:** TLP:CLEAR

---

## About This Report

This report is part of a hands-on project focused on building practical skills in threat data analysis and CVE research. It is one report in an ongoing series covering the February 2026 T-Pot deployment. I'm open to constructive feedback.

---

## Summary

CVE-2026-24061 is the most recently disclosed CVE observed in this dataset. It was published on 2026/01/21, less than two weeks before this deployment began. The honeypot recorded **9 events** across **4 days**, making it a low-volume but contextually notable finding. Exploitation attempts were visible within days of disclosure and continued into late February.

---

## CVE-2026-24061: GNU InetUtils telnetd Authentication Bypass

An argument injection flaw in GNU InetUtils `telnetd`. During Telnet `NEW_ENVIRON` option negotiation, the client can supply a `USER` environment variable. `telnetd` passes this value unsanitized to `/usr/bin/login` as a command-line argument. Sending `USER` as `-f root` causes `login` to skip authentication entirely and grant a root shell. No credentials are required. No user interaction is required. The flaw was introduced in a 2015 commit and went undetected for over 10 years.

**Affected products:** GNU InetUtils `telnetd` versions 1.9.3 through 2.7. Fixed in 2.7-2, patches released 2026/01/20.

| Metric | Value | Source |
|--------|-------|--------|
| CVSS v3.1 | 9.8 (AV:N/AC:L/PR:N/UI:N/S:U/C:H/I:H/A:H) | NVD |
| EPSS | 75.27% | Shodan CVEDB |
| CISA KEV | Yes, added 2026/01/26 | CISA |
| Ransomware | Unknown | Shodan CVEDB |

**Observed activity:** **9 total events** across 4 active days. The first event appeared on 2026/02/01, 11 days after disclosure. Day 1 accounted for **6 of the 9 events**, the highest single-day count. The remaining 3 events were spread across 2026/02/04, 2026/02/06, and 2026/02/20.

**Assessment:** This CVE was under two weeks old when the deployment started. Scanners were probing for it within 11 days of public disclosure. GreyNoise documented exploitation attempts from multiple countries starting within 18 hours of the original announcement. The CISA KEV addition on 2026/01/26 confirms real-world exploitation was already underway before this honeypot collected its first event. The low event count here is consistent with a niche target surface. `telnetd` is not common on modern internet-facing hosts, so scanning campaigns for this CVE reach fewer responding services than broader web application scans. The declining activity after day 1 may reflect reduced scanning cadence as automated tools moved on.

**Defender note:** Patch to GNU InetUtils 2.7-2 or later. If `telnetd` is not needed, disable it entirely. If you manage embedded Linux devices, network appliances, or OT systems running older distributions, check whether `inetutils-telnetd` is installed. CISA's KEV deadline for federal agencies was 2026/02/16.

---

## Data Reference

### Table 1: CVE-2026-24061 Daily Event Counts

| Day | Date | Events |
|-----|------|--------|
| 01 | 2026/02/01 | 6 |
| 02 | 2026/02/02 | 0 |
| 03 | 2026/02/03 | 0 |
| 04 | 2026/02/04 | 1 |
| 05 | 2026/02/05 | 0 |
| 06 | 2026/02/06 | 1 |
| 07-19 | 2026/02/07 to 2026/02/19 | 0 |
| 20 | 2026/02/20 | 1 |
| 21-28 | 2026/02/21 to 2026/02/28 | 0 |
| **Total** | | **9** |

---

## Methodology Notes

**Event counts reflect IDS alerts, not confirmed exploitation.** A Suricata signature match means inbound traffic matched a known attack pattern. The honeypot is not a real `telnetd` host, so these are reconnaissance or probing attempts, not successful root access.

**Days with 0 events are omitted from the main table rows** but retained in the collapsed row above for completeness.

---

*All data sourced from Elasticsearch DSL queries against T-Pot honeypot logs, 2026/02/01 through 2026/02/28. Raw query output is preserved in [honeypot-threat-research/data](https://github.com/Aeronique/honeypot-threat-research/tree/main/data).*

---

## References

| Source | URL |
|--------|-----|
| NVD: CVE-2026-24061 | https://nvd.nist.gov/vuln/detail/CVE-2026-24061 |
| CVE Record: CVE-2026-24061 | https://www.cve.org/CVERecord?id=CVE-2026-24061 |
| CISA KEV Catalog | https://www.cisa.gov/known-exploited-vulnerabilities-catalog |
| oss-security: GNU InetUtils telnetd disclosure | https://www.openwall.com/lists/oss-security/2026/01/19/1 |
| GNU bug-inetutils: Patch announcement | https://lists.gnu.org/archive/html/bug-inetutils/2026-01/msg00004.html |
| Codeberg: Fix commit (sanitize USER variable) | https://codeberg.org/inetutils/inetutils/commit/fd702c02497b2f398e739e3119bed0b23dd7aa7b |
| Shodan CVEDB: CVE-2026-24061 | https://cvedb.shodan.io/cve/CVE-2026-24061 |
| GreyNoise Labs: 18 Hours of Unsolicited Telnet Houseguests | https://www.labs.greynoise.io/grimoire/2026-01-22-f-around-and-find-out-18-hours-of-unsolicited-houseguests/ |
| The Hacker News: Critical GNU InetUtils telnetd Flaw | https://thehackernews.com/2026/01/critical-gnu-inetutils-telnetd-flaw.html |
| Horizon3.ai: CVE-2026-24061 Rapid Response | https://horizon3.ai/attack-research/vulnerabilities/cve-2026-24061/ |
| Ubuntu Security: CVE-2026-24061 | https://ubuntu.com/security/CVE-2026-24061 |
