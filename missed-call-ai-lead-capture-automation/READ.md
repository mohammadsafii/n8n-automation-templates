# AI SMS Lead Capture & Booking System for Home Service Businesses

An automated, memory-driven SMS agent built using **n8n**, **Twilio**, **Groq (LLaMA 3.3 70B)**, and **Google Sheets**. It detects missed calls in real time, initiates a conversation over SMS, gathers structured lead details while maintaining multi-turn context, and instantly alerts the business owner when a lead is qualified.

---

## 🌟 Key Features

- **Instant Missed Call Recovery:** Webhooks trigger an automated outreach text within seconds of a missed call.
- **Stateful Memory via Google Sheets:** Persists conversation history per phone number across multi-turn interactions.
- **LLaMA 3.3 70B Intelligence:** Powered by Groq for sub-second, highly structured JSON responses tailored for trade services.
- **Automated Lead Qualification:** Extracts `service_type`, `issue`, `urgency`, `customer_name`, and `address`.
- **Owner Instant Notification:** Dispatches direct SMS alerts with structured lead summary as soon as a lead is qualified.

---

## 🛠️ Tech Stack

- **Workflow Engine:** [n8n](https://n8n.io/)
- **SMS & Webhook Gateway:** [Twilio](https://www.twilio.com/)
- **LLM Inference:** [Groq](https://groq.com/) (`llama-3.3-70b-versatile`)
- **State & Database:** [Google Sheets API](https://developers.google.com/sheets/api)

---

## 📊 Google Sheets Setup

Create a Google Sheet with a worksheet named `Sheet1` containing the following column headers in Row 1:

| timestamp | caller_number | call_status | conversation_history |
| :--- | :--- | :--- | :--- |

1. Make sure your Google account connected in n8n has edit access to this spreadsheet.
2. Copy the **Spreadsheet ID** from the URL:
   `https://docs.google.com/spreadsheets/d/<YOUR_SPREADSHEET_ID>/edit`

---

## 🚀 Setup & Installation

### 1. Import Workflow into n8n
1. Open your n8n instance.
2. Click **Workflows** > **Add Workflow**.
3. Click the `...` menu (top right) and select **Import from File**.
4. Upload `ai-sms-lead-capture-workflow.json`.

### 2. Configure Node Parameters
After importing, update the following placeholders in the workflow nodes:

- **Google Sheets Nodes:** Select your credentials and paste `YOUR_SPREADSHEET_ID` into the `Document` parameter for:
  - `Append row in sheet`
  - `Get row(s) in sheet`
  - `Update row in sheet`
- **Groq HTTP Request Node:** Update the `Authorization` header with your actual key:
  - Header: `Authorization`
  - Value: `Bearer YOUR_GROQ_API_KEY`
- **Twilio Nodes:**
  - `Send Initial Outreach SMS`: Set `From` to your Twilio Phone Number.
  - `Send AI Response SMS`: Set `From` to your Twilio Phone Number.
  - `Alert Owner - Qualified Lead`: Set `From` to your Twilio Phone Number and `To` to the **Business Owner's Phone Number** (`+1XXXXXXXXXX`).

### 3. Twilio Webhook Configuration
1. Activate your n8n workflow to generate production Webhook URLs.
2. Copy the Production URL for `Webhook` (Missed Calls) and set it as the status callback URL in Twilio.
3. Copy the Production URL for `Webhook1` (Inbound SMS) and set it as the primary Webhook URL for incoming messages on your Twilio Phone Number.

---

## 📄 License

MIT License. Free for personal and commercial use.