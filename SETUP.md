# Set up a private GitHub repository under `henryt19-ui`

**Important:** This repository package contains technical information derived from a draft labeled confidential/internal-use-only. Obtain Team 3 approval and choose **Private** visibility by default.

## Option 1 — GitHub website

1. Sign in to GitHub as `henryt19-ui` or an account authorized to create repos in that namespace.
2. Visit https://github.com/new, select owner `henryt19-ui`, and enter the suggested name `Musical-Instrument-Learning-Hub` (or the team-approved name).
3. Choose **Private**. Leave the README initialization unchecked because `README.md` is already included here.
4. Create the repository.
5. **Extract the ZIP locally.** In the new repository, use **Add file → Upload files** to upload the extracted contents, retaining folder structure. The GitHub web interface may require directory-by-directory upload depending on browser support; Git CLI is more reliable for hidden `.github/` files.
6. Commit the files, then ensure the GitHub README displays as intended.
7. Add approved teammates via **Settings → Collaborators** and configure branch protection on `main` if desired.

## Option 2 — Git CLI (recommended)

After creating the *empty, private* repo in the desired GitHub owner, from the extracted folder:

```bash
cd MIH-GitHub-Repository-Starter
git init -b main
git add .
git commit -m "docs: initialize Musical Instrument Learning Hub design repository"
git remote add origin https://github.com/henryt19-ui/Musical-Instrument-Learning-Hub.git
git push -u origin main
```

Use your organization-approved GitHub authentication (credential manager, SSH, or GitHub CLI). Do **not** embed personal access tokens in commands or files. If the destination repo is not empty, first clone/pull its existing history and merge files rather than forcing a push.

## Before sharing externally

- Verify GitHub repository visibility is Private and that every collaborator should have access.
- Obtain permission before making source-derived documentation public.
- Do not commit the original design Word document, real `.env` files, payment keys, Cognito credentials, account tokens, or proprietary model binaries.
- Confirm Figma access and reconcile UI specifications before claiming design implementation.
- The contents here are docs/placeholders. They do **not** create a running web app, mobile app, ECS cluster, database, or payment integration.

## Repository connection status when package was created

The GitHub connection visible to the assistant was authenticated as `tylerhenry705-lgtm`, not `henryt19-ui`; it did not have write permission to the inspected `henryt19-ui` repositories. No remote GitHub repository has been created or modified by this package process.
