# RepoSight Tracker

> **Background analytics collector for RepoSight**

RepoSight Tracker is a lightweight GitHub Actions–based repository that collects GitHub traffic analytics for your selected repositories. It stores this data in your Supabase project so RepoSight can visualize trends over time.

This repository runs quietly in the background once set up.

---

## 📋 Table of Contents

- [Purpose](#purpose)
- [Architecture](#architecture)
- [How Data Collection Works](#how-data-collection-works)
- [Setup](#setup)
  - [Required Secrets](#required-secrets)
  - [Configuring Repositories](#configuring-repositories)
- [GitHub Actions Workflow](#github-actions-workflow)
- [Data Ownership & Security](#data-ownership--security)
- [Troubleshooting](#troubleshooting)
- [Related Repository](#related-repository)
- [License](#license)

---

## 🎯 Purpose

RepoSight Tracker is responsible for:

- 📊 Fetching GitHub traffic data (views, clones, referrers, paths)
- 🔄 Capturing the initial set of available data
- ⏰ Running a daily sync to store new analytics
- 💾 Pushing normalized records to Supabase

**Note:** This repository has **no UI**. All visualization and management happen in the RepoSight App.

---

## 🏗 Architecture

RepoSight uses two components working in tandem:

```
┌─────────────────────┐         ┌──────────────────────────┐
│   RepoSight App     │ ◄────── │  RepoSight Tracker       │
│                     │         │                          │
│ • Dashboard         │         │ • Data Collection        │
│ • Settings          │         │ • GitHub Actions         │
│ • Analytics         │         │ • Background Jobs        │
└─────────────────────┘         └──────────────────────────┘
         │                                   │
         │                                   │
         └───────────► Supabase ◄────────────┘
```

**Setup:** Fork this tracker repository once. After secrets are added, it begins collecting analytics automatically.

---

## 🔄 How Data Collection Works

### Initial Activation
When the workflow runs for the first time, it collects all available GitHub traffic data from the last few days and stores it in Supabase.

### Daily Updates
A scheduled job syncs new analytics once a day and appends them to your dataset.

### Stored Metrics
- 👁️ Views
- 👤 Unique views
- 📥 Clones
- 🔢 Unique clones
- 🔗 Referrer domains
- 📄 Popular paths

Each snapshot is saved with timestamps so RepoSight can chart trends over time.

---

## ⚙️ Setup

### Required Secrets

Add these secrets to **your fork** of this repository:

| Secret Name | Description |
|-------------|-------------|
| `GITHUB_PAT` | Personal Access Token with read access to repositories being tracked |
| `SUPABASE_URL` | Your Supabase project URL |
| `SUPABASE_SERVICE_ROLE_KEY` | Service role key used by GitHub Actions for inserts |

**Configuration Location:**  
`Settings → Security → Actions secrets and variables → Actions`

---

### Configuring Repositories

Repositories to track are defined in:

```
config/repos.json
```

**Example:**

```json
{
  "repositories": [
    "username/project-one",
    "username/project-two"
  ]
}
```

You can modify this list anytime to add or remove projects.

> **Note:** The tracker repository itself is not meant to be listed here.

---

## 🤖 GitHub Actions Workflow

This repository includes a workflow that:

- ⏱️ Runs once per day using cron
- ▶️ Can be triggered manually for testing
- 🔄 Handles both the initial sync and daily updates
- 🔌 Communicates with Supabase through its REST API

You normally won't need to modify the workflow file.

---

## 🔒 Data Ownership & Security

- ✅ All analytics are stored in **your own** Supabase project
- 🔐 GitHub Actions use your secrets securely
- 📖 The RepoSight App only **reads** analytics; it never modifies data
- 👤 You retain full control over your data, repository, and infrastructure

---

## 🛠 Troubleshooting

If analytics don't appear in RepoSight:

1. ✔️ Confirm GitHub Actions ran successfully in your fork
2. 🔑 Verify all required secrets are present
3. 🔓 Ensure your GitHub PAT has read access to the repositories listed in `config/repos.json`

Most issues will be highlighted inside the RepoSight App.

---

## 🔗 Related Repository

**[RepoSight App](https://github.com/username/reposight-app)**  
Visualization and analytics dashboard for your collected GitHub metrics.

---

## 📄 License

MIT License