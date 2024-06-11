# Repo Setup



> [!IMPORTANT]
> The initial repo clone will fail on fetching lfs assets - extra steps are needed to set up lfs and point it to the S3 bucket

## Setup (one-time)

1. Install [`lfs-s3`](https://github.com/nicolas-graves/lfs-s3) (eg via aur: [`lfs-s3-git`](https://aur.archlinux.org/packages/lfs-s3-git))
2. Clone git repo:

    `GIT_LFS_SKIP_SMUDGE=1 git clone <repository>`

3. Source `.gitconfig` - run:

    `git config --local include.path ../.gitconfig`

4. Reset working tree:

    `git sec reset --hard main`

## Pushing/Pulling

The credentials for S3 LFS should be encrypted in `credentials.asc` (replace the template and encrypt with gpg) - they need to be provided as env variables to `lfs-s3`

A convenient way of doing so to use the `sec` prefix alias from `.gitconfig`, eg:

```
git sec push
git sec lfs push --all origin main
```

Using `sec` before any git command automatically decrypts and exports the credential variables and makes them available.
