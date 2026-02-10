```markdown
# CI/CD + VERIFICATION PIPELINE (HARNESS / AZURE DEVOPS STYLE)

┌──────────────────────────────────────────────────────────────┐
│                        PIPELINE OVERVIEW                      │
└──────────────────────────────────────────────────────────────┘
                 ┌──────────────────────────┐
                 │        CI STAGE          │
                 │     (Build & Push)       │
                 └─────────────┬────────────┘
                               │
                               ▼
                 ┌──────────────────────────┐
                 │        CD STAGE          │
                 │     (Deploy to EKS)      │
                 └─────────────┬────────────┘
                               │
                               ▼
                 ┌──────────────────────────┐
                 │   VERIFICATION STAGE     │
                 │ (Prometheus Metrics Check)│
                 └─────────────┬────────────┘
                               │
                               ▼
                 ┌──────────────────────────┐
                 │   APPROVAL / ROLLBACK    │
                 └──────────────────────────┘

---

# CI Stage (Build & Push)

┌──────────────────────────────────────────────────────────────┐
│                          CI STAGE                             │
└──────────────────────────────────────────────────────────────┘

        ┌──────────────┐
        │ Clone Repo    │
        └───────┬──────┘
                │
        ┌───────▼────────┐
        │ Install Deps    │
        └───────┬────────┘
                │
        ┌───────▼────────┐
        │ Build Docker    │
        │ Image           │
        └───────┬────────┘
                │
        ┌───────▼──────────────────────────────┐
        │ Push Image to ECR                    │
        │ Tag = <+pipeline.sequenceId>         │
        └──────────────────────────────────────┘

---

# CD Stage (Deploy to EKS)

┌──────────────────────────────────────────────────────────────┐
│                          CD STAGE                             │
└──────────────────────────────────────────────────────────────┘

        ┌──────────────┐
        │  Pull Image   │
        │  Tag from CI  │
        └───────┬──────┘
                │
        ┌───────▼──────────────────────────────┐
        │ Apply K8s Manifests                   │
        │ - Deployment                          │
        │ - Service                             │
        │ - Ingress                             │
        └───────┬──────────────────────────────┘
                │
        ┌───────▼──────────────┐
        │ Wait for Rollout      │
        └───────────────────────┘

---

# Verification Stage (Prometheus)

┌──────────────────────────────────────────────────────────────┐
│                     VERIFICATION STAGE                        │
└──────────────────────────────────────────────────────────────┘

        ┌────────────────────────────────────────────┐
        │ Query Prometheus Metrics                   │
        │ - inference_latency_seconds                │
        │ - inference_errors_total                   │
        │ - pred_delay_positive_total                │
        │ - feature_distance_mean                    │
        └───────┬────────────────────────────────────┘
                │
        ┌───────▼──────────────┐
        │ Risk Analysis         │
        │ (Harness CV Engine)   │
        └───────┬──────────────┘
                │
     ┌──────────▼──────────┐
     │ Pass?                │
     └───────┬─────────────┘
             │
   ┌─────────▼──────────┐         ┌───────────────┐
   │   Approve Release   │         │   Rollback     │
   └─────────────────────┘         └───────────────┘

---

# Full Pipeline (All Stages Together)

┌──────────────────────────────────────────────────────────────┐
│                        FULL PIPELINE                          │
└──────────────────────────────────────────────────────────────┘

CI STAGE  
    ↓  
Build Docker Image  
    ↓  
Push to ECR  
    ↓  
CD STAGE  
    ↓  
Deploy to EKS  
    ↓  
Wait for Rollout  
    ↓  
VERIFICATION STAGE  
    ↓  
Prometheus Metrics Check  
    ↓  
Pass → Approve  
Fail → Rollback
```