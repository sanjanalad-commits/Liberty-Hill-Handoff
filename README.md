# Liberty-Hill-Handoff


> Data infrastructure and transformation scripts powering Liberty Hill Foundation's CBO Portal — connecting CiviCRM, BigQuery, Tableau dashboards, and Asana workflows.

---

## 🏗️ Architecture

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                          LIBERTY HILL CRM                        │
└─────────────────────────────────────────────────────────────────────────────┘

     ┌─────────────┐         ┌─────────────┐         ┌─────────────┐
     │   CallHub   │         │  BigQuery   │         │   Tableau   │
     │   (Source)  │────────▶│  (Storage)  │────────▶│ (Dashboards)│
     └─────────────┘         └──────┬──────┘         └─────────────┘
                                    │                       │
                                    │                       ▼
     ┌─────────────┐         ┌──────▼──────┐         ┌─────────────┐
     │    Asana    │◀────────│   CiviCRM   │         │  3 Dashboards│
     │ (Workflows) │         │   (Portal)  │         │  CBO/Funder/ │
     └─────────────┘         └──────┬──────┘         │   Internal   │
                                    │                └─────────────┘
                                    ▼
                             ┌─────────────┐
                             │  OnlyOffice │
                             │   (Docs)    │
                             └─────────────┘
```

---

## 📁 Repository Structure

```
liberty-hill-cbo-portal/
│
├── README.md                      # You are here
│
├── docs/
│   ├── tech-stack.md              # Infrastructure, credentials, commands
│   ├── data-sources.md            # BigQuery tables & data flow
│   └── crm-setup.md               # CiviCRM VM, cron jobs, Bitbucket PR link
│
├── data-transformations/          # Data transformation scripts
│   ├── LH_cleaned.R               # CallHub → BigQuery replication
│   └── [additional scripts]       # Other transformation scripts
│
└── config/
    └── cron-schedules.md          # All scheduled jobs documentation
```

---

## 🚀 Quick Links

| Resource | Link |
|----------|------|
| **CiviCRM Portal** | http://34.94.236.4/civicrm/home |
| **GCP Console** | [liberty-hill-462819](https://console.cloud.google.com/home/dashboard?project=liberty-hill-462819) |
| **Tableau Dashboards** | [Tableau Online](https://us-west-2b.online.tableau.com) |
| **CRM Files (Bitbucket)** | [Bitbucket PR](#) ⚠️ *Add link* |

---

## 📊 System Components

### CiviCRM Portal
Web-based CRM for CBO management, contact tracking, and workflow automation.
- **VM:** `civicrm-vm` (34.94.236.4)
- **Database:** Cloud SQL MySQL 8.0
- **[Full Setup Guide →](docs/crm-setup.md)**

### Tableau Dashboards
Three dashboards for different audiences:
| Dashboard | Audience | Purpose |
|-----------|----------|---------|
| CBO Dashboard | Community Organizations | Track their own metrics |
| Funder Dashboard | Funders | Monitor grant performance |
| Internal Dashboard | Liberty Hill Staff | Complete operational view |

### BigQuery Data Warehouse
Central data storage with automated replication from CallHub.
- **Project:** `liberty-hill-462819`
- **Dataset:** `liberty_hill_replica`
- **[Data Sources Guide →](docs/data-sources.md)**

### Asana Integration
Automated workflow for:
- Campaign requests from CBOs
- Invoice submissions and approvals

---

## 🔄 Data Flow

```
1. CallHub (LH's data)
       │
       ▼ [Hourly Replication - LH_cleaned.R]
       │
2. BigQuery (sql-mirror-db → liberty-hill-462819)
       │
       ▼ [Transformation Scripts]
       │
3. Tableau Dashboards (CBO / Funder / Internal)
```

---

## 📚 Documentation

| Document | Description |
|----------|-------------|
| [Tech Stack](docs/tech-stack.md) | Infrastructure, credentials, common commands |
| [Data Sources](docs/data-sources.md) | BigQuery tables, dashboards, data lineage |
| [CRM Setup](docs/crm-setup.md) | CiviCRM configuration, VMs, cron jobs |
| [Cron Schedules](config/cron-schedules.md) | All automated job schedules |

---

## 👥 Team Contacts

| Role | Name | Responsibility |
|------|------|----------------|
| Data Manager | Birdie | BigQuery, transformations, dashboards |
| CRM Development | Sanjana | CiviCRM setup, integrations |

---

## ⚠️ Important Notes

- **CRM Files:** All CRM customization files are in the [Bitbucket Pull Request](#). Detailed explanations are in the PR comments.
- **Credentials:** See [Tech Stack](docs/tech-stack.md) for all access credentials (keep secure!)
- **Data Replication:** Runs hourly via cron on `liberty-hill-replication-vm`

---

## 🛠️ Getting Started

### Access the CiviCRM Portal
```bash
# SSH into CiviCRM VM
gcloud compute ssh civicrm-vm --zone=us-west2-c --project=liberty-hill-462819
```

### Access the Replication VM
```bash
# SSH into Replication VM
gcloud compute ssh liberty-hill-replication-vm --zone=us-central1-a --project=liberty-hill-462819
```

### Run Manual Data Replication
```bash
cd /home/sanjana_lad && Rscript LH_cleaned.R
```

---

*Last Updated: February 2025*
