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

**render.yaml** — Automatic deployment config
===============================================

`render.yaml` is an **Infrastructure as Code** file that tells Render.com exactly how to build and run your app without manual setup in the dashboard. It defines:

- **type: web** → Creates a web service (not static site).
- **buildCommand** → Installs Python dependencies from `requirements.txt`.
- **startCommand** → Runs the app using `gunicorn` with Uvicorn worker (production-ready).
- **envVars** → Sets Python version to 3.13.
- **plan: free** → Uses Render's free tier (included; auto-spins down after 15 min inactivity).

### Deploy to Render (automated via render.yaml)

**Step 1: Check render.yaml is in the repo**
- File location: `/render.yaml` (repo root)
- Already committed and pushed to `main` branch

**Step 2: Connect repo to Render.com**
- Go to https://dashboard.render.com
- Click **New +** → **Web Service**
- Select **Connect a repository**
  - Search for `musical-pancake-rumors`
  - Click **Connect**

**Step 3: Render auto-detects render.yaml**
- Render automatically reads `render.yaml` from your repo root
- No need to manually enter Build Command or Start Command
- Configuration is **instant** — just click **Deploy**

**Step 4: Deploy**
- Click the **Deploy** button
- Render clones your repo, installs dependencies, and starts the server
- Wait ~1–2 minutes for deployment to complete

**Step 5: Test your live app**
- After deploy, Render provides a live URL: `https://<your-service>.onrender.com`
- Test endpoints:

```bash
# HTML page
curl https://<your-service>.onrender.com/

# JSON API
curl https://<your-service>.onrender.com/api
```

### Why render.yaml?

- **No manual config**: Render reads the file automatically — no risk of typos in the dashboard.
- **Reproducible**: Same deployment every time; team members just push to repo.
- **Version control**: Changes to build/start commands are tracked in git.
- **Alternative to Docker**: Simpler than writing a `Dockerfile` for this use case.

### Quick local test (templates + static)

```bash
python -m pip install -r requirements.txt
uvicorn main:app --reload --host 0.0.0.0 --port 8000
# open http://127.0.0.1:8000/
```

### Troubleshooting

- **"Build failed"**: Check Render logs → usually missing dependency. Add to `requirements.txt`.
- **"Service crashed"**: Check Start Command syntax. Run locally first: `gunicorn -k uvicorn.workers.UvicornWorker main:app --bind 0.0.0.0:8000 --workers 1`
- **Static files not loading**: Ensure `static/` and `templates/` folders exist and are committed.
- **Slow startup**: Free tier spins down after 15 min; first request takes ~30 sec.

