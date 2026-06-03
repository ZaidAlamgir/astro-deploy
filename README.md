# Astro Site Compiler & Deployer (GitHub Actions)

This repository acts as a cloud compiler and deployer for your Astro website. It runs on GitHub Actions' free servers, pulling your source code and private content from GitLab, building it, and pushing the compiled output back to your GitLab Pages.

## How it works

1. **Trigger:** The workflow is triggered by pushes to this repository, manual runs, or webhooks (Repository Dispatch) from your GitLab content repositories.
2. **Build:** GitHub Actions spins up an Ubuntu cloud server, checks out your private website source (from your GitLab `astro-migration` branch), and pulls all 29 of your private content repositories.
3. **Compile:** It runs `npm run build` to compile the Astro site.
4. **Deploy:** It packages the built HTML/JS, adds a simple `.gitlab-ci.yml` that tells GitLab Pages to publish it, and force-pushes it back to the `main` branch of your GitLab project `website-host-astro`.
5. **GitLab Pages Publish:** GitLab sees the push, runs a 5-second publish job (since no compilation is needed there anymore), and hosts your site.

## Secrets to Configure on GitHub

Before running this workflow, you must add the following Secret to this GitHub repository:

### 1. `GITLAB_PAT`
* **What it is:** A GitLab Personal Access Token (PAT) with `api`, `read_repository`, and `write_repository` scopes.
* **Why it is needed:** This allows GitHub Actions to securely authenticate and clone your private code/content from GitLab, and write the built files back to your GitLab Pages project.
* **How to add it on GitHub:**
  1. Go to your GitHub repository Settings.
  2. Click **Secrets and variables** -> **Actions** in the sidebar.
  3. Click **New repository secret**.
  4. **Name:** `GITLAB_PAT`
  5. **Value:** Paste your GitLab Personal Access Token.
  6. Click **Add secret**.
