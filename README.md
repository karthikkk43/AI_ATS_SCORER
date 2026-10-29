# AI_ATS_SCOREr

AI-assisted resume analysis with a Streamlit frontend, FastAPI backend, and a bundled fine-tuned BERT model. Upload a resume, optionally supply a job description, and review an ATS score with actionable feedback.

## Features

- Resume uploads in PDF, DOC, and DOCX formats.
- Overall ATS score and a breakdown across formatting, keywords and skills, content quality, skill validation, and ATS compatibility.
- Semantic skill validation and job-description comparison using the included fine-tuned model.
- Suggestions generated through Groq.
- Supabase email/password and Google sign-in, with saved analysis history.
- Downloadable summaries and PDF reports.
- A teal frontend palette with visible keyboard focus and reduced-motion styles.

## Stack

| Layer | Technology |
| --- | --- |
| Frontend | Streamlit |
| API | FastAPI |
| Semantic model | Fine-tuned BERT through Sentence Transformers |
| Text processing | spaCy (`en_core_web_sm`) |
| Suggestions | Groq |
| Authentication and storage | Supabase |
| PDF export | Jinja2 and WeasyPrint |

Resume analysis can call configured external services for suggestions, authentication, and storage. Treat resume data accordingly.

## Project structure

```text
AI_ATS_SCOREr/
├── backend/                  API, scoring, parsing, authentication, and database integration
├── frontend/                 Streamlit views, components, services, and styles
├── finetuned-bert/            Model weights, tokenizer, and model configuration
├── .env.example              Blank configuration template
├── .gitattributes            Git LFS rules for model weights
├── .gitignore                Secret and runtime exclusions
├── requirements.txt          Original application dependencies
├── requirements-local.txt    Dependencies with compatible model-runtime versions
├── RUN_AND_PUSH.md           Additional setup instructions
├── CHANGES.md                Frontend and packaging changes
└── VERIFICATION.md           Checks performed on the packaged application
```

## Requirements

Use **Python 3.12**. The bundled model was saved with Sentence Transformers **5.7.0**, Transformers **5.20.2**, and PyTorch **2.11.0**. Those versions are pinned in `requirements-local.txt`.

Install Git LFS before cloning so the model weights are downloaded instead of a pointer file. On macOS with Homebrew:

```bash
brew install git-lfs
git lfs install
```

Then clone and install:

```bash
git clone https://github.com/karthikkk43/AI_ATS_SCOREr.git
cd AI_ATS_SCOREr
git lfs pull
python3.12 -m venv .venv
source .venv/bin/activate
python -m pip install -r requirements-local.txt
python -m spacy download en_core_web_sm
```

On Windows, activate the environment with `.venv\Scripts\activate` instead.

PDF export requires WeasyPrint's native libraries. On macOS, install Pango with `brew install pango`. On Debian/Ubuntu, install `libpango-1.0-0` and `libpangoft2-1.0-0` through the system package manager.

## Local configuration

Copy the blank template to `.env` if you do not already have local configuration:

```bash
cp -n .env.example .env
```

Fill in your own values locally:

| Variable | Purpose |
| --- | --- |
| `SUPABASE_URL` | Supabase project URL |
| `SUPABASE_KEY` | Backend service-role key for database operations |
| `SUPABASE_ANON_KEY` | Frontend Supabase client key |
| `SUPABASE_JWT_SECRET` | JWT verification configuration used by the existing backend |
| `GROQ_API_KEY` | Groq-generated suggestions |
| `AUTH_REDIRECT_URL` | Local Google OAuth callback, normally `http://localhost:8501` |
| `SENTENCE_TRANSFORMER_MODEL` | Bundled model path: `./finetuned-bert` |

The service-role key must remain in backend/local configuration. Never commit `.env`, real API keys, or `.streamlit/secrets.toml`. Keep credentials out of frontend source and browser-delivered assets. The project includes only blank/example configuration.

For Google sign-in, configure the matching local redirect URL in your existing authentication setup. The original authentication and database code is unchanged.

## Start the application

Run both commands from the repository root in separate terminals with the virtual environment activated.

Backend:

```bash
source .venv/bin/activate
SENTENCE_TRANSFORMER_MODEL=./finetuned-bert python -m uvicorn backend.main:app --host 127.0.0.1 --port 8000
```

Frontend:

```bash
source .venv/bin/activate
python -m streamlit run frontend/streamlit_app.py --server.address 127.0.0.1 --server.port 8501
```

- Application: http://localhost:8501
- API documentation: http://localhost:8000/docs
- Model health: http://localhost:8000/api/v1/health

The health endpoint should report `nlp_loaded: true` and `embedder_loaded: true`. Stop each server with Ctrl+C in its terminal.

## Model storage

`finetuned-bert/model.safetensors` is approximately 438 MB and is stored through Git LFS. If model loading fails after cloning, run `git lfs pull` and confirm the weight file is the complete binary, not a short text pointer.

The fine-tuned model and backend source have not been modified by the frontend color refresh. See `VERIFICATION.md` for the completed syntax, rendering, model-inference, and file-integrity checks and their limits.
