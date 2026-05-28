# ajentik-ai-tugas-5
repository for agentic ai course

Ini adalah tugas 5 mata kuliah agnetic ai, batch 1, di Kelas Otomesyen.
# Customer Service AI Agent - Toko Sejahtera (n8n Workflow)

This repository contains the workflow automation file built using **n8n** to deploy an Artificial Intelligence (AI) Customer Service Agent. The system seamlessly integrates a **Telegram Bot** as the customer interface and a **Google Sheet** as a real-time product inventory and price database.

This workflow is designed to intelligently detect customer intents, categorize their inquiries, and either generate automated contextual responses or route them through dedicated downstream operational paths.

---

## Workflow Component Overview

Based on the architecture of the `tugas modul 5.json` file, the core components involved are:

1. **Telegram Trigger**: Monitors and captures every incoming text message from customers via the Telegram bot chat.
2. **AI Agent (LangChain)**: The core brain of the system that utilizes a Large Language Model (LLM) to analyze inputs, determine categories, and invoke tools dynamically based on system instructions.
3. **Language Models (LLMs)**: Combines `models/gemini-2.0-flash` and an OpenRouter-hosted model (`z-ai/glm-4.5-air:free`).
4. **Simple Memory (Window Buffer)**: Persists conversation history mapped to the customer's `chat.id`, allowing the AI agent to maintain context throughout multi-turn dialogues.
5. **get prices tool (Google Sheets Tool)**: A specialized capability granted to the AI Agent to pull live inventory stock and price listings directly from the official Google Spreadsheet.
6. **Structured Output Parser**: Enforces the AI Agent to strictly respond in a raw JSON schema containing only `category` and `reply` keys.
7. **Switch Node**: Acts as a logical router that directs the workflow to different branches based on the evaluated payload `category`.
8. **Set Nodes (Edit Fields)**: Standardizes static responses for operational queries (*promo*, *bantuan*, and *komplain*).
9. **Send a Text Message (Telegram Node)**: Dispatches the final compiled answer back to the customer on Telegram using Markdown formatting.

---

## AI Categorization & Routing Rules

The AI Agent is hardcoded with a comprehensive system prompt to classify user inquiries into exactly one of five distinct categories:

| Category | Trigger Condition | Action / Output Behavior |
| :--- | :--- | :--- |
| **`harga`** *(Price)* | Inquiries regarding product pricing, stock availability, or price lists. | **Must** execute the `get prices tool` to fetch real-time spreadsheet data before formulating an accurate, dynamic JSON response. |
| **`promo`** *(Promotion)* | Inquiries about active discounts, member benefits, or sales. | Keeps the AI `reply` field blank; downstream routing falls back onto a static node detailing the **5% member discount**. |
| **`bantuan`** *(Support)* | Inquiries about store location, operating address, or official contact numbers. | Keeps the AI `reply` field blank; downstream routing falls back onto a static node providing the **Bali branch address & WhatsApp contact**. |
| **`komplain`** *(Complaint)* | Reports regarding damaged items, missing orders, or customer dissatisfaction. | Keeps the AI `reply` field blank; downstream routing falls back onto a static node prompting a structured **transaction issue form**. |
| **`lainnya`** *(Other)* | Small talk, generic greetings (*hello*, *good morning*), or general compliments. | The AI directly generates a polite, contextual greeting response in Indonesian. |

---

## How to Use & Deploy

### 1. Prerequisites
* A running instance of **n8n** (Cloud or Self-hosted).
* A **Telegram Bot Token** (Obtainable via [@BotFather](https://t.me/BotFather)).
* **Google Cloud Console Credentials** (OAuth2 configured with read permissions for the Google Sheets API).
* API Keys for **Google Gemini** and/or **OpenRouter**.

### 2. Import the Workflow to n8n
1. Download the `tugas modul 5.json` file from this repository.
2. Open your n8n workspace dashboard.
3. Click the top-right menu icon (three dots/lines) -> select **Import from File**.
4. Choose and upload `tugas modul 5.json`.

### 3. Setup Your Credentials
Once imported, connect your localized API accounts to the nodes displaying connection alerts:
* **Telegram Trigger** & **Send a text message**: Link your active Telegram Bot Account credentials.
* **Google Gemini Chat Model**: Input your Google Gemini API Key.
* **OpenRouter Chat Model**: Input your OpenRouter API Key.
* **get prices tool**: Authorize your Google OAuth2 account that has access to read the target spreadsheet.

### 4. Activate the Workflow
Test the stream live by clicking **Listen for test event**, or simply toggle the **Publish** switch in the top-right corner to **On** to deploy the bot 24/7.

---

## QA Testing Matrix (Test Cases)

You can validate the deterministic routing and AI outputs of your deployed Telegram Bot using the quality assurance matrix below:

| ID | User Input (Telegram Chat) | Expected Category | Expected Output Response (System Criteria) | Status |
| :---: | :--- | :---: | :--- | :---: |
| **TC-01** | "Halo, selamat pagi Toko Sejahtera!" | `lainnya` | Generates an automated polite greeting. (e.g., "Selamat pagi! Ada yang bisa kami bantu hari ini?") | ⬜ Pass/Fail |
| **TC-02** | "Berapa harga sabun cuci piring sekarang? Apakah ada stoknya?" | `harga` | Fetches real-time price variables from Google Sheets and answers with accurate numbers. | ⬜ Pass/Fail |
| **TC-03** | "Apakah ada diskon atau promo minggu ini?" | `promo` | "Setiap anggota (member) Toko Sejahtera berhak mendapatkan potongan harga sebesar 5% untuk setiap transaksi dengan nilai pembelian di atas Rp 100.000." | ⬜ Pass/Fail |
| **TC-04** | "Saya mau tahu alamat tokonya di mana ya? Sama nomor WA nya." | `bantuan` | "Untuk bantuan, silahkan hubungi Customer service kami via WA di 081934384508, atau datang langsung ke Toko Sejahtera di Jalan Ir. Soekarno No.5, Tabanan, Bali" | ⬜ Pass/Fail |
| **TC-05** | "Barang yang saya beli kemarin pecah, gimana nih tanggung jawabnya?" | `komplain` | "Mohon maaf atas ketidaknyamanan yang terjadi. Mohon sertakan data berikut untuk kami proses. 1. Tanggal transaksi... [etc]" | ⬜ Pass/Fail |
| **TC-06** | "Kalian luar biasa, pelayanannya cepat banget!" | `lainnya` | Generates a custom conversational thank-you response acknowledging the user's praise. | ⬜ Pass/Fail |




