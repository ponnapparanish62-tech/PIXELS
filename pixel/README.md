# Pixel Dash

A lightweight browser arcade game (endless runner) — dodge obstacles, collect
coins, beat your best score. Built as a single self-contained `index.html`
so it's trivial to containerize and deploy.

This repo is set up to match the Azure DevOps CI/CD project brief: GitHub
push triggers a build pipeline that Dockerizes the app and pushes it to
Azure Container Registry (ACR); a release pipeline then deploys that image
to Azure App Service.

## Files

- `index.html` — the game (open it directly in any browser to play locally)
- `Dockerfile` — serves `index.html` via nginx on port 80
- `azure-pipelines.yml` — build + release pipeline definition
- `.gitignore`

## 1. Get this onto GitHub

From this folder, run:

```bash
git init
git add .
git commit -m "Initial commit: Pixel Dash game + Docker + Azure Pipelines"
git branch -M main
git remote add origin https://github.com/<your-username>/pixel-dash.git
git push -u origin main
```

If you don't have a GitHub repo yet:
1. Go to https://github.com/new
2. Name it (e.g. `pixel-dash`), leave it empty (no README/gitignore — you
   already have them here), click **Create repository**
3. Copy the repo URL it gives you and use it in the `git remote add` command
   above

If `git` asks you to log in, use a GitHub personal access token as the
password (Settings -> Developer settings -> Personal access tokens), or
authenticate via the GitHub CLI (`gh auth login`) first.

## 2. Try it locally (optional, no Docker needed)

Just open `index.html` in a browser, or serve it:

```bash
python3 -m http.server 8080
# visit http://localhost:8080
```

## 3. Try it in Docker (optional, before wiring up Azure)

```bash
docker build -t pixel-dash .
docker run -p 8080:80 pixel-dash
# visit http://localhost:8080
```

## 4. Wire up Azure DevOps

Following the project brief:

1. Create an Azure DevOps organization + project at https://dev.azure.com
2. **Project settings -> Service connections**, create:
   - An **Azure Container Registry** connection (points at your ACR)
   - An **Azure Resource Manager** connection (points at your subscription)
3. In the Azure portal, create:
   - A **Resource Group**
   - An **Azure Container Registry (ACR)**
   - An **App Service** (Web App for Containers) in that resource group
4. Edit `azure-pipelines.yml` in this repo and fill in the placeholders:
   - `dockerRegistryServiceConnection`
   - `containerRegistry` (your ACR login server, e.g. `myacr.azurecr.io`)
   - `azureSubscription`
   - `appServiceName`
5. In Azure DevOps, create a new pipeline, point it at this GitHub repo, and
   let it pick up `azure-pipelines.yml`
6. Run the pipeline — it builds the Docker image, pushes it to ACR, then
   deploys it to your App Service
7. Open your App Service URL — you should see Pixel Dash live
8. Make a small change to `index.html`, commit, and push — the pipeline
   should trigger automatically and redeploy

## Notes

- The pipeline triggers on pushes to `main`
- Build IDs are used as image tags so each build is traceable; `latest` is
  also pushed for convenience
