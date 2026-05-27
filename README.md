# ajentik-ai-tugas-5
repository for agentic ai course

Ini adalah tugas 5 mata kuliah agnetic ai, batch 1, di Kelas Otomesyen.
# Bangli Meubel Chatbot Automation via Telegram and n8n

This repository contains the production-ready n8n workflow JSON file for the **Bangli Meubel** Telegram AI Assistant chatbot. Built using n8n's Advanced AI components, the workflow routes user queries based on dynamic intent recognition and leverages distinct multi-agent configurations to deliver contextual answers regarding furniture pricing, promotional tracking, customer support registration, and complaint ticket filing.

## Project Overview

The Bangli Meubel Chatbot is designed to automate customer interactions for a premium furniture seller based in Bangli, Bali. The workflow acts as an intelligent router and handling layer:

* **Telegram Integration:** Listens to incoming customer messages via Telegram Webhooks.
* **Intent Router (Switch):** Filters requests conditionally based on text matching (`harga`, `promo`, `bantuan`, `komplain`).
* **Multi-Agent Architecture:** Delegates execution paths to specialized LangChain AI Agents utilizing highly efficient LLM backends (via OpenRouter).
* **Fallback General Agent:** Includes a memory-buffered master agent equipped with a calculation engine to handle open-ended design inquiries or general assistant conversations.

---

## System Architecture & Prompt Design

The system relies on structured system prompts instructing agents to process data constraints in English while outputting responses exclusively in **Bahasa Indonesia**.

1.  **Price Agent (`AI Agent1`):** Houses static data blocks containing updated inventory pricing for products categorized under *Springbed* (A–D), *Sofa* (A–D), and *Meja* (A–B).
2.  **Promo Agent (`AI Agent2`):** Directs absolute values of specific active model configurations (e.g., 10% off for high-end Springbeds, 5% off for specific Sofas).
3.  **Customer Support Agent (`AI Agent3`):** Forces slot-filling behavior. It aggressively tracks down 3 parameters before submitting user data: *Address*, *Phone Number*, and *Assistance Scope*.
4.  **Complaint Handling Agent (`AI Agent4`):** Operates under high empathy requirements. Mandates a 4-field verification validation sequence (*Phone*, *Address*, *Receipt Reference Number*, *Core Defect/Issue Description*).

---

## Technical Prerequisites

Before deploying the configuration, ensure you have the following services active:

* **n8n Instance** (n8n v1+ supporting Advanced AI nodes)
* **Telegram Bot Token** (Generated through [@BotFather](https://t.me/BotFather))
* **OpenRouter API Key** (Configured with credit allocations for model execution endpoint routing)

---

## Installation & Setup

1.  **Clone the Repository:**
    ```bash
    git clone (https://github.com/holybakerbyte/ajentik-ai-tugas-5/)
    cd bangli-meubel-n8n-bot
    ```
2.  **Import to n8n:**
    * Open your n8n workspace browser UI.
    * Click on **Workflows** -> **Import from File** (`Add Workflow` dropdown).
    * Select the file `tugas modul 5.json` from this repository.
3.  **Link Credentials:**
    * Open the **Telegram Trigger** and **Send a text message** nodes; create or link your `Telegram API` credentials.
    * Open any **OpenRouter Chat Model** node; configure your `OpenRouter API` credentials.
4.  **Activate Webhook:**
    * Save the workflow and toggle the active state status in the upper right corner to **Active**.

---

## User Manual (Cara Pakai)

Once activated, customers interact with the bot seamlessly through the Telegram Messenger UI application:

1.  **Initiating Conversation:** Search for your configured Bot username on Telegram and press **/start** or type any greeting text.
2.  **Checking Catalog & Prices:** Send text queries mentioning the keyword `harga` along with your desired furniture category (e.g., *"Berapa harga sofa B?"*).
3.  **Checking Active Deals:** Inquire using the keyword `promo` (e.g., *"Apakah ada promo untuk springbed?"*).
4.  **Requesting Human Agent Assistance:** Type queries targeting `bantuan` (e.g., *"Saya perlu bantuan"*). The bot will engage in conversational loop constraints until you submit your complete contact details.
5.  **Submitting Warranty or Post-Purchase Complaints:** Use the keyword `komplain` (e.g., *"Saya mau komplain barang rusak"*). Follow the conversational step questions to supply the transaction receipt marker.

---

## Test Cases (Skenario Uji Coba)

Run these validation pathways directly inside your Telegram Bot client to test systemic edge compliance and agent accuracy:

### Test Case 1: Pricing Catalog Check
* **User Input:** *"Halo, saya mau cek harga Springbed C dan Meja B"*
* **Expected Behavior:** The `Switch` node routes to `AI Agent1`.
* **Expected Bot Response:** The AI must explicitly return **Rp 9.000.000** for Springbed C and **Rp 3.000.000** for Meja B in clear, polite Bahasa Indonesia.

### Test Case 2: Out of Stock / Unlisted Item Request
* **User Input:** *"Berapa harga lemari pakaian premium?"*
* **Expected Behavior:** The `Switch` node routes to `AI Agent1`.
* **Expected Bot Response:** The AI must politely declare that wardrobes/lemari are not present in the current knowledge base catalogue and redirect the user to consult custom order procedures.

### Test Case 3: Promotional Discount Match
* **User Input:** *"Apakah ada promo potongan harga hari ini?"*
* **Expected Behavior:** The `Switch` node routes to `AI Agent2`.
* **Expected Bot Response:** The AI responds enthusiastically stating that there is a **10% discount** for Springbed C & D, a **5% discount** for Sofa C & D, and explicitly clarifies that there are no current promotional campaigns for tables ("Meja").

### Test Case 4: Ticket Slot-Filling Loop (Customer Support)
* **User Input:** *"Saya butuh bantuan pasang furniture"*
* **Expected Behavior:** The `Switch` node routes to `AI Agent3`.
* **Expected Bot Response:** The bot thanks the user, but notes that parameters are missing. It requests the user's complete **Address** and **Phone Number** to log the assembly request.
* **Follow-up Input:** *"Jalan Merdeka No. 10, HP 0812345678"*
* **Expected Final Response:** The agent detects all requirements have been met, acknowledges data capture completion, and confirms an interior specialist will reach out soon.

### Test Case 5: Empathetic Complaint Processing
* **User Input:** *"Barang saya cacat, saya mau komplain!"*
* **Expected Behavior:** The `Switch` node routes to `AI Agent4`.
* **Expected Bot Response:** The AI delivers an immediate empathetic apology statement and requests 4 concrete data validations: *Phone Number, Complete Address, Purchase Receipt Number,* and *Complaint Details*.

### Test Case 6: Fallback General Knowledge Routing
* **User Input:** *"Bangli Meubel ini lokasinya di daerah mana ya? Dan buka jam berapa?"*
* **Expected Behavior:** The `Switch` node triggers the fallback routing path and pushes data down to the master **AI Agent**.
* **Expected Bot Response:** The bot accesses the foundational business knowledge profile matrix to answer that the storefront resides at **Jl. Merdeka No. 46, Kawan, Bangli, Bali**, and operates **Monday through Friday from 09:00 to 17:00 WITA**.

---

## License
Distributed under the MIT License. See `LICENSE` for more information.
