# 🚀 CodeUp Riwi: Logistics System Documentation

> **Project:** CodeUp Riwi: Beyond Limits — Competitive Version (Crudzaso)  
> **Team:** Multidisciplinary (C# & Java)

## 📖 Overview

Welcome to the official documentation repository for the **Advanced Logistics System**. This repository serves as the **single source of truth** for our project's technical and functional knowledge base.

It is designed to be automatically synchronized with **Wiki.js**, providing a centralized and navigable platform for developers, DevOps engineers, and end-users.

## 🏗️ Repository Structure

This repository is organized to map directly to our Wiki.js navigation tree:

```text
/
├── 01-ARCHITECTURE/        # High-level decisions, diagrams, and patterns
├── 02-SERVICES/            # Technical specs for C# Back, Java Microservice, Frontend, & Mobile
├── 03-DEVOPS-CI-CD/        # Deployment flow, VPS config, and Observability (Grafana/Prometheus)
└── 04-USER-MANUAL/         # End-user guides for Admin Web and Mobile App
```

## 🛠️ Tech Stack

This project integrates a diverse set of technologies:

| Area | Technology | Role |
|------|------------|------|
| **Backend** | .NET (C#) | Main Orchestrator & API |
| **Microservice** | Java | IA & Route Optimization Algorithms |
| **Frontend** | [Framework] | Responsive Web Application |
| **Mobile** | .NET MAUI/Xamarin | Field Operations App |
| **DevOps** | Docker, Portainer | Containerization & Orchestration |
| **Observability** | Prometheus, Grafana | Metrics & Monitoring |
| **Docs** | Wiki.js | Knowledge Management |

## 🤝 Contribution Guidelines

All team members must follow these rules to maintain a clean documentation structure:

1.  **Language**: All documentation must be written in **English**.
2.  **Format**: Use standard **Markdown**.
3.  **Hierarchy**: Do not change the folder names or numerical prefixes (e.g., `01-`, `02-`) as they determine the sort order in Wiki.js.
4.  **Images**: Place images in an `/assets` folder relative to your document or use an external host if configured.

### How to add a new page?

1.  Navigate to the relevant section (e.g., `02-SERVICES/CSharp-Backend`).
2.  Create a new `.md` file with a descriptive name.
3.  Include the standard frontmatter at the top:

```yaml
---
title: My New Page
description: A short description of this page's content.
---
```

## 🚀 Deployment & Sync

This documentation is deployed automatically:

1.  Push changes to the `main` branch.
2.  **Wiki.js** detects the commit via git hook/polling.
3.  Changes are rendered immediately on the Wiki portal.

---

*Generated for CodeUp Riwi Project Integration.*
