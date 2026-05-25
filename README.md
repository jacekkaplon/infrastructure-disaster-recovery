# Automated Cross-Site Infrastructure Backup & Disaster Recovery System

A production-grade, automated Disaster Recovery (DR) and configuration backup pipeline engineered to protect multi-site hybrid infrastructures. The framework utilizes a secure, decentralized architecture to orchestrate stateful backups across public cloud environments (VPS nodes) and local storage clusters (Proxmox VE/TrueNAS hypervisors), implementing storage deduplication and cryptographic transport verification.

<br>

## 🛡️ Architecture & Security Design Principles

Unlike traditional, high-risk backup configurations where edge nodes push data to a central storage container, this system implements a hardened **Pull-Method Architecture** combined with storage-optimized delta monitoring:

```text
+-----------------------+                    +-----------------------+
|   PUBLIC CLOUD VPS    |                    | LOCAL PROMOX / NAS    |
|                       |     SSH Tunnel     |                       |
|  [Production State]   | <================= |   [Backup Core Node]  |
|  Config Files & DBs   |   (Secure Pull)    |  Hardlinks/rsync/Cron |
+-----------------------+                    +-----------------------+
           │                                             │
           ▼                                             ▼
   Compromise Hazard:                             Immune to Attack:
   If VPS is breached,                         Attacker cannot access,
   attacker has ZERO                           modify, or wipe historical
   access to Backup Node.                      backup generations.
```

🔒 Hardened Pull-Method Execution – The backup jobs are fully initiated and driven by the internal, secure storage node. The public-facing cloud VPS holds no cryptographic keys or credentials to the backup environment. In the event of a total virtual server root compromise, historical backup retention states remain fully isolated and immune to destructive modification or ransom wiping.

⚡ Hardlink-Based Storage Deduplication – Leverages precise rsync differential synchronization backed by filesystem hardlinks. Identical, unchanged configuration fields across multi-generational retention windows consume zero additional disk sectors, establishing a lightweight snapshotting ledger without the performance overhead of full filesystem clustering.

⚙️ Stateful Error Handling & Lifecycle Traps – Implements robust Bash automation scripts using active signal trapping (trap), granular exit status tracking, and strict administrative alert mechanisms to capture storage boundary exhaustion or transient packet drop failures.

🔬 System Documentation & Blueprints
The technical architectural design, script workflows, and restoration drills are documented across two core engineering blueprints included inside this repository:

📄 1. Automated Cross-Site Backup & Restore Specification
Description: Enterprise-level disaster recovery handbook detailing global backup scheduling, retention rotation variables, and step-by-step restoration verification playbooks to ensure minimal Recovery Time Objective (RTO) inside virtualized topologies.


👉 Download Cross-Site Backup Specification (PDF)

📄 2. Selective Cloud Config Backup Blueprint (Pull-Method)
Description: Deep technical deployment guide focused on the selective "Pull" synchronization methodology. Documents precise user privileges mitigation, target directories grouping, and automation script validation procedures.


👉 Download VPS Pull-Method Blueprint (PDF)

Disclaimer: This repository is part of a secure home-laboratory infrastructure designed exclusively for studying system resilience, data deduplication mechanics, and advanced Disaster Recovery prototyping.
