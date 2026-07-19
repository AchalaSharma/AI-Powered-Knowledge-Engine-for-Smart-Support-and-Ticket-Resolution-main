# AI-Powered Knowledge Engine for Smart Support and Ticket Resolution

An intelligent support ticketing and analytics application built with Streamlit. This system provides a unified interface for customer support agents and users to interact with an AI ticket assistant (leveraging RAG and a LangChain agent) and monitor real-time ticket metrics via an interactive analytical dashboard.

---

## 🚀 Key Features

*   **💬 AI Ticket Assistant (Chatbot)**
    *   **Conversational Agent:** Powered by LangChain and Groq (`llama-3.1-8b-instant`) with conversation memory to handle multi-turn queries.
    *   **Retrieval-Augmented Generation (RAG):** Extracts answers from internal support manuals (`Train.pdf`) using HuggingFace embeddings (`all-MiniLM-L6-v2`) and an in-memory FAISS vector database powered by Google Gemini (`gemini-1.5-flash`).
    *   **Live Database Tools:** Ability to automatically look up ticket statuses (`GoogleSheetsLookup`) or log new support tickets (`SaveTicket`) to the database.
    *   **External Web Search:** Uses Tavily Search when questions fall outside the local knowledge base.

*   **📊 Analytic Dashboard**
    *   **Key Metrics (KPIs):** Real-time monitoring of Total Tickets, Resolved, Unresolved, and Resolution Percentage.
    *   **Interactive Visualizations:** Responsive Plotly charts representing ticket statuses, category distribution, and unresolved query statistics.
    *   **Article Insights:** Displays frequency trackers showing top referenced customer issues.
    *   **System Alerts:** High-level warning banners highlighting low-coverage categories.

*   **🚨 Automated Alerts**
    *   Send custom email alerts to support teams or customers directly from the dashboard sidebar using secure Gmail SMTP.

*   **🏷️ Semantic Categorization**
    *   Uses a dedicated classification pipeline via Groq to automatically route and tag incoming tickets into one of 20 categories (e.g., `refund`, `cancellation`, `technical_issue`, etc.).

---

## 🛠️ Technology Stack

*   **UI Framework:** Streamlit
*   **Agent & Orchestration:** LangChain
*   **LLM Services:** Groq (Llama 3.1) & Google Generative AI (Gemini 1.5 Flash)
*   **Vector Embeddings:** HuggingFace `sentence-transformers/all-MiniLM-L6-v2`
*   **Vector Database:** FAISS (in-memory)
*   **Database Storage:** Google Sheets API (`gspread`)
*   **Data Analysis & Plots:** Pandas, Plotly Express
*   **Email Engine:** SMTP & EmailMessage

---

## 📂 Project Structure

*   `finalmain.py` - Main Streamlit entry point coordinating routing and sidebar navigation.
*   `chatbot.py` - Renders the user-facing chat window and connects it to the backend agent.
*   `dashboard2.py` - Computes performance metrics and renders Plotly visualization widgets.
*   `ai3.py` - Configures the LangChain agent, custom tools, memory, and LLM orchestration.
*   `raghugging.py` - Embeds, indexes, and queries the local knowledge PDF using FAISS & Gemini.
*   `categorization.py` - Performs semantic classification on ticket texts using Groq.
*   `alert.py` - Implements the Gmail alert email-sending system.
*   `Train.pdf` - PDF manual containing the primary support guidelines/knowledge base.
*   `MyDatabaseSheet.xlsx` - Reference structure/format of the ticket registry spreadsheet.
*   `doc.md` - Technical setup notes for Google Cloud Console and library dependencies.

---

## ⚙️ Setup & Installation

### 1. Clone & Initialize Environment
Set up a python virtual environment:
```bash
python -m venv myenv
myenv\Scripts\activate
```

### 2. Install Dependencies
Install all required packages from `requirements.txt`:
```bash
pip install -r requirements.txt
```
*(If dependencies are missing, run the following command to update core libraries)*:
```bash
pip install -U langchain langchain-community langchain-google-genai langchain-text-splitters faiss-cpu pypdf python-dotenv langchain-groq gspread oauth2client pandas plotly streamlit
```

### 3. Setup Google Sheets API Credentials
1. Go to the [Google Cloud Console](https://console.cloud.google.com/).
2. Enable the **Google Sheets API** and **Google Drive API** for your project.
3. Generate service account credentials, download the JSON key file, and rename it to `credentials.json`.
4. Move `credentials.json` into the root directory of this project.
5. Create a Google Sheet named `MyDatabaseSheet` and share it with the service account email (with Editor permissions).

### 4. Configure Environment Variables
Create a `.env` file in the root folder and add the following keys:
```env
GROQ_API_KEY="your_groq_api_key"
GOOGLE_API_KEY="your_google_gemini_api_key"
TAVILY_API_KEY="your_tavily_search_api_key"
```

---

## 🏃 Run the Application

Launch the Streamlit dashboard by executing:
```bash
streamlit run finalmain.py
```
This will open up a local web server (usually at `http://localhost:8501`) where you can interact with the chatbot and analytic metrics.
