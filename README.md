# SUPPORT SPHERE AI ⚡

**SUPPORT SPHERE AI** is a state-of-the-art, full-stack AI-Powered Intelligent Customer Support Web Application. It combines Natural Language Processing (NLP), Deep Learning (PyTorch BiLSTM), Retrieval-Augmented Generation (RAG) with ChromaDB and Sentence Transformers, and authorized SQLite transactional records to deliver instantaneous, accurate, and grounded customer service.

---

## 🌟 Key Features

1. **Futuristic Cyber-Themed UI**:
   - Modern, responsive dark theme with purple and cyan neon accents, glassmorphic cards, smooth animations, and interactive prompts.
   - Real-time messaging, typing indicators, auto-scroll, and Speech-to-Text voice recognition (Web Speech API).

2. **Deep Learning Intent Classification (PyTorch BiLSTM)**:
   - Custom-trained 2-layer Bidirectional LSTM neural network trained on customer support utterances.
   - Automatically classifies inquiries into: `check_order`, `track_ticket`, `create_ticket`, `refund_policy`, `shipping_faq`, `warranty_inquiry`, `human_escalation`, `cancel_order`, `greeting`, `goodbye`.
   - Confidence thresholding (0.55) with intelligent fallback to semantic knowledge search.

3. **High-Precision RAG (ChromaDB + Sentence Transformers + PyMuPDF)**:
   - Indexes official company policies, FAQs, and support manuals from multi-page PDFs using PyMuPDF (`pymupdf`).
   - Semantic vector search with dense embeddings via `all-MiniLM-L6-v2` stored in persistent ChromaDB collections.
   - Zero-hallucination grounded responses with verified source citations: `[Source: <filename>, Page <X>]`.
   - Fallback protection: Never fabricates company policies or refund approvals when information is absent.

4. **Authorized SQL Database Lookups**:
   - Live querying of SQLite database for customer order status (`#ORD-XXXXX`), tracking numbers, carriers, and delivery estimates.
   - Customer support ticket status lookup (`#TCK-XXXXX`), notes, and assigned agents.
   - Order cancellation verification (orders in "Processing" can be cancelled; already "Shipped" or "Delivered" orders cannot).

5. **Customer Sentiment Analysis & Human Escalation**:
   - Detects customer emotions (Frustrated, Dissatisfied, Neutral, Satisfied).
   - Frustrated queries or explicit requests to speak with a human agent automatically trigger high-priority human escalation and instant ticket creation.

6. **Operations & Admin Management Dashboard (`/admin`)**:
   - **Ticket Queue**: View, filter, and update ticket statuses (`Open`, `In Progress`, `Resolved`, `Closed`), assign agents, and log resolution notes.
   - **Knowledge Base Hub**: Drag-and-drop PDF/TXT/MD uploader, chunk count inspection, one-click document deletion, and re-indexing.
   - **Order Records**: Inspect authorized customer orders, shipping addresses, and tracking details.
   - **FAQ Manager**: Add, manage, and toggle instant FAQ records.
   - **Real-Time Analytics**: Visual KPI cards, satisfaction metrics, customer sentiment distribution bars, and live conversation logs.

---

## 🏗️ Architecture & AI Workflow

```
[ Customer Query ]
        │
        ▼
[ NLP Preprocessing & Entity Extraction ] (Order IDs, Ticket IDs, Email, Phone)
        │
        ▼
[ Sentiment Analyzer ] (Detects Frustration, Urgency, Escalation Triggers)
        │
        ▼
[ Deep Learning Intent Classifier (PyTorch BiLSTM) ]
        │
   ┌────┴───────────────────────────┬───────────────────────────┐
   ▼                                ▼                           ▼
[ Order Status / Cancel ]    [ Ticket Status / Create ]    [ Policy & Technical Inquiries ]
   │ (SQL Query)                    │ (SQL Query)               │ (ChromaDB Vector Search)
   ▼                                ▼                           ▼
[ SQLite Orders Table ]      [ SQLite Tickets Table ]      [ Sentence Transformers ]
   │                                │                           │
   └────────────────────────────────┼───────────────────────────┘
                                    │
                                    ▼
                     [ Grounded Answer Synthesizer ]
             (Strict citation formatting: [Source, Page])
                                    │
                                    ▼
                        [ Interactive UI Response ]
```

---

## 🛠️ Technology Stack

| Layer | Technologies |
|---|---|
| **Frontend** | HTML5, CSS3 (Custom Cyber-Dark Theme, Glassmorphism), Vanilla JavaScript |
| **Backend** | Python 3.14 / 3.12, Flask, Flask-CORS, Werkzeug |
| **Database** | SQLite, SQLAlchemy ORM |
| **NLP** | Tokenization, Regular Expressions, Entity Extraction, Sentiment Scoring |
| **Deep Learning** | PyTorch (`torch.nn.LSTM`, Bidirectional, Dropout, Linear, AdamW) |
| **Embeddings** | Sentence Transformers (`all-MiniLM-L6-v2`) |
| **Vector DB** | ChromaDB (`chromadb.PersistentClient`) |
| **PDF Processing**| PyMuPDF (`pymupdf` / `fitz`) |
| **Response Generation** | Google Gemini API / OpenAI API (Optional) + Grounded Local Fallback Synthesizer |

---

## 📂 Project Structure

```
SUPPORT SPHERE AI/
├── app.py                     # Flask application entry point and REST API routes
├── config.py                  # Environment and application configuration
├── requirements.txt           # Python dependencies
├── test_app.py                # Automated integration test suite
├── .env.example               # Example environment variables
├── .env                       # Active environment configuration
├── README.md                  # Comprehensive documentation
│
├── models/                    # SQLAlchemy database models
│   ├── __init__.py
│   └── database.py            # User, Order, Ticket, Conversation, KnowledgeDocument, FAQ, Feedback
│
├── nlp/                       # NLP preprocessing & Sentiment
│   ├── __init__.py
│   ├── preprocessor.py        # Tokenizer, regex entity extractor (ORD-*, TCK-*, email)
│   └── sentiment.py           # Frustration scoring and human escalation detector
│
├── deep_learning/             # PyTorch Intent Classification
│   ├── __init__.py
│   ├── dataset.py             # Intent training dataset and vocabulary mappings
│   ├── model.py               # BiLSTM neural network architecture
│   └── classifier.py          # PyTorch model trainer, loader, and inference service
│
├── rag/                       # RAG Pipeline & Vector Database
│   ├── __init__.py
│   ├── pdf_loader.py          # PyMuPDF text extractor and overlapping chunker
│   ├── vector_store.py        # ChromaDB manager and Sentence Transformers embeddings
│   └── generator.py           # Grounded response generator with citations
│
├── services/                  # Business Logic Services
│   ├── __init__.py
│   ├── intent_service.py      # Master workflow coordinator
│   ├── order_service.py       # SQL lookup and order cancellation
│   ├── ticket_service.py      # Ticket generation, update, and lookup
│   ├── rag_service.py         # Knowledge document indexing and querying
│   └── seed_data.py           # Seeds database and generates sample policy PDFs
│
├── static/                    # Frontend Assets
│   ├── css/
│   │   ├── style.css          # Chatbot interface styling (futuristic dark theme)
│   │   └── admin.css          # Admin operations dashboard styling
│   └── js/
│       ├── chat.js            # Real-time chat engine, speech recognition, modals
│       └── admin.js           # Admin panel data loaders, document upload, status updates
│
├── templates/                 # Jinja2 HTML Templates
│   ├── index.html             # Customer Support Chatbot UI
│   └── admin.html             # Operations & Knowledge Management Dashboard
│
└── data/                      # Persistent storage (auto-generated)
    ├── support_sphere.db      # SQLite database
    ├── chroma_db/             # ChromaDB vector store directory
    ├── sample_docs/           # Generated sample support policy PDFs and MD files
    ├── trained_models/        # Saved PyTorch intent model weights and vocab.json
    └── uploads/               # User-uploaded support documents
```

---

## 🚀 Quickstart & Installation

### 1. Clone or Open the Workspace
```bash
cd "c:\Users\Gopika\OneDrive\Desktop\New folder\SUPPORT SPHERE AI"
```

### 2. Install Dependencies
```bash
python -m pip install -r requirements.txt
```

### 3. Environment Configuration
The application comes preconfigured with default settings in `.env`.
To customize or add an external LLM API key (optional):
```env
# .env
SECRET_KEY=support-sphere-secret-key-2026

# Optional: Add Gemini or OpenAI API Key for enhanced LLM generation
LLM_PROVIDER=gemini
GEMINI_API_KEY=your_gemini_api_key_here
OPENAI_API_KEY=your_openai_api_key_here
```
> **Note:** If no API key is set, Support Sphere AI automatically runs its built-in high-precision Local Grounded Synthesizer without requiring any external cloud credentials!

### 4. Run the Application
You can start the server with any of the following:

**Using Python directly:**
```bash
python app.py
```

**Using the 1-click Windows Batch launcher:**
```cmd
run.bat
```

**Using PowerShell:**
```powershell
.\run.ps1
```

Once running, open your web browser at:
- **Chatbot Interface**: [http://127.0.0.1:5000/](http://127.0.0.1:5000/)
- **Admin Dashboard**: [http://127.0.0.1:5000/admin](http://127.0.0.1:5000/admin)

### 5. Run Automated Tests
```bash
python test_app.py
```

---

## 🧪 Sample Test Data & Queries

### Sample Orders (in SQLite):
- `ORD-84920` — CyberSound Pro Wireless Headphones ($199.99, **Shipped**)
- `ORD-73194` — Nova Ultra 4K Smart Cinema Projector ($649.00, **Processing**)
- `ORD-51928` — Pulse Mechanical Keyboard ($129.50, **Delivered**)
- `ORD-92011` — Quantum Gaming Mouse ($79.99, **Out for Delivery**)

### Sample Support Tickets (in SQLite):
- `TCK-30219` — Display flicker on HDMI 2.1 (Status: **In Progress**)
- `TCK-84721` — VAT invoice request (Status: **Resolved**)
- `TCK-19482` — Change delivery instructions (Status: **Open**)

### Example Queries to Try:
| Query | Engine Triggered | Result |
|---|---|---|
| *"Track order ORD-84920"* | SQL Order Lookup | Returns CyberSound Headphones, Shipped status, tracking code, and delivery date. |
| *"What is your return policy?"* | ChromaDB RAG | Returns 30-day return window, condition criteria, and cites `return_and_refund_policy.pdf`. |
| *"Is water damage covered under warranty?"* | ChromaDB RAG | Explains that liquid submersion is excluded from standard warranty and cites `warranty_and_repairs_guide.pdf`. |
| *"What is the status of ticket TCK-30219?"* | SQL Ticket Lookup | Returns ticket status (`In Progress`), technician David, and notes. |
| *"I want to cancel order ORD-73194"* | SQL Transaction | Cancels order (since it is still Processing) and initiates refund. |
| *"This is unacceptable! I want a real human!"* | Sentiment + Escalation | Detects `Frustrated` emotion, prioritizes human escalation, and displays ticket modal. |

---

## 🛡️ Security & Reliability Best Practices
- **No Hardcoded Secrets**: All keys, directories, and database paths are loaded from environment variables.
- **SQL Injection Prevention**: Parameterized queries via SQLAlchemy ORM.
- **Strict File Upload Validation**: Allowed extensions (`.pdf`, `.txt`, `.md`), file size limit (16MB), and filename sanitization via `secure_filename`.
- **Zero Hallucination Policy**: RAG queries cite verified document references. If information is not found in knowledge base, system gracefully triggers fallback and offers human escalation.

---

## 📜 License
Developed with ❤️ by the Support Sphere AI Team.

#   S U P P O R T - S P H E R E - A I  
 