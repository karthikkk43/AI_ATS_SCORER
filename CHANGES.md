# Package changes

Frontend presentation changes:
- `frontend/assets/styles.css`: teal accent palette, darker secondary text, coordinated Streamlit button/tab colors, visible keyboard focus, mobile hero sizing, reduced-motion support, and a repaired malformed CSS comment.
- `frontend/views/landing.py`: teal header gradient and a softer matching shadow. Existing content, layout, navigation and behavior are preserved.
- `frontend/components/_helpers.py`: darker amber text for score and severity readability. Score thresholds and severity selection are unchanged.

Packaging additions:
- `.gitignore`: excludes local secrets, environments, caches and logs.
- `.gitattributes`: uses Git LFS for model weights.
- `.env.example`: blank local configuration template; contains no real credentials.
- `requirements-local.txt`: references the original requirements and pins the three model-runtime packages to the model's saved versions. Use Python 3.12.
- `RUN_AND_PUSH.md`: local startup and user-run GitHub push instructions.
- `CHANGES.md`: this change record.

The complete contents of every changed file are included in this folder and ZIP; no source sections are omitted. All backend files and the original requirements.txt are unchanged. All files copied from the supplied model folder are unchanged. Research notebooks, local environments, secret configuration, and Git history are not included.
