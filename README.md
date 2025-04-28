# Job Recommendation Tool (FastAPI + Gemini + Embeddings)

This project is a resume-to-job matching system built with **FastAPI**, **Gemini (LLM)**, **SBERT embeddings**, and a local **SQLite** database of job listings. It allows users to upload a PDF resume and returns the top job matches, complete with a match score, matched skills, and match reason.

![Homescreen](img_scr/HomeScreen.png)

---

## 🚀 Features

- ✅ PDF resume parsing with `PyMuPDF`
- ✅ Skill extraction using Gemini
- ✅ Job embeddings with SentenceTransformers (SBERT)
- ✅ Hybrid ranking with embeddings + Gemini for match reason
- ✅ Word cloud generation
- ✅ Full CRUD for jobs via FastAPI
- ✅ Docker-ready for deployment

---

## 🧱 Project Structure

```
backend/
├── app/
│   ├── api/                 # FastAPI routes
│   ├── db/                  # SQLAlchemy database setup
│   ├── models/              # ORM models
│   ├── schemas/             # Pydantic models
│   ├── scripts/             # Data loaders and utilities
│   └── services/            # Business logic (matcher, parser, etc.)
├── alembic/                 # Database migrations
├── app.db                   # SQLite database (not included in Docker image)
├── requirements.txt
├── Dockerfile
```

---

## 🧪 Local Development

### 1. Clone the Repository

```bash
git clone https://github.com/your-username/resume-recommendation-tool.git
cd resume-recommendation-tool/backend
```

### 2. Set Up Environment

Use `venv` or `conda`. Example with `venv`:

```bash
python -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
python -m spacy download en_core_web_sm
```

### 3. Add your Gemini API key to environment variable (.env)

### 4. Run the API with Uvicorn

```bash
uvicorn app.main:app --reload
```

> The API will be available at http://127.0.0.1:8000

You can now upload resumes via `/docs` or use Postman/your frontend.

---

## 🐳 Docker Deployment

### 1. Build Docker Image

```bash
docker build -t resume-api .
```

### 2. Run with SQLite DB Mounted

```bash
docker run -it --rm   -v /path/to/resumeapp.db:/app/app.db   -p 8000:8000   resume-api
```

Or if using **local Ollama**, use host networking:

```bash
docker run -it --rm   --network host   -v /path/to/resumeapp.db:/app/app.db   -p 8000:8000   resume-api
```

### 3. Enable Auto-Restart on Reboots

```bash
docker run -d   --restart unless-stopped   --network host   -v /home/ubuntu/resumeapp.db:/app/app.db   -p 8000:8000   resume-api
```

---

## 🔗 API Endpoint

### `POST /resume/match`

**Request:**

- Content-Type: `multipart/form-data`
- Field: `file` → your `.pdf` resume

**Response:**

```json
{
  "resume_skills": [...],
  "matches": [
    {
      "jobId": 123,
      "matchScore": 0.78,
      "matchedSkills": [...],
      "matchReason": "Mentions Java and CI/CD, which align with this job."
    }
  ],
  "wordCloud": [
    { "text": "python", "value": 5 },
    ...
  ]
}
```

---

## 🧠 Matching Logic

1. Extracts text from PDF using `PyMuPDF`
2. Skills are extracted using Gemini Flash 2.0
3. Skills and jobs are embedded using SBERT (MiniLM)
4. Top 100 job matches are retrieved using Cosine similarity
5. These top 100 jobs along with the resume are sent as RAG to Gemini which returns top 10 best matches jobs

![JobMatches](img_scr/JobMatches.png)

![img](img_scr/Match_2.png)

---

## ⚠️ Notes

- The database (`app.db`) is **not included in the Docker image**
- Make sure you have a Gemini api key before running the backend
- Word clouds use extracted skills and frequency from the full resume

---

## 📊 Metrics/Eval

* Created script to manually evaluate 200 reumes/job posting pairs (10 job postings per resume 20 resumes in total).
* Evaluated the accuracy of 7 tokenizer/ranking algorithm pair for job matching/resume pairing task including BERT/BM25, MiniLM/Cosine, MpNet/Bm25, Roberta/BM25, WhitespaceTokenization/Bm25, and LongFormer4096/BM25.
* Testing showcased MiniLM/Cosine was the best performing combo and was therefore used for initial job posting filtering.

![Metrics](img_scr/Metrics.png)

---

## 🔜 Future Plans

* Login and User Accounts: Allow users to login and save resumes and view past job matches.
* Multi-Resume: Allow users to upload multiple resumes bound to an account to visualize different matches for a particular resume.
* Updated Job Pool: Utilize Linkedin or Indeed API for real time job matching.
* Recommendation System: Recommend certain skills or requirements for specific roles a user desires in order to become a better candidate match for a job description.
* Resume Pooling: Pool together top scoring resumes for certain positions in order to better allow for resume recommendations for a particular user.
* Resume Feedback: Suggust improvements to a resume for top job matches based on other top scoring resumes.
* Multilingual Resume Support: Extend NLP pipeline to process resumes in other languages (e.g. Spanish).
* Visual PDF Reports: Export match results and resume analysis as professional PDFs.

---

📚 License

MIT License
