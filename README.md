# ResCheck: Resume and Job Description Matcher

## Student & Batch Details
- *Name:* Abhinav Awasthi
- *Registration Number:* 23FE10CDS00388
- *Branch:* Data Science and Engineering
- *Batch:* Batch F
- *GitHub Username:* Abhinav-Analyst7
- *Project Title:* ResCheck — Resume-to-Job Matching and Screening
- *Training Program:* NLP Capstone Project Training Program

---

## Project Details
- **Project Title:** ResCheck — Resume-to-Job Matching and Screening
- **Project Type:** NLP prototype
- **Backend:** Python, FastAPI, Sentence Transformers
- **Frontend:** React, Vite

---

## Project Overview
ResCheck is a web application prototype for comparing resumes with a target job description. It extracts resume text, calculates semantic similarity, identifies keyword overlap, and provides single-resume results or a ranked batch leaderboard.

The current implementation includes:
- **Resume parsing:** Extracts text from PDF and DOCX files.
- **Semantic matching:** Uses the `all-MiniLM-L6-v2` Sentence Transformers model to calculate a similarity score.
- **Keyword gap analysis:** Compares terms from the resume and job description and returns matched and missing keywords.
- **Batch ranking:** Scores multiple resumes and orders results by match score.
- **Resume tailoring:** Builds suggestions from resume text, job requirements, and missing skills; can call Gemini or OpenAI when configured with an API key.
- **Web interface:** React dashboard for single-resume matching and batch screening.

> The score is an estimate of textual similarity, not a probability of being hired. The implementation does not currently provide a calibrated confidence score or a validated hiring recommendation.

---

## Repository Structure
```text
.
├── README.md                   # Project documentation
├── prompt.md                   # Project context and assistant guidance
├── main.py                     # FastAPI application used by the frontend
├── api.py                      # Separate FastAPI implementation with overlapping routes
├── requirements.txt            # Python dependencies
├── package.json                # Root JavaScript development dependencies
├── src/
│   ├── parser.py               # PDF and DOCX text extraction
│   ├── preprocessor.py         # Text cleaning and keyword extraction
│   ├── matcher.py              # Semantic scoring and keyword-gap analysis
│   └── tailor.py               # Resume-tailoring prompts and optional LLM calls
├── frontend/
│   ├── package.json            # Frontend dependencies and scripts
│   └── src/
│       ├── App.jsx             # Landing/dashboard navigation
│       ├── ResCheckLanding.jsx # Landing page
│       └── ScreeningDashboard.jsx # Single and batch screening interface
├── data/                       # Job-description, leaderboard, and resume files
└── prototype.ipynb             # Exploratory notebook
```

---

## Installation & Setup Guide

### 1. Install Python dependencies
From the project root, create and activate a virtual environment, then install the requirements:

```powershell
py -m venv .venv
.\.venv\Scripts\Activate.ps1
py -m pip install -r requirements.txt
```

The first backend startup may download the `all-MiniLM-L6-v2` model and require an internet connection.

### 2. Start the API
From the project root:

```powershell
py -m uvicorn main:app --reload
```

The API is available at `http://127.0.0.1:8000`; interactive API documentation is at `http://127.0.0.1:8000/docs`.

### 3. Start the frontend
In a second terminal:

```powershell
cd frontend
npm install
npm run dev
```

Open the local URL printed by Vite (typically `http://localhost:5173`). The frontend currently sends API requests to `http://127.0.0.1:8000`.

---

## Key Features
1. **Single Resume Match:** Upload a resume and enter a job description to view a semantic match score, matched and missing keywords, and a short resume excerpt.
2. **Batch Leaderboard:** Upload multiple resumes to score, rank, search, and paginate the resulting candidates.
3. **Resume Parsing:** Extracts text from PDF and DOCX resume files for analysis.
4. **Keyword Gap Analysis:** Shows job-description terms found in the resume and terms not found.
5. **Tailoring Suggestions API:** Creates resume bullet-point suggestions while instructing the model not to invent experience or metrics. Gemini or OpenAI use requires a configured API key.
6. **Theme Toggle:** Switches the web interface between dark and light themes.

---

## Matching Workflow
```text
Resume (PDF/DOCX)             Job Description
        │                            │
        ▼                            ▼
   Text extraction              Text input
        │                            │
        └─────────────┬──────────────┘
                      ▼
             Semantic similarity
             Keyword gap analysis
                      │
                      ▼
          Score, matched/missing terms,
              or ranked leaderboard
```

In `main.py`, the match score is based on sentence-embedding cosine similarity. Keyword matches and gaps are returned as separate explanatory results, they are not currently combined into the score.


