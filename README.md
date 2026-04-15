# Ass_6_MLOPs — Gatekeeper CI/CD Pipeline

This project demonstrates a **Gatekeeper CI/CD** pattern using GitHub Actions,
enforcing strict conditional execution to prevent wasteful GPU resource allocation.

## Workflow: `pipeline.yaml`

```
push / pull_request
        │
        ▼
  ┌───────────┐   FAIL   ╔══════════════════════════╗
  │  linter   │─────────►║  Pipeline stops here      ║
  └───────────┘          ╚══════════════════════════╝
        │ PASS
        ▼
  Three-way condition check:
    ✔ linter passed
    ✔ branch == main
    ✔ commit message contains [run-train]
        │ ALL TRUE
        ▼
  ┌───────────┐
  │   train   │  (GPU job)
  │           │──failure()──► upload error_logs.txt artifact
  │           │──always() ──► "Cleaning up temporary cloud volumes..."
  └───────────┘
```

## How to trigger training

```bash
git commit -m "feat: update model architecture [run-train]"
git push origin main
```

## How to skip training (linter only)

```bash
git commit -m "docs: update README"
git push origin main        # train job will show as Skipped
```
