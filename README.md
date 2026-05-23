# HR Ecosystem Portal

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg?style=for-the-badge)](https://opensource.org/licenses/MIT)
[![Google Apps Script](https://img.shields.io/badge/Google_Apps_Script-4285F4?style=for-the-badge&logo=google&logoColor=white)](https://developers.google.com/apps-script)
[![Gemini API](https://img.shields.io/badge/Gemini_API-10b981?style=for-the-badge&logo=google&logoColor=white)](https://ai.google.dev/)

An advanced, serverless Single-Page Application (SPA) built for Google Apps Script. This tool serves as a comprehensive corporate-grade **HR Ecosystem Portal**. It empowers HR leaders to upload CSV or Excel files, manages and cleanes data completely in-memory for maximum privacy and performance, draws dynamic and responsive custom charts, and utilizes a custom-prompted **Gemini AI chatbot** alongside automated insights, database views, and organization budget simulation features.

---

## Core Features & Architecture

The application is structured into five distinct operational views to serve as a complete HR command center:

1. **Executive Dashboard**:
   - High-level KPIs: Active Headcount, Average Compensation, Department Count, and Attrition Rate.
   - Customized horizontal department distribution charts, gender ratio doughnut layouts, and compensation curve line charts powered by an inline premium HTML5 Canvas engine.
2. **Workforce Directory**:
   - A fully searchable, filterable data grid listing employee records.
   - Interactive, programmatically bound, in-memory CRUD operations: Add new employees, edit existing salaries or departments, and delete records with real-time global dashboard updates.
3. **Pay Equity & Insights**:
   - Automated gender pay gap analytics showing base wage disparities by department.
   - Attrition warning hotspots highlighting high-turnover groups.
   - **Gemini Executive Report**: Instantly synthesizes benchmarks, salary curves, and pay gaps into a structured executive brief with tables and recommendations.
4. **Payroll Budget Simulator**:
   - Model department-wide compensation modifications via range sliders.
   - Live projected changes to overall active payroll and average salaries before permanent application.
5. **Interactive AI Analyst**:
   - A persistent sidebar assistant that securely parses aggregated session context vectors to answer queries in plain language.

---

## Technical Architecture

This application utilizes a decoupled, client-centric architecture to secure PII and bypass traditional Apps Script execution limits:

* **In-Memory ETL Pipelines**: Cleanse currency formats, match status conditions, resolve column names dynamically (e.g. "wage" or "pay" mapping to salary), and store state entirely inside the browser's sandbox. No raw employee data is stored on remote servers.
* **Programmatic Event Binding**: Replaces inline DOM attributes with modular `addEventListener` registrations to ensure compatibility and error-free execution within Google Apps Script's strict sandboxed iframe containers.
* **Self-Contained Canvas Engine**: Uses custom drawing algorithms to render vector-sharp, fluidly resized visualizations on high-DPI displays without relying on external charting script resources.

---

## Deployment & Setup

### Prerequisites
* A Google Account
* A Gemini API Key ([Get a key here](https://aistudio.google.com/app/apikey))

### Configuration
1. Open the Apps Script editor (`script.google.com`) and paste the contents of `src/Code.js` and `src/Index.html` into corresponding files.
2. Run the `setApiKey()` function once to save your Gemini API key in `ScriptProperties` under the key `GEMINI_API_KEY`.
3. Deploy the application as a **Web App**, set executing permissions to `Me` and accessibility to `Anyone`.
