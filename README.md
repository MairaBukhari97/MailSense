# MailSense – Smart Email Engine

**An AI-powered Gmail assistant for intelligent email management, classification, summarization, and inbox interaction.**

MailSense is a Final Year Project developed to simplify email management using Artificial Intelligence, Natural Language Processing, and Retrieval-Augmented Generation (RAG).

The system integrates with Gmail to retrieve and organize emails, automatically classify messages, generate AI-powered summaries and replies, and allow users to ask questions about their inbox through a conversational interface.

Rather than manually searching through hundreds of emails, users can interact with their inbox and retrieve relevant information using natural language.

## Key Features

### 1. Gmail Integration & Authentication

* Google OAuth 2.0 authentication.
* Secure storage of authentication tokens and user information in PostgreSQL.
* JWT-based session management with a 7-day expiry.
* Gmail API integration for retrieving, sending, and managing emails.

### 2. Email Management

* Initial synchronization of up to 500 inbox emails.
* Automatic background synchronization every 60 seconds.
* Incremental synchronization to retrieve newly received emails.
* Email listing and display, ordered by date.
* Send, delete, and manage read/unread email status.

### 3. AI-Powered Email Classification

MailSense uses a locally hosted BERT-based classification model to categorize emails into five categories:

| Category             | Description                                      |
| -------------------- | ------------------------------------------------ |
| Account Alerts       | Account notifications and security alerts        |
| Career & Personal    | Career opportunities and personal communications |
| Finance & Legal      | Financial and legal-related emails               |
| Marketing & Outreach | Promotional emails and marketing communications  |
| Work & Operations    | Work-related and operational emails              |

### 4. AI Email Assistance

Using Groq LLM APIs, MailSense provides several email assistance features:

* **Email Summarization:** Generates concise, abstractive, bullet-point summaries of email content.
* **Email Caption Generation:** Creates descriptive 5–10 word titles for emails.
* **AI Reply Generation:** Drafts context-aware replies that users can review and edit before sending.
* **New Email Generation:** Generates complete email drafts from user-provided descriptions.
* **Sentiment Analysis:** Analyzes email tone, urgency, and polarity.
* **Calendar Event Extraction:** Detects date and time references in emails and supports creating events through the Google Calendar API.

### 5. Retrieval-Augmented Generation (RAG)

MailSense includes a local email retrieval system that allows users to ask questions about their inbox.

The RAG pipeline:

1. Retrieves inbox emails from PostgreSQL.
2. Divides email content into 800-character chunks.
3. Generates embeddings using Sentence Transformers (`all-MiniLM-L6-v2`).
4. Stores embeddings and email content in ChromaDB using separate collections per user.
5. Retrieves relevant information through hybrid search.
6. Uses Groq LLMs to generate answers based on the retrieved email context.

Additional retrieval capabilities include:

* **Hybrid Search:** Combines semantic search, keyword matching, and urgency scoring.
* **Conversation-Based Q&A:** Allows users to ask natural-language questions about their inbox.
* **Source References:** Provides references to relevant emails used to generate answers.
* **Conversation History:** Retains up to 20 conversation turns per session.
* **Follow-up Question Rewriting:** Converts context-dependent follow-up questions into self-contained queries before retrieval.
* **Sender-Aware Filtering:** Identifies sender names in natural-language queries and filters results to relevant messages.

## Technology Stack

| Component                      | Technologies                                        |
| ------------------------------ | --------------------------------------------------- |
| Frontend                       | React.js, JavaScript, CSS, Vite                     |
| Frontend Development Tool      | Google Antigravity                                  |
| Backend                        | Python, FastAPI                                     |
| Database & ORM                 | PostgreSQL, SQLAlchemy                              |
| Vector Database                | ChromaDB                                            |
| Machine Learning               | DistilBERT                                          |
| Text Embeddings                | Sentence Transformers (`all-MiniLM-L6-v2`)          |
| AI / LLM Integration           | Groq API                                            |
| Retrieval System               | Retrieval-Augmented Generation (RAG), Hybrid Search |
| Authentication & Authorization | Google OAuth 2.0, JWT                               |
| Email Integration              | Gmail API                                           |
| Calendar Integration           | Google Calendar API                                 |

## System Workflow

1. **Authentication:** Users connect their Gmail accounts through Google OAuth 2.0.
2. **Email Synchronization:** MailSense retrieves inbox emails through the Gmail API and stores them in PostgreSQL.
3. **Email Classification:** The locally hosted classification model assigns each email to one of five categories.
4. **Email Processing:** AI features provide summaries, captions, sentiment analysis, and reply generation.
5. **RAG Indexing:** Email content is divided into chunks, converted into embeddings, and stored in ChromaDB.
6. **Information Retrieval:** Hybrid search retrieves relevant email content based on user queries.
7. **AI Response Generation:** The retrieved information is passed to a Groq LLM to generate a contextual answer with source references.

## Project Structure

```text
MailSense/
│
├── backend/
│   ├── RAG/
│   ├── db/
│   └── ...
│
├── frontend4/
│   ├── src/
│   ├── public/
│   ├── package.json
│   └── vite.config.js
│
├── models/
│   └── MAILSENSE_FINAL_MODEL/
│
├── main.py
├── requirements.txt
└── README.md
```

*This is a simplified overview of the project structure.*

## Installation & Setup

### Prerequisites

* Python 3.11
* Node.js and npm
* PostgreSQL
* Google Cloud project with Gmail API enabled
* Groq API key

### 1. Clone the Repository

```bash
git clone https://github.com/MairaBukhari97/MailSense.git
cd MailSense
```

### 2. Create a Virtual Environment

```bash
python -m venv venv
```

Activate it on Windows:

```powershell
.\venv\Scripts\Activate.ps1
```

### 3. Install Backend Dependencies

```bash
pip install -r requirements.txt
```

### 4. Configure Environment Variables

Create a `.env` file in the project root and configure the required credentials.

The configuration includes:

* `DATABASE_URL`
* `CLIENT_ID`
* `CLIENT_SECRET`
* `GROQ_API_KEY`

Use your own credentials and keep this file private.

**Important:** Never upload API keys, OAuth credentials, database passwords, or other sensitive information to GitHub.

### 5. Run the Backend

From the project root:

```bash
python -m uvicorn main:app --reload --host 127.0.0.1 --port 8000
```

Backend address:

```text
http://127.0.0.1:8000
```

### 6. Run the Frontend

Open a separate terminal:

```bash
cd frontend4
```

Install dependencies:

```bash
npm install
```

Start the development server:

```bash
npm run dev
```

Open the local address displayed in the terminal.

## Security

* Google OAuth 2.0 handles Gmail account authorization.
* JWT tokens protect authenticated API endpoints.
* Sensitive credentials are maintained through environment variables.
* Gmail access is authorized through Google's consent flow.

---

