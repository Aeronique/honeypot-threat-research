# T-Pot Honeypot Data Collection Methodology
**February 1, 2026 to February 28, 2026**
*honeypot-threat-research*

---

## Overview

This document describes the data collection methodology used for the 28-day T-Pot honeypot deployment. All attack statistics, CVE counts, source IP data, and ASN totals in this project are derived directly from Elasticsearch queries run against live honeypot data, executed through the Kibana Dev Tools console. This approach produces verifiable, reproducible results that are not dependent on dashboard screenshot timing or UI rendering limitations.

---

## Technology Stack

| Component | Role |
|-----------|------|
| T-Pot Community Edition | Honeypot framework running on Ubuntu 24.04 LTS, Google Cloud Platform |
| Elasticsearch | Backend database storing all captured attack events, indexed by timestamp |
| Kibana | Visual interface for dashboards and the Dev Tools query console |
| Suricata | Intrusion detection engine providing CVE identification and alert signatures |
| Kibana Dev Tools | Built-in Elasticsearch query console used to run all DSL queries in this project |
| Claude (Anthropic) | Used to assist with mathematical and statistical analysis of collected data |

---

## Query Methodology

All data in this project was collected using Elasticsearch DSL (Domain Specific Language) queries. Each query targets the `logstash-*` index pattern, which is where T-Pot writes all honeypot event data. Queries are scoped to a single 24-hour window using the `@timestamp` field, ensuring daily counts are clean and do not overlap.

A consistent filter excludes the internal Google Cloud VM IP address (`10.128.0.2`) from all queries. Without this filter, internal VM traffic would appear as the top source IP and inflate counts. Every query in this project applies this exclusion.

### Daily Query Set

The following six queries are run each day. Only the date values are updated between days. All other parameters remain identical across the 28-day deployment period.

| Query | What It Returns |
|-------|----------------|
| Total Count | Single integer representing all attack events recorded that day, excluding internal IP |
| Top ASNs | Top 20 autonomous system numbers by event count, showing which networks sent the most traffic |
| Top Source IPs | Top 20 individual attacker IP addresses by event count |
| Top CVEs | Up to 50 CVE-tagged Suricata signatures matched that day, showing active vulnerability exploitation attempts |
| Top Signatures | Top 20 Suricata alert signatures by frequency, covering all alert categories |
| Top Ports | Top 20 destination ports targeted by attackers that day |

### Query Structure

All queries share the same base structure. The `bool` query combines a `range` filter on `@timestamp` with a `must_not` exclusion on the internal IP. Aggregations are added per query type. The `size` field is set to `0` on search queries because only aggregation results are needed, not individual event records.

```json
GET logstash-*/_search
{
  "size": 0,
  "query": {
    "bool": {
      "must": [
        {
          "range": {
            "@timestamp": {
              "gte": "2026-02-01T00:00:00",
              "lte": "2026-02-01T23:59:59"
            }
          }
        }
      ],
      "must_not": [
        {
          "term": {
            "src_ip.keyword": "10.128.0.2"
          }
        }
      ]
    }
  },
  "aggs": { ... }
}
```

---

## Data Storage and File Naming

Each day's query results are saved as a separate JSON file using the following naming convention:

```
tpot_day01_2026-02-01.json
tpot_day02_2026-02-02.json
tpot_day03_2026-02-03.json
```

Zero-padding the day number ensures files sort correctly in chronological order in any file explorer. Each file contains the raw JSON output from all six queries for that day, labeled by query type. No data is modified or summarized before saving.

---

## Data Collection Limitations

The following limitations apply to this dataset and are disclosed in the interest of analytical transparency:

- Top 20 aggregations capture the highest-volume sources only. Long-tail sources below the cutoff are counted in total event figures but are not individually identified.
- Elasticsearch log retention is 30 days by default. The final full-dataset query on March 1, 2026 captures all 28 days of this deployment before any indices roll off.
- CVE identification depends on Suricata ruleset coverage. Exploitation attempts against vulnerabilities without matching Suricata rules will appear in total event counts but will not be attributed to a specific CVE.
- Geographic attribution relies on MaxMind GeoIP database accuracy, which has known limitations for cloud provider IP ranges and VPN exit nodes.

---

## Lessons Learned from First Deployment

Kibana dashboard screenshots were used as the primary collection method during the prior 31-day deployment. That approach introduced inconsistencies because screenshot timing affected which events had been indexed, multi-day windows appeared in some screenshots, and cumulative totals could not be independently verified after log retention removed older indices.

Direct Elasticsearch queries solve all of these problems. Every number in this dataset is reproducible by running the same query against the same index with the same date range. The raw JSON output is preserved exactly as Elasticsearch returned it, with no manual transcription or rounding.

---

## Final Dataset Verification

On March 1, 2026, a final full-dataset Kibana screenshot was taken with the date range set to February 1 through February 28, 2026. This screenshot serves as a single-source verification of cumulative totals and provides a visual summary that complements the daily JSON records. The screenshot total event count, top ASNs, top CVEs, and top signatures were compared against the summed daily query results to confirm data integrity before publication.
