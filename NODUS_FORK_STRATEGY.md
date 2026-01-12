# Nodus Fork Strategy for google/adk-python

## Remotes
- **upstream**: https://github.com/google/adk-python.git (Google official)
- **origin**: https://github.com/nodus-factory/adk-python.git (Nodus fork)

## Branches
- **upstream-main**: Pure tracking of upstream/main (NEVER modify manually)
- **nodus-main**: Nodus patches rebased on upstream-main (our working branch)
- **Production tags**: v0.X.Y-nodus.N

## Sync Workflow

### Update from Google:
```bash
git checkout upstream-main
git fetch upstream
git reset --hard upstream/main
git push origin upstream-main --force
```

### Rebase our patches:
```bash
git checkout nodus-main
git rebase upstream-main
# Resolve conflicts if any
git push origin nodus-main --force
```

## Usage in nodus-adk-runtime

pyproject.toml dependency:
```toml
[project.dependencies]
google-adk = { git = "https://github.com/nodus-factory/adk-python.git", branch = "nodus-main" }
```

## Why This Strategy?

1. **upstream-main** keeps a clean copy of Google's main branch
   - Acts as our source of truth for upstream changes
   - Never touched manually, only hard reset from Google
   
2. **nodus-main** contains our minimal patches
   - Rebased on top of upstream-main regularly
   - Keeps our changes separate and reviewable
   - Easy to upstream contributions back to Google

3. **Production tags** ensure stability
   - Tag tested versions: `v0.3.0-nodus.1`
   - Deploy from tags, not from moving branches
   - Easy rollback if issues arise

## Adding a Nodus Patch

```bash
git checkout nodus-main
# Make your changes
git commit -m "fix: your patch description"
git push origin nodus-main
```

## Tagging for Production

```bash
git checkout nodus-main
git tag -a v0.3.0-nodus.1 -m "Production release based on ADK v0.3.0"
git push origin v0.3.0-nodus.1
```

Then update nodus-adk-runtime to use the tag:
```toml
google-adk = { git = "https://github.com/nodus-factory/adk-python.git", tag = "v0.3.0-nodus.1" }
```

## Principles

- ✅ Keep patches minimal
- ✅ Document all changes
- ✅ Sync with Google regularly
- ✅ Consider upstreaming useful patches
- ❌ Never modify upstream-main manually
- ❌ Avoid long-lived divergence from Google


