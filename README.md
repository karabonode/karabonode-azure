# KaraboNode — Hybrid AD & SOC Home Lab

This repo is the evidence trail of a 6-month, self-directed lab I'm running to get job-ready for two specific roles: **Junior Azure & Active Directory Specialist** and **SOC Analyst**. Not a tutorial follow-along — I scoped the plan directly against real job postings, and every week maps back to a line in one of them.

The premise: **KaraboNode** is a fictional mid-size company partway through modernising from fully on-prem IT to a hybrid Azure setup, while standing up its first real security monitoring function. I play both roles it needs — the admin building and hybridising its Active Directory, and the analyst monitoring the environment as it changes.

The full plan, including the honest starting-point scorecard and cost breakdown, is in [karabonode-roadmap.md](karabonode-roadmap.md).

## What's actually in here

- **`evidence/` folders** — day-by-day write-ups of the work as I complete it: what I built, the commands I ran, what broke, and what I'd do differently. These are working notes, not polished blog posts — that's the point.
- **[karabonode-roadmap.md](karabonode-roadmap.md)** — the 6-month roadmap the daily work follows.

The environment so far: a Windows Server VM in Azure (`dc01-karabonode`, B-series, deallocated when I'm not working — compute costs are real), promoted to the first domain controller of the `ad.karabonode.co.za` forest, with the AD database/logs/SYSVOL on a separate data disk per Microsoft's guidance rather than the lab-default OS disk.

## Progress

✅ done · 🔵 in progress · ⬜ not started

### Month 1 — On-prem Active Directory foundations
- ✅ **Week 1 — First domain controller:** Azure networking planned, VM deployed and secured, data disk prepared, AD DS installed, DC promoted, forest created and documented
- 🔵 **Week 2 — DNS, OUs, users & groups:** DNS + SRV records verified ✅ · OU structure built ✅ · users created ✅ · security groups + AGDLP applied ✅ · week consolidation ⬜
- 🔵 **Week 3 — Second DC & replication:** dc02 deployed ✅ · DNS pointed and domain-joined ✅ · force replication, break/fix ⬜
- ⬜ **Week 4 — Group Policy & AD cleanup:** password/login-script/software-restriction GPOs, stale-account cleanup exercise

### Month 2 — Hybrid identity, Intune, PowerShell
- 🔵 **Week 1 — Entra Connect sync setup:** sync server deployed ✅ · prerequisites and current agent downloaded ✅ · Express setup installed ✅ · sync verified ✅ · write-up ⬜
- ⬜ **Week 2 — Break/fix sync & identity concepts**
- ⬜ **Week 3 — Intune enrollment**
- ⬜ **Week 4 — PowerShell scripting & AZ-104 prep**

### Month 3 — SOC/SIEM depth + MITRE
- ⬜ Sentinel rebuilt over the full environment, 5+ original analytics rules with MITRE ATT&CK technique IDs, AD attack simulated and detected, ticketing workflow

### Month 4 — EDR + phishing analysis
- ⬜ Defender for Endpoint investigation, raw header analysis (SPF/DKIM/DMARC), 3+ documented phishing investigations, written triage playbook

### Month 5 — Integration + Security+
- ⬜ Multi-stage cross-domain incident (phish → compromised AD account → correlated detection), timed triage drills, Security+ (SY0-701) exam

### Month 6 — Capstone + applying
- ⬜ Full architecture diagram, portfolio polish, applications out

## Certifications targeted

- **AZ-104** (Microsoft Azure Administrator) — Month 2
- **CompTIA Security+ (SY0-701)** — Month 5

---

*The daily task files driving this stay local; what's published here is the proof of work. If you're a recruiter or hiring manager reading this: the evidence folders are the honest version of "hands-on experience" — including the parts that went wrong.*
