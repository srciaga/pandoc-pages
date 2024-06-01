# Pandoc Conversion and Pages Deployment Action

> [!NOTE]
> This action is for my personal use and hasn't been thoroughly tested.

This GitHub Action creates and deploys a single-page GitHub Pages website from the `gh-pages` branch. It converts content from a Markdown file to HTML using Pandoc whenever changes are made. It also deploys the website if any other relevant files are modified.

## Repository Structure

Here's what your repo structure's `main` branch should look like:

```
- your-repo/
  ├── .github/
  │   └── workflows/
  │       └── convert-and-deploy.yml     # Workflow to call this action
  ├── assets/
  │   ├── index.md                       # Markdown file to be converted
  │   └── template.html                  # HTML template for conversion
  ├── public/                            # Files to deploy
  │   └── index.html                     # Converted HTML file
  │   └── ...                            # Other public/ files and directories
  └── ...                                # Other project files and directories
```

## Setup

1. Go to *Settings > Actions > General > Workflow permissions* and enable Read and write permissions.
  
2. Ensure that the required files are in the `main` branch's root directory:

```
mkdir -p assets public && touch assets/index.md assets/template.html public/index.html
```

3. Create the workflow:
```
mkdir -p .github/workflows && touch .github/workflows/convert-and-deploy.yml
```
Paste the following into `convert-and-deploy.yml`:

```yaml
name: "Convert & Deploy"
on:
  push:
    paths:
      - "assets/index.md"
      - "assets/template.html"
      - "public/**"
  workflow_dispatch:
jobs:
  convert:
    runs-on: ubuntu-latest
    steps:
      - name: "Run Pandoc Conversion and Pages Deployment action"
        uses: srciaga/pandoc-pages@main
        with:
          gh_token: ${{ secrets.GITHUB_TOKEN }}
```
4. Push all the file's you've added so far.
3. The workflow should run and create the `gh-pages` branch. If not, go to the *Actions* tab, click on the **Convert & Deploy** workflow. Run the workflow from the `main` branch.
4. Go to *Settings > Pages > Build and deployment* and configure the following:
   - *Source*: Deploy from a branch
   - *Branch*: `gh-pages` / (root)
5. `git pull` and push modifications to `index.md` and `template.html`. Your website will automatically deploy! Remember to pull after each deployment.

There are sample files for [`index.md`](https://github.com/srciaga/pandoc-pages/blob/344afebea560e60dcf124face84c32cb2cc3db02/sample-index.md) and [`template.html`](https://github.com/srciaga/pandoc-pages/blob/344afebea560e60dcf124face84c32cb2cc3db02/sample-template.html) in this repo.

Check out the Pandoc User's Guide to learn more about [templates](https://pandoc.org/MANUAL.html#templates) and [Pandoc's extended markdown](https://pandoc.org/MANUAL.html#pandocs-markdown).
