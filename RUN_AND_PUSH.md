# Run locally and push to GitHub

This package includes the complete existing backend and frontend, plus the model copied from the supplied Desktop model folder. It has no Git history. Backend source and model contents are unchanged. Your existing repository and local secret configuration have not been modified.

## Run locally

Use Python 3.12. The supplied model was saved with Sentence Transformers 5.7.0, Transformers 5.20.2, and PyTorch 2.11.0. `requirements-local.txt` installs the existing dependencies with these compatible versions.

Open a terminal in this extracted folder:

```bash
python3.12 -m venv .venv
source .venv/bin/activate
python -m pip install -r requirements-local.txt
python -m spacy download en_core_web_sm
```

Create your local configuration using the blank template, then fill it in using your existing configuration. Do not overwrite a configured `.env` file:

```bash
cp -n .env.example .env
```

Use your existing Supabase and Groq configuration. The backend needs Supabase configuration for authenticated requests and saved history; the frontend needs `SUPABASE_URL` and `SUPABASE_ANON_KEY` to sign in. Keep the JWT verification configuration matching your existing Supabase project. Set the Google OAuth redirect to `http://localhost:8501` in the provider if using Google sign-in locally.

Both servers must start from this project folder. The model path below overrides any old model path in your environment without modifying the model or application code.

Terminal 1:

```bash
source .venv/bin/activate
SENTENCE_TRANSFORMER_MODEL=./finetuned-bert python -m uvicorn backend.main:app --host 127.0.0.1 --port 8000
```

Terminal 2:

```bash
source .venv/bin/activate
python -m streamlit run frontend/streamlit_app.py --server.address 127.0.0.1 --server.port 8501
```

Open http://localhost:8501. API documentation is at http://localhost:8000/docs. Confirm http://localhost:8000/api/v1/health reports both `nlp_loaded` and `embedder_loaded` as true.

If ports 8000 or 8501 are already occupied by the previously hosted copy, stop that copy before launching this one. Stop these foreground servers with Ctrl+C in their respective terminals.

PDF export also requires the WeasyPrint native libraries. Keep the working system installation on your computer; on a new macOS machine, install Pango with `brew install pango`. For Linux prerequisites, see the original README.

## Update this GitHub repository

This project is published to `https://github.com/karthikkk43/AI_ATS_SCOREr`.
Install Git LFS before cloning and run `git lfs pull` after cloning to retrieve model weights. Git LFS storage and bandwidth limits apply.

After editing your clone, review the changed files and commit only the intended project files. Do not stage local secrets.

```bash
git status --short
git add frontend backend finetuned-bert requirements.txt requirements-local.txt README.md RUN_AND_PUSH.md CHANGES.md VERIFICATION.md .gitignore .gitattributes .env.example
git lfs ls-files
git diff --cached --stat
git commit -m "Update ATS scorer"
git push origin main
```

Use your existing Git identity. No authorship or collaborator setting changes are required.
