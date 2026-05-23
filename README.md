# HR Analytics AI Hub & Management System

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg?style=for-the-badge)](https://opensource.org/licenses/MIT)
[![Google Apps Script](https://img.shields.io/badge/Google_Apps_Script-4285F4?style=for-the-badge&logo=google&logoColor=white)](https://developers.google.com/apps-script)
[![Gemini API](https://img.shields.io/badge/Gemini_API-10b981?style=for-the-badge&logo=google&logoColor=white)](https://ai.google.dev/)
[![ES6](https://img.shields.io/badge/ES6-F7DF1E?style=for-the-badge&logo=javascript&logoColor=black)](https://www.ecma-international.org/ecma-262/6.0/)

An advanced, serverless Single-Page Application (SPA) built on Google Apps Script. This tool serves as a comprehensive **HR Management and Analytics System**. It empowers HR professionals to upload raw CSV, Excel, or SQLite database files, parses and manages data entirely in-memory for maximum privacy and performance, and hosts a custom-prompted **Gemini AI chatbot** alongside automated insights, database views, and org simulation pages.

---

## Table of Contents
- [Architecture & Engineering Approach](#architecture--engineering-approach)
- [Core Features & Tabs](#core-features--tabs)
- [Setup & Local Development](#setup--local-development)
- [Configuration & Security](#configuration--security)
- [Deployment](#deployment)
- [Repository Structure](#repository-structure)
- [Contributing](#contributing)
- [License](#license)

---

## Architecture & Engineering Approach

This project bypasses traditional database latency in Apps Script by relying on a browser-first, in-memory architecture:

* **Multiformat Client-side ETL**: Ingests and processes data from **CSV** (via PapaParse), **Excel** (via SheetJS), and **SQLite databases** (via sql.js WebAssembly compilation) directly in the user's browser.
* **Reactive State Management**: Modifications in the Employee Database or simulations in the Org Simulator trigger an automatic re-evaluation of KPIs, averages, and Chart.js visualizations in real-time.
* **Secure AI Pipelines**: Utilizes the Google Apps Script `PropertiesService` to store the Gemini API key securely in Google's cloud infrastructure, separating credentials from code while maintaining a direct client-facing service endpoint.

---

## Core Features & Tabs

The application features a modern tabbed layout dividing HR actions into dedicated pages:

1. **Workforce Dashboard**:
   - High-level KPIs: Active Headcount, Average Compensation, Department Count, and Attrition Rate.
   - Dynamic charts powered by Chart.js (Department Distribution, Gender Diversity, and Compensation Curves).
2. **Employee Database**:
   - A fully searchable, filterable data grid listing employee records.
   - In-memory CRUD operations: Add new employees, edit existing records (salaries, departments, statuses), or delete records with immediate dashboard updates.
3. **Insights Hub**:
   - Auto-generated pay equity assessments and gender pay gap calculations.
   - Attrition risk highlights identifying high-turnover department hotspots.
   - **Executive AI Report**: A one-click button that prompts Gemini to synthesize the current session data into a structured executive brief.
4. **Org Simulator**:
   - Real-time simulation of department-wide salary adjustments using interactive range sliders.
   - "Live Impact Card" detailing how simulated actions affect total payroll, average wages, and budget headroom before making changes permanent.
5. **AI Analyst Panel**:
   - A persistent sidebar assistant capable of answering natural language queries concerning the ingested workforce database.

---

## Setup & Local Development

This repository uses [clasp (Command Line Apps Script Projects)](https://github.com/google/clasp) for local development.

### 1. Prerequisites
* Node.js & npm installed
* A Google Account
* A Gemini API Key ([Get one here](https://aistudio.google.com/app/apikey))

### 2. Install & Authenticate Clasp
```bash
npm install -g @google/clasp
clasp login
```

### 3. Clone & Initialize
```bash
git clone https://github.com/your-username/hr-analytics-hub.git
cd hr-analytics-hub

# Initialize a new Google Apps Script Web App
clasp create --type webapp --title "HR Analytics Hub" --rootDir ./src
```

### 4. Push Code to Google Servers
```bash
clasp push
```

---

## Configuration & Security

**Never hardcode your API keys.** Follow these steps to securely configure Gemini:

1. Run `clasp open` to open the Apps Script editor in your browser.
2. Open `Code.js`.
3. Locate the `setApiKey()` function.
4. Temporarily uncomment the line:
   ```javascript
   PropertiesService.getScriptProperties().setProperty('GEMINI_API_KEY', 'YOUR_ACTUAL_KEY_HERE');
   ```
5. Insert your Gemini API key, and click **Run** > `setApiKey` in the toolbar.
6. Delete or re-comment the line. Your key is now permanently and securely stored in Google's backend properties.

---

## Deployment

To release the application to users:
1. In the Apps Script UI, click **Deploy** > **New Deployment**.
2. Select type: **Web App**.
3. **Execute as**: `Me` (Allows the app to run under your permissions/API key).
4. **Who has access**: `Anyone` (Or restrict to your Google Workspace domain).
5. Click **Deploy**. You will receive a URL you can share with your HR team.

---

## Repository Structure

```text
hr-analytics-hub/
├── .github/                 # GitHub specific configurations (Templates, Actions)
├── docs/                    # Architectural and data flow documentation
├── src/
│   ├── Code.js              # Server-side API endpoints & App Service
│   └── Index.html           # Frontend UI, CSS, and Client-side AppController
├── appsscript.json          # Google Apps Script manifest file
└── README.md                # Documentation
```

## Contributing

Contributions, issues, and feature requests are welcome! 
Check out the [CONTRIBUTING.md](./CONTRIBUTING.md) and [Pull Request Template](.github/PULL_REQUEST_TEMPLATE.md) for standard guidelines.

## License

Distributed under the MIT License. See `LICENSE` for more information.
