# Agentic AI Job Application Pipeline

An intelligent multi-agent system that automates the end-to-end job application process — from parsing your CV to generating tailored cover letters and saving everything to a Google Sheet.

Built with **Groq (LLaMA 3.3 70B)**, **Tavily Search**, and **Sentence Transformers** as a Google Colab notebook.

---

## How It Works

The pipeline chains 5 autonomous agents together:

```
CV (PDF) → Agent 1 → Agent 2 → Agent 3 → Agent 4 → Agent 5
           Parse CV   Search    Rank Jobs  Cover     Save to
                      Jobs      by Match   Letter    Sheet
```

### Agent 1 — CV Parser
- Accepts a PDF CV upload
- Extracts raw text using **PyMuPDF**
- Sends to **Groq (LLaMA 3.3-70b-versatile)** to structure it as JSON
- Outputs: `name`, `email`, `location`, `skills`, `current_title`, `desired_role`, `experience_years`

### Agent 2 — Job Search
- Takes job title + location from CV or user input
- Queries **Tavily Search API** for real job postings
- Filters out aggregator sites (Indeed, LinkedIn, Glassdoor, Totaljobs)
- Returns direct employer job pages

### Agent 3 — Job Ranking
- Encodes the CV profile and each job listing using **`all-MiniLM-L6-v2`** (Sentence Transformers)
- Scores each job via **cosine similarity** against the CV
- Returns top-K best-matched jobs

### Agent 4 — Cover Letter Generator
- Takes CV data + top ranked job
- Prompts **Groq** to generate a professional, one-page cover letter
- Tone: professional, no emojis, personalised per role

### Agent 5 — Google Sheet Coordinator
- Authenticates via Google Colab auth
- Saves ranked jobs + generated cover letters to a Google Sheet
- Columns: Role, Company, Location, Apply Link, Email, Cover Letter

---

## Setup

### 1. Clone the repo
```bash
git clone https://github.com/YOUR_USERNAME/agentic-job-pipeline.git
```

### 2. Open in Google Colab
Upload `Intelligent_System_Assignment_1_.ipynb` to [Google Colab](https://colab.research.google.com/) or open directly from GitHub.

### 3. Set API Keys in Colab Secrets
Go to **Colab → Secrets (🔑)** and add:

| Secret Name | Where to get it |
|---|---|
| `GROQ_API_KEY` | [console.groq.com](https://console.groq.com) |
| `TAVILY_API_KEY` | [tavily.com](https://tavily.com) |

### 4. Prepare a Google Sheet
- Create a new Google Sheet
- Share it with your Google account
- Copy the URL — you'll paste it at the end of the pipeline

### 5. Run the notebook
Run all cells top to bottom. When prompted:
- Upload your CV as a PDF
- Confirm or override the job title and location
- Paste your Google Sheet URL at the end

---

## Tech Stack

| Tool | Purpose |
|---|---|
| [Groq](https://groq.com) | LLM inference (LLaMA 3.3 70B) |
| [Tavily](https://tavily.com) | Real-time job search API |
| [Sentence Transformers](https://sbert.net) | CV ↔ Job semantic matching |
| [PyMuPDF](https://pymupdf.readthedocs.io) | PDF text extraction |
| [gspread](https://docs.gspread.org) | Google Sheets integration |
| Google Colab | Execution environment |

---

## Project Structure

```
agentic-job-pipeline/
├── Intelligent_System_Assignment_1_.ipynb   # Main notebook
├── README.md
└── .gitignore
```

---

## Example Output

```
🚀 Starting Agentic AI Job Application Pipeline...

CV text extracted (first 500 chars): ...

✅ Found 8 real job pages
🔍 Top jobs ranked by CV match:

1. Software Engineer - TechCorp
   🌐 URL: https://techcorp.com/jobs/swe
   ✅ Similarity Score: 0.8741

📄 Sample Cover Letter of 1st Best Matched Job:

Dear Hiring Manager,
I am writing to express my interest in...

✅ Jobs & cover letters saved to Google Sheet.
🎉 Pipeline complete!
```

---

## Notes

- All LLM calls use `temperature=0.0` for CV parsing (deterministic) and `temperature=0.6` for cover letters (creative but grounded)
- Job search skips major aggregators to surface direct employer listings
- The Google Sheet coordinator requires Colab authentication — it won't work outside Colab without modifying the auth flow

---

## Author

**Ujjwal** — MSc Artificial Intelligence, University of East London  
[LinkedIn]([https://linkedin.com/in/YOUR_PROFILE](https://www.linkedin.com/in/ujjwal-baral-26682518a/)) · [GitHub](https://github.com/uzwall)
