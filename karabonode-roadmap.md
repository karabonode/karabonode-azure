# KaraboNode: 6-Month Roadmap (Final) — SOC Analyst + Azure & AD Administrator

Final, consolidated version. Supersedes all earlier drafts ("Northbridge Retail," the first KaraboNode pass). Scoped directly against the job postings you shared, with DevOps/Event Streaming deliberately deferred to a Phase 2 that starts after you're employed — see the note at the end for why.

## The two roles this targets, and one honest scorecard

**SOC Analyst (24/7 MSSP-style monitoring role).** Wants SIEM (you have a foundation), EDR (zero), email/phishing analysis (zero — a genuinely distinct skill from anything Azure teaches), CompTIA Security+ as the named essential cert, MITRE ATT&CK familiarity, and ticketing/shift-handover discipline.

**Junior Azure & Active Directory Specialist.** Wants on-prem AD (domain controllers, DNS, replication, Group Policy), hybrid identity sync (Entra Connect), Intune, and PowerShell — the on-prem/hybrid side that pure cloud-native Entra ID work doesn't cover.

*(One posting you shared, titled "Junior SOC Analyst" but actually describing contractor/workforce delivery coordination with ATS and LinkedIn Recruiter tools, is not a cybersecurity role and isn't part of this plan's technical prep.)*

| Target | Score /10 | Why |
|---|---|---|
| SOC Analyst | **3** | SIEM/KQL foundation real. EDR: 0. Phishing analysis: 0. Security+: not started. MITRE mapping: touched once, not habitual. |
| Azure & AD Specialist | **3** | General Azure comfort real (RBAC, VMs, networking, monitoring). On-prem AD DS, GPO, hybrid sync, Intune: all 0. PowerShell: minimal. |

## The KaraboNode story

**KaraboNode** — a mid-size company partway through modernizing from fully on-prem IT to a hybrid Azure setup, while standing up its first real security monitoring function. You play both roles it needs at once: the admin stabilizing and hybridizing its Active Directory, and the analyst monitoring the environment for threats as it modernizes. Realistic combination at a small/mid-size company, not a stretch.

Portfolio lives in a GitHub repo (`karabonode-platform`): infrastructure code, an `/incidents` folder for real investigation write-ups, a `/decisions` folder for short architecture/security decisions with reasoning, and by month 6 a full architecture diagram.

## How this plan actually works

**Pacing:** built for 15–20 hours/week — tight, not slack. If a week comes in light, don't compress the backlog into the next week; cut from the bottom of this priority order instead: the second DC/replication demo first, then the EDR trial, then extra TryHackMe reps. AD fundamentals, hybrid sync, Sentinel detections, phishing analysis, and both certs are the non-negotiable core.

**Cost control — this got underestimated the first time, so numbers this pass:**

| Item | Cost |
|---|---|
| AZ-104 exam voucher | ~$165 USD |
| CompTIA Security+ (SY0-701) exam voucher | ~$439 USD (price rose from $425 in June 2026 — confirm current price before booking) |
| Windows Server VM (B2s-class, Windows licensing included) | roughly $60–90/month **if left running continuously** — this is the real risk |
| Entra ID P1 trial | free, 30 days |
| Defender for Endpoint trial | free trial but requires a card on file and auto-converts to a paid subscription after the window — calendar the cancellation date the day you start it |

Total realistic cash outlay over 6 months: roughly **$650–750** for both certs plus VM compute, assuming you deallocate VMs when not actively using them rather than leaving them running 24/7. Don't run two Windows Server DCs plus a sync server continuously for six months — deallocate (not delete) the second DC right after the Month 1 replication exercise, and stop/start the sync server around when you're actually working with it rather than leaving it up full-time.

## Month 1 — On-prem Active Directory foundations

- **Week 1:** Deploy a Windows Server VM in Azure, add the AD DS role, promote it to a Domain Controller, create the `ad.karabonode.co.za` forest. Follow Microsoft's guidance to put the AD database/logs/SYSVOL (NTDS.dit) on a separate data disk from the OS disk, not the default — real-world best practice, not lab-only formality.
- **Week 2:** Configure DNS (AD-integrated zone), build out OUs, users, and groups reflecting a small company's structure.
- **Week 3:** Deploy a second DC to see **replication** in action (named explicitly in the posting) — force replication, then deliberately break and fix it once to understand real failure modes. Deallocate this second VM once the exercise is documented, per the cost note above.
- **Week 4:** Basic **Group Policy** (password policy, a login-script GPO, a restricted-software GPO). AD cleanup exercise: seed some intentionally stale accounts/permissions, then find and clean them up — mirrors the posting's "AD cleanup tasks" line directly.

**Deliverable:** a working (initially 2-DC, then scaled back to 1) on-prem-style AD forest, documented, with a cleanup write-up.

## Month 2 — Hybrid identity + Intune + PowerShell

- **Week 1:** Install **Microsoft Entra Connect Sync** on its own dedicated member server — never on a domain controller. Important current detail: as of July 2026 new versions are only distributed through the Entra admin center (Entra ID > Entra Connect > Get Started > Manage > Download Connect Sync Agent), not the old Download Center, and sync stops working after September 30, 2026 on anything older than version 2.5.79.0 — grab the current version from the admin center, not an old cached installer. Sync `ad.karabonode.co.za` to your Entra tenant.
- **Week 2:** Deliberately break sync once (stop the service, create a conflicting object) and practice diagnosing it — "support hybrid identity" day-to-day is mostly troubleshooting sync issues, not the initial setup.
- **Week 3:** **Microsoft Intune** — enroll a test device, push a basic compliance policy and a configuration profile.
- **Week 4:** **PowerShell** — write 3 real scripts (bulk AD user creation from a CSV, a stale-account report, an Azure resource tagging script using Az PowerShell). Run an AZ-104 practice assessment; sit the exam if the score supports it.

**Deliverable:** working hybrid sync, an Intune-managed test device, 3 PowerShell scripts in the repo, AZ-104 sat or booked.

## Month 3 — SOC/SIEM depth + MITRE discipline

- **Week 1:** Rebuild Sentinel against the now-larger environment — AD DS logs, Entra sign-ins, and Azure resource logs all in one workspace.
- **Week 2:** Write 5+ original analytics rules. For every one, tag the specific **MITRE ATT&CK technique ID** it detects, not just the tactic — build this as a habit, since it's named as desirable in the posting.
- **Week 3:** Simulate and detect an AD-specific attack relevant to a hybrid environment (e.g., a Kerberoasting attempt or suspicious replication request) using your AD logs.
- **Week 4:** Build a lightweight ticketing workflow (GitHub Issues, or a free tool like osTicket) and route every incident through it with a proper shift-handover note at the end. Begin **Security+** (SY0-701) study — start with General Security Concepts and Threats/Vulnerabilities/Mitigations, the two lighter-weighted domains, to build momentum.

**Deliverable:** 5+ original detection rules with MITRE mapping, one AD-attack scenario detected end-to-end, a ticketing/handover workflow in active use.

## Month 4 — EDR + email/phishing analysis

- **Week 1:** Sign up for the Microsoft Defender for Endpoint trial (card required, auto-converts — calendar the cancellation date now), onboard your lab VM, investigate a simulated alert.
- **Week 2:** Practice an endpoint isolation/containment action. If you'd rather skip the billing risk entirely, Microsoft's built-in attack simulation content shows the same investigation workflow without a live trial.
- **Week 3:** Phishing analysis — learn to read raw email headers (SPF/DKIM/DMARC results, Received chain, Reply-To mismatches) using free tools (MXToolbox header analyzer), and sandbox URL/attachment checks via VirusTotal and urlscan.io, practicing against publicly available phishing sample sets.
- **Week 4:** Write a phishing triage playbook covering "user reports suspicious email" through "confirmed phishing, contained, user notified." Continue Security+ (Security Architecture and Security Operations domains — the two heaviest-weighted).

**Deliverable:** 3+ documented phishing investigations, a written phishing playbook, one EDR investigation walkthrough.

## Month 5 — Integration, speed, and certification

- **Week 1–2:** Build and execute a multi-stage cross-domain scenario — a phishing email leads to a compromised on-prem AD account, detected via a suspicious Entra sign-in correlated with unusual AD activity. Document it as a full incident write-up.
- **Week 3:** Heavy rep practice — TryHackMe SOC Level 1 and/or LetsDefend, targeting phishing and endpoint modules specifically. Add timed response drills (mock alert, countdown, practice triaging under pressure — a trained skill, not just a personality trait).
- **Week 4:** Final Security+ review (Security Program Management and Oversight domain), sit the exam once practice scores are consistently strong.

**Deliverable:** one multi-stage cross-domain incident write-up, Security+ passed or booked.

## Month 6 — Capstone, packaging, and applying

- **Week 1:** Finish the full KaraboNode architecture diagram — on-prem AD, hybrid sync, Azure workloads, Sentinel/monitoring layer, all in one picture. Polish the repo README so it reads clearly for a non-technical skim and a technical deep-read.
- **Week 2:** Rewrite resume and LinkedIn using language pulled directly from the postings (SIEM, EDR, phishing analysis, MITRE ATT&CK, hybrid identity, Group Policy, Intune) — ATS systems and skimming recruiters respond to matched terminology.
- **Week 3:** Mock interview prep — rehearse concrete answers to "walk me through an incident you handled" and "how would you triage this alert" using your own documented incidents as material, not hypotheticals.
- **Week 4:** Apply broadly, including to postings stating "1+ years" — a strong portfolio plus Security+ plus demonstrable hands-on work gets many entry-level candidates through MSSP screening even when the years line reads as a hard requirement. Track applications, follow up, iterate based on interview feedback.

**Deliverable:** finished portfolio repo, capstone write-up, applications in motion.

## Progress tracker

- [ ] Month 1: AD forest built, replication demo done, GPOs configured, cleanup documented
- [ ] Month 2: Hybrid sync working, Intune device enrolled, 3 PowerShell scripts written, AZ-104 sat
- [ ] Month 3: 5+ MITRE-mapped detections, AD attack simulated + detected, ticketing workflow live
- [ ] Month 4: EDR investigation done, 3+ phishing write-ups, phishing playbook written
- [ ] Month 5: Multi-stage incident documented, Security+ sat
- [ ] Month 6: Architecture diagram done, resume/LinkedIn updated, applications going out

## Resources

- [AZ-104 exam page](https://learn.microsoft.com/en-us/credentials/certifications/exams/az-104/) — official skills outline and practice assessment link
- [CompTIA Security+ (SY0-701) exam page](https://www.comptia.org/certifications/security) — official objectives and voucher purchase
- [Install AD DS on an Azure VM](https://learn.microsoft.com/en-us/windows-server/identity/ad-ds/deploy/virtual-dc/adds-on-azure-vm) — official deployment guidance including the separate-disk recommendation
- [Microsoft Entra Connect download and setup](https://learn.microsoft.com/en-us/entra/identity/hybrid/connect/how-to-connect-install-express) — current download path (via Entra admin center, not the old Download Center)
- TryHackMe SOC Level 1 path, LetsDefend — for realistic incident reps
- MITRE ATT&CK Navigator (attack.mitre.org) — for technique mapping practice

## What's deliberately not in this plan: DevOps

You also shared postings for a Junior DevOps & Event Streaming role (Kubernetes, OpenShift, Kafka/NiFi/RabbitMQ, CI/CD, Grafana/Prometheus). Worth naming honestly: the more detailed of those postings asks for 3+ years' experience despite the "Junior" label — it's closer to a mid-level Platform/SRE role. That track is real and worth pursuing, but not inside this same 6 months: it's a genuinely separate specialization, your current footing there is close to zero, and splitting focus three ways risks three weak portfolios instead of two strong ones.

The plan: finish this 6-month track, use it to land a first role in SOC or Azure/AD administration, then treat DevOps as a deliberate Phase 2 — a year or so of real ops/security experience makes the Kubernetes/Kafka/CI-CD learning curve faster and far more credible than trying to fake it in a home lab with no job behind it. When you're ready to start that phase, come back and we'll scope it the same way this one was scoped.

## Realistic 6-month outcome

Genuinely competitive for junior Azure & AD Specialist roles (AZ-104 certified, real hybrid AD experience, PowerShell scripts to show) and meaningfully stronger for SOC Analyst roles (Security+ certified, real phishing/EDR investigation write-ups, MITRE-mapped detections) — even though the literal "1+ years" line in some postings isn't something six months of lab work can satisfy on paper. The portfolio and certs are what get you past that line in practice at the junior tier.
