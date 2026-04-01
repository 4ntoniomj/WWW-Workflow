https://roadmap.sh/projects/github-actions-deployment-workflow

---

# GitHub Actions
A simple workflow for deploy static website in GitHub Pages.
## Code
```yaml
# Project name.
name: Deploy GitHub Pages

# When deploy.
on:
  push:
    branches: ["main"]
  # Or manually.
  workflow_dispatch:

# Permissions for access to GitHub Pages.
permissions:
  contents: read
  pages: write
  id-token: write

# Prevent multiple deployments from running at the same time to avoid race conditions.
concurrency:
  group: "pages"
  cancel-in-progress: false

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      # Get project.
      - name: Checkout
        uses: actions/checkout@v4
      # Prepare the context for GitHub Pages to accept a deploy from that workflow.
      - name: Setup Pages
        uses: actions/configure-pages@v5
      - name: Build with Jekyll
        uses: actions/jekyll-build-pages@v1
        with:
          source: ./
          destination: ./_site
      # Uploading files to GitHub internal storage.
      - name: Upload artifact
        uses: actions/upload-pages-artifact@v3

  deploy:
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    runs-on: ubuntu-latest
    # Requirement.
    needs: build
    # Deploy the web.
    steps:
      - name: Deploy to GitHub Pages
        id: deployment
        uses: actions/deploy-pages@v5
```
[Website](https://4ntoniomj.github.io/WWW-Workflow/)
