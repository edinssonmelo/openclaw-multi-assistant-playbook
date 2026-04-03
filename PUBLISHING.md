# Publishing This Playbook as Its Own Repository

This folder is designed to be copied or versioned as a **standalone public repository** without exposing the rest of a private infrastructure stack.

## Option A - Keep it inside a monorepo

If this playbook lives inside a larger repository, readers can clone only this folder with [git sparse-checkout](https://git-scm.com/docs/git-sparse-checkout) or copy the directory manually.

## Option B - Publish it as a new repository

On your machine:

```bash
cd openclaw-multi-assistant-playbook
git init
git add .
git commit -m "docs: initial OpenClaw multi-assistant playbook"
```

Create an empty repository on GitHub or GitLab and connect it:

```bash
git remote add origin https://github.com/<your-user>/<repo-name>.git
git branch -M main
git push -u origin main
```

## Before making it public

- Check that `docs/` and `templates/` do not contain personal notes or environment-specific secrets.
- Do not publish `.env` files, API keys, SSH keys, production domains, chat IDs, or screenshots with sensitive data.
- Optionally add `CODE_OF_CONDUCT.md`, issue templates, or contribution guidelines if you want broader community reuse.
