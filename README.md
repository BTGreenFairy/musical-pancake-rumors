# musical-pancake-rumors
Testing github pages

## FastAPI hello world

This repository contains a minimal FastAPI example.

Run locally:

```bash
python -m pip install -r requirements.txt
uvicorn main:app --reload --host 0.0.0.0 --port 8000
```

Test the endpoint:

```bash
curl http://127.0.0.1:8000/
# {"message":"Hello, World!"}
```

Running as a dynamic (templated) web service
-------------------------------------------

This repo now serves a rendered HTML page at `/` (Jinja2 templates) and an API JSON at `/api`. Static files are available under `/static`.

Render.com (Web Service) deployment notes:

- Build: Render will run `pip install -r requirements.txt` automatically for Python services.
- Start Command (set in Render Web Service settings):

```bash
gunicorn -k uvicorn.workers.UvicornWorker main:app --bind 0.0.0.0:$PORT --workers 2
```

- Repository layout expected by Render: keep the repo root with `main.py`, `requirements.txt`, `templates/` and `static/`.
- If you prefer Docker, add a `Dockerfile` and choose Docker deploy.

Quick local test (templates + static):

```bash
python -m pip install -r requirements.txt
uvicorn main:app --reload --host 0.0.0.0 --port 8000
# open http://127.0.0.1:8000/
```

