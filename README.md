# SANED سانِد — National Social Protection Intelligence Platform

> Architecture documentation and interactive diagrams for the Sultanate of Oman's Social Protection & Welfare Intelligence Platform.

## 🛡️ Platform Overview

**SANED** (سانِد — "Supporter/Helper") is a sovereign, government-grade platform that integrates multi-ministry data into a secure lakehouse, utilizes configurable ML middleware for demographic segmentation and poverty indexing, and implements a governed agentic AI layer for grant delivery, CSR allocation, and policy analytics.

## 📐 Architecture Portal

The interactive architecture documentation is published via GitHub Pages:

**🔗 [View Architecture Portal →](https://YOUR-USERNAME.github.io/PASI/)**

### Available Diagrams

| # | Diagram | Description |
|---|---------|-------------|
| 01 | High-Level Architecture | 8-layer logical architecture overview |
| 02 | End-to-End Data Flow | Ministry source → ETL → Analytics serving |
| 03 | Lakehouse Medallion Zones | Bronze → Silver → Gold zone design |
| 04 | MLOps Pipeline | 6-phase ML lifecycle with governance |
| 05 | AI Agentic Platform | 5-tier agent architecture with RAG |
| 06 | Security & Governance | Sovereign security framework |
| 07 | CXO Comprehensive Dashboard | Consolidated executive view |
| 08 | HIMAYA Intelligence Platform | 7-layer intelligence architecture |
| 09 | **SANED Full-Stack Architecture** | Complete CoreX → Lakehouse → MLOps → AI stack |

## 🏗️ Technology Stack

- **Infrastructure**: CoreX Platform (Sovereign HW, GPU, Networking)
- **Data**: Medallion Lakehouse (Bronze/Silver/Gold)
- **ML**: AutoML, Feature Store, Model Registry
- **AI**: Agent Orchestration, RAG, Vector Search
- **Security**: Zero-Trust, AES-256/CMK, 100% Oman Data Residency

## 📦 Repository Structure

```
PASI/
├── docs/                  # GitHub Pages site (deployed)
│   ├── index.html         # Landing portal
│   ├── 404.html           # Custom 404 page
│   ├── .nojekyll          # Bypass Jekyll
│   └── diagrams/          # All architecture diagrams
├── architecture/          # Source architecture files
│   ├── ARCHITECTURE.md    # Master architecture document
│   └── diagrams/          # Source HTML diagrams
├── .github/workflows/     # CI/CD
│   └── deploy-pages.yml   # GitHub Pages deployment
└── social-protection-fund-prd.md  # Product requirements
```

## 🚀 Deployment

This site auto-deploys to GitHub Pages on every push to `main` that modifies the `docs/` folder.

### Manual Setup

1. Push this repo to GitHub
2. Go to **Settings → Pages**
3. Under **Build and deployment → Source**, select **GitHub Actions**
4. The workflow will auto-trigger on the next push

## ⚖️ Confidentiality

This repository contains sovereign architecture documentation for the Sultanate of Oman's Social Protection Fund. Handle with appropriate classification controls.

---

*Powered by CoreX Platform · v1.0*
