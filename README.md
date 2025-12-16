# Repo Setup

> [!IMPORTANT]
> The initial repo clone will fail on fetching lfs assets - extra steps are needed to set up lfs and point it to your S3 bucket

## Initial Setup

Prepare this repository for normal git use on a new host

1. Install [`lfs-dal`](https://github.com/regen100/lfs-dal) (eg via aur: [`lfs-dal-git`](https://aur.archlinux.org/packages/lfs-dal-git), or via binaries from [releases](https://github.com/regen100/lfs-dal/releases))

    Make sure it is in your `PATH`

2. Clone this git repo:
    *Skips smudging LFS files - LFS is not set up yet*

    `GIT_LFS_SKIP_SMUDGE=1 git clone <repository>`

3. Include repo `.gitconfig` into local git config:

    `git config --local include.path ../.gitconfig`
    
4. Set up lfs-dal credentials

    Populate `.lfsdalconfig`. See the [`.lfsdalconfig.example`](.lfsdalconfig.example) for reference.

    **Do not commit this file. Currently, `.gitignore` excludes it.**

    _Strong suggestion: use [sops](https://github.com/mozilla/sops) and commit the encrypted credentials to the repo!_

5. Reset working tree:

    `git reset --hard main`
    
6. Fetch lfs assets:

    `git lfs pull`
    
## Use sops for credentials (optional)

Set up your repo with sops by creating a `.sops.yaml` (eg, using [sops pgp](https://github.com/getsops/sops?tab=readme-ov-file#22encrypting-with-gnupg-subkeys))

Populate `.lfsdalconfig` with desired values (use `.lfsdalconfig.example` as a reference)
Encrypt the file with sops and commit to the repo:

```
sops -e .lfsdalconfig > .lfsdalconfig.enc
git commit -a -m "Add encrypted .lfsdalconfig credentials" && git push
```

Then, on a new host, decrypt the file:

```
sops -d .lfsdalconfig.enc > .lfsdalconfig
```
