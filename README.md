# AI Career Mentor Agent

Personalized career guidance built with an agentic AI workflow. The project takes a candidate's resume, compares it against a set of job postings, figures out which role fits best, spots the skill gaps, and generates a short learning roadmap — all through a chain of small, focused agents built with LangGraph.

## Why this project

Most student NLP projects stop at "classify the resume category." This one goes a step further and turns the analysis into a decision-making pipeline: each agent does one job and passes its output to the next, similar to how a real career counseling workflow would be broken down into steps.

## What it does

- Cleans and preprocesses resume and job description text (NLTK tokenization, stopword removal, lemmatization)
- Matches a resume against job postings using TF-IDF + cosine similarity
- Extracts known skills from resume text
- Runs a 7-agent LangGraph pipeline to produce a full career report
- Keeps a lightweight in-session memory of past analyses
- Exports results to `outputs/career_report.csv`

## Agent pipeline

```
Resume Analysis Agent
        |
Skill Extraction Agent
        |
Career Recommendation Agent
        |
   Skill Gap Agent
        |
  Career Score Agent
        |
Learning Roadmap Agent
        |
  Career Report Agent
```

Each agent reads and writes to a shared `AgentState` object, so the graph carries context forward instead of recomputing things.

## Project structure

```
AI Career Mentor Agent/
├── data/
│   ├── resume_dataset.csv
│   └── job_descriptions.csv
├── outputs/
│   └── career_report.csv
├── AI_Career_Mentor_Agent.ipynb
├── requirements.txt
└── README.md
```

## Datasets

**resume_dataset.csv** — `category`, `job_title`, `Text`
**job_descriptions.csv** — `job_id`, `category`, `job_title`, `job_description`, `job_skill_set`

Both are synthetic but structured to look like real resume/job posting data, covering 10 tech career categories (Data Science, Web Development, Cloud Computing, Cyber Security, etc.).

## Setup

```bash
pip install -r requirements.txt
```

The notebook downloads the required NLTK corpora (`punkt`, `stopwords`, `wordnet`) on first run.

## Running it

Open `AI_Career_Mentor_Agent.ipynb` and run all cells top to bottom. It runs comfortably on a laptop (tested target: Ryzen 5, 8 GB RAM) — no GPU or heavy transformer models needed.

## Output

For each resume processed, the pipeline produces:

- Recommended job title
- Career match score
- Extracted user skills
- Missing skills for the recommended role
- A short learning roadmap
- Suggested mini-projects to close the skill gap
- A one-paragraph career summary

Results for every resume run in a session are collected and exported to `outputs/career_report.csv`.

## Tech stack

pandas, numpy, matplotlib, seaborn, NLTK, scikit-learn (TF-IDF, cosine similarity), LangGraph, LangChain

## Possible extensions

- Swap TF-IDF for sentence embeddings for better semantic matching
- Add a Streamlit front end for resume upload
- Persist memory across sessions instead of just in-memory dict
- Add a resume-improvement suggestion agent
