# SSH/Rsync Deployment (cPanel)

## 1) Generate SSH key (local)
```bash
ssh-keygen -t ed25519 -C "deploy@pneflags" -f ~/.ssh/pneflags_deploy -N ""
```

## 2) Install public key on server
- Log in to cPanel → **SSH Access** → **Manage SSH Keys** → **Import Key**.
- Paste the contents of `~/.ssh/pneflags_deploy.pub`.
- Authorize the key.
- Ensure the key is in the user's `~/.ssh/authorized_keys` on the server.

## 3) Find your SSH host and target directory
- Host: your cPanel SSH hostname (e.g., `ssh.yourdomain.com` or main domain)
- User: your cPanel username
- Target dir: absolute path to your document root, e.g. `/home/<user>/public_html/`

## 4) Add GitHub Secrets (Repo → Settings → Secrets and variables → Actions)
- `DEPLOY_SSH_PRIVATE_KEY` → paste contents of `~/.ssh/pneflags_deploy`
- `DEPLOY_SSH_HOST` → e.g. `ssh.yourdomain.com`
- `DEPLOY_SSH_USER` → your cPanel username
- `DEPLOY_TARGET_DIR` → e.g. `/home/<user>/public_html/`
- `DEPLOY_KNOWN_HOSTS` → output of: `ssh-keyscan -t rsa,ecdsa,ed25519 ssh.yourdomain.com`

## 5) Push to main
- Push any commit to `main`. GitHub Actions will rsync the repo content to your server.
- Exclusions: `.git/`, `.github/`, `_reports/`, `_reports_fix/`

## Notes
- Ensure `public_html` exists and is writable by your user.
- The workflow uses `--delete` to remove files on the server that were deleted in the repo.
