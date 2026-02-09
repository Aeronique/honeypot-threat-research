# Honeypot Threat Research

A 31-day experiment setting up a honeypot to see what happens when you expose vulnerable services to the internet.

## Project Summary

I deployed a T-Pot honeypot on Google Cloud Platform for a month (December 31, 2025 to January 30, 2026) to capture real attack traffic. Over 31 days, the honeypot logged 5.5 million attack events from all over the world.

### Goals

- Understand how attack campaigns work from start to finish
- Identify actively targeted vulnerabilities
- Learn to spot campaign patterns in large datasets
- Practice writing threat intelligence reports

## Deployment Details

**Infrastructure:**  
Platform: T-Pot Community Edition (free, packages multiple honeypots together)  
Server: Ubuntu 24.04 LTS on Google Cloud Platform  
Runtime: 31 days continuous  
Total Events: ~5.5 million  

### Honeypot Components

T-Pot bundles several different honeypots into one system. Each honeypot pretends to be a different vulnerable service:

- **Cowrie** simulates SSH and Telnet servers
- **Honeytrap** handles multiple network protocols
- **Dionaea** is designed to capture malware
- **Sentrypeer** focuses on VoIP/SIP attacks
- Plus several others for different attack types

## Repository Structure (Tenative)

```
/reports
  - campaign-analysis.md          
  - vulnerability-exploitation.md 
  - infrastructure-tracking.md    
  - credential-intelligence.md    

/visualizations
  - Dashboard screenshots
  - Attack maps and geographic distributions
  - Temporal pattern charts

/indicators
  - High-confidence IOCs
  - Suricata signatures
  - ASN and IP listings
```

## Analysis Methodology

Daily snapshots were collected at consistent intervals from Kibana dashboards. Each day's analysis tracked attacker ASNs, source IPs, geographic distribution, port targeting, Suricata signatures, CVE exploitation attempts, and credential patterns.

Campaigns were identified through correlation analysis across multiple metrics: sudden volume spikes, sustained presence from specific infrastructure, metric correlation (geography + ASN + attack type), and clean campaign transitions.

The challenge was distinguishing coordinated campaigns from coincidental timing. Attribution decisions were made by correlating geography, infrastructure (ASN), and attack characteristics.

## Technical Infrastructure

**Analysis stack:**
- Elasticsearch for event storage and querying
- Kibana for dashboard visualization
- Google Cloud Storage for automated backups
- Suricata for signature detection

**System management:**
- Automated nightly backups to Google Cloud Storage
- Health monitoring scripts for early problem detection
- Proactive component restarts during high-volume periods to prevent data loss

## Background

I have operational and tactical intellignce experience from previous work, but this was my first project focused specifically on operational cyber threat intelligence. The goal was to learn CTI fundamentals by deploying infrastructure, processing attack data at scale, and identifying patterns in millions of events.

Working through 5.5 million individual events to identify discrete campaigns was more difficult than expected, but taught me how to extract significant intelligence from large security datasets.

## Reports

Detailed analysis reports are in development, covering:
- Campaign lifecycles and operational patterns
- Vulnerability exploitation analysis
- Infrastructure tracking and attribution
- Credential intelligence and common patterns
- Defensive recommendations based on observed TTPs

These will be added to `/reports` as completed.

---

**Questions?** Open an issue if you want to know more about the methodology or specific campaign analysis.

**Analysis Period:** December 31, 2025 to January 30, 2026
