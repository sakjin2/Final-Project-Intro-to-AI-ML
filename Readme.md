# Final Project: IITB Insti-Assist — A RAG-Powered AI Assistant for IIT Bombay

## Academic Assistant
A RAG-powered AI assistant that answers questions about IIT Bombay's academic policies - grading, registration, exam rules, malpractice rules, degree
requirements etc. by retrieving relevant sections from official institute rulebooks and generating an LLM's answer by using them as context. If the answer isn't supported by the retrieved documents, the assistant says "I don't know" instead of guessing.

## How it works

- **Data ingestion:** 10 official IIT Bombay PDF rulebooks (see in pdf/ directory), parsed with `pdfplumber`
- **Chunking:** character-based, 200 chars/chunk with 40 char overlap
- **Embeddings:** `sentence-transformers/all-MiniLM-L6-v2` 
- **Vector search:** FAISS `IndexFlatIP` (cosine similarity via L2 normalization), top-5 retrieval
- **Grounding:** results below a 0.25 similarity threshold are discarded;if nothing clears the bar, the assistant returns "I don't know"
- **Generation:** Gemini 3.1 Flash Lite, prompted to answer only from retrieved context and never mentions " answering from the provided text"(This was actually done after observing it's behaviour as it didn't seem like an assistant that way)
- **Source display:** every answer lists which PDF(s) it was grounded in
- **Interface:** Gradio `ChatInterface`, launched with a public share link

## Data sources

| File | Covers |
|---|---|
| `ugrulebook.pdf` | Undergraduate academic rules |
| `mtech_mba_rules.pdf` | M.Tech / MBA academic rules |
| `phd_rules.pdf` | PhD academic rules |
| `m_eng_rules.pdf` | M.Eng / Masters rules |
| `IDDDP_rules.pdf` | Inter-Disciplinary Dual Degree Programme rules |
| `grading.pdf` | Grading policy |
| `academic_calendar.pdf` | Academic calendar |
| `award_rules.pdf` | Award rules |
| `acad_malpractices_1.pdf` | Academic malpractice policy (part 1) |
| `acad_malpractices_2.pdf` | Academic malpractice policy (part 2) |

## Setup

### 1. Get a Gemini API key (free)
Grab one at https://aistudio.google.com/apikey.

### 2. Open the notebook in Colab
Use the link in this repo, or open `Final_Project.ipynb` directly in Google Colab.

### 3. Add your key as a Colab Secret
- Click the key icon in the left sidebar → **Add new secret**
  - Name: `GEMINI_API_KEY`
  - Value: your key
  - Toggle **Notebook access** on

> No Hugging Face token or login is required as the embedding model (`all-MiniLM-L6-v2`) is fully public.

### 4. Run
Run all cells top to bottom. The first cell clones this repo (including all PDFs) directly into the Colab, i.e. no manual pdf uploads needed. The final cell launches a Gradio web app with a public `.gradio.live` link (valid up to a week) alongside an inline preview in the notebook.You may click on the link to check the assistant on a separated tab , or share the link to any other device also for checking!!

## Repo structure
```
Final-Project-Intro-to-AI-ML/
├── Readme.md
├── requirements.txt
├── Colab_File_Link
├── writeup.pdf
├── Final_Project.ipynb
└── pdf/
    ├── ugrulebook.pdf
    ├── Mtech_Mba_Rules.pdf
    ├── phd_rules.pdf
    ├── M_Eng_Masters_Rules.pdf
    ├── IDDDP_rules.pdf
    ├── grading.pdf
    ├── academic_calendar.pdf
    ├── award_rules.pdf
    ├── acad_malpractices_1.pdf
    └── acad_malpractices_2.pdf
```


