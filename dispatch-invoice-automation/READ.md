# 🚚 Invoice Automation for Dispatchers (n8n Workflow)

Automate your weekly dispatch invoicing with this production-ready n8n workflow. It reads load data from Google Sheets, dynamically generates customized HTML invoices, converts them to PDF using Gotenberg, updates load statuses, and emails final invoices to drivers or companies via Gmail.

---

## 📋 Features

* **Dual Billing Modes:** Supports both **Per-Driver Invoicing** (Path A) and **Combined/Grouped Company Invoicing** (Path B).
* **Automated Schedule:** Runs automatically every week on specified days (default: Mondays and Fridays at 7:00 PM).
* **Dynamic HTML to PDF:** Renders crisp, professional PDF invoices via Gotenberg.
* **Auto-Incrementing Invoice Numbers:** Tracks and updates invoice numbers automatically in your control sheet.
* **Status Updates:** Marks processed loads as invoiced in Google Sheets to prevent duplicate billing.

---

## 🛠️ Prerequisites

Before importing the workflow, make sure you have:

1. **n8n Instance:** Running self-hosted or on n8n Cloud.
2. **Gotenberg Service:** A running instance of [Gotenberg](https://gotenberg.dev/) (used to convert HTML invoices to PDF).
3. **Google Service Account:** With access to your Google Sheet.
4. **Gmail Account:** Connected to n8n via OAuth2 or App Password.

---

## 🚀 Quick Setup Guide

### 1. Set Up Your Google Sheet
Ensure your Google Sheet includes the following tabs:
* **Business Config:** Business name, logo (Base64), address, phone, payment terms, colors, and service fee percentage.
* **Control Tab:** Company names, sheet GIDs, current invoice numbers, and driver-based flags.
* **Company Load Tabs:** One tab per company containing individual load records.

---

### 2. Configure Workflow Placeholders

After importing `workflow.json` into n8n, update the following placeholders:

#### A. Set Your Google Sheet ID
Open the **Client Config** node and update the value:
* **Key:** `sheetId`
* **Value:** Replace `YOUR_GOOGLE_SHEET_ID_HERE` with your actual Google Sheet ID (found in the sheet URL between `/d/` and `/edit`).

#### B. Update Sheet Tab IDs (GIDs)
Because Google Sheets assigns new GIDs when copying templates, verify or update the `sheetName` GIDs in these nodes:
* **Read Business Config**
* **Read Control Tab**
* **Increment Invoice Number**
* **Reread Current Invoice Number**

> *Tip:* If your tab IDs differ, switch the `sheetName` mode in those nodes from **ID** to **Name** and select the matching tab name.

#### C. Set Gotenberg PDF Server URL
Open the two Gotenberg nodes:
* `Generate PDF - Gotenberg`
* `Generate PDF - Gotenberg (Driver)`

Replace `YOUR_GOTENBERG_SERVER_URL` in the **URL** field with your actual Gotenberg host URL:
```text
http://<your-gotenberg-host>:3000/forms/chromium/convert/html