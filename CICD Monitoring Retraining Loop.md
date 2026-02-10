This shows the lifecycle from **initial model creation → deployment → monitoring → drift → retraining → redeployment → monitoring** again.

---

## End‑to‑End ML + CI/CD + Monitoring + Retraining Loop (with 5 examples per block)

```
┌──────────────────────────────────────────────────────────────────────────────┐
│                           INITIAL MODEL DEVELOPMENT                           │
└──────────────────────────────────────────────────────────────────────────────┘

┌───────────────────────────────┐
│ DATA INGESTION                │
│ - Load flight logs from S3    │
│ - Pull weather API data       │
│ - Import airport metadata     │
│ - Read historical delays CSV  │
│ - Fetch holiday/event data    │
└───────────────────────────────┘

┌───────────────────────────────┐
│ DATA VALIDATION               │
│ - Schema validation           │
│ - Missing value checks        │
│ - Outlier detection           │
│ - Range validation            │
│ - Duplicate row detection     │
└───────────────────────────────┘

┌───────────────────────────────┐
│ FEATURE ENGINEERING           │
│ - Encode airline carrier      │
│ - Extract hour-of-day         │
│ - Normalize distance          │
│ - Weather severity score      │
│ - Holiday/weekend flags       │
└───────────────────────────────┘

┌───────────────────────────────┐
│ MODEL TRAINING                │
│ - Train PyTorch DelayNet      │
│ - Use Adam optimizer          │
│ - Early stopping              │
│ - Class imbalance handling    │
│ - GPU-accelerated training    │
└───────────────────────────────┘

┌───────────────────────────────┐
│ MODEL EVALUATION              │
│ - Accuracy                    │
│ - Recall for “delay” class    │
│ - ROC-AUC                     │
│ - Latency benchmark           │
│ - Robustness tests            │
└───────────────────────────────┘

┌───────────────────────────────┐
│ MODEL REGISTRATION            │
│ - Save model_v1 in registry   │
│ - Store metrics JSON          │
│ - Store training params       │
│ - Store dataset version       │
│ - Add lineage metadata        │
└───────────────────────────────┘

┌───────────────────────────────┐
│ MODEL PACKAGING               │
│ - FastAPI inference service   │
│ - Dockerize model             │
│ - Add /predict endpoint       │
│ - Add /metrics endpoint       │
│ - Tag image: model_v1         │
└───────────────────────────────┘

(Produces image: delay-ml:model_v1)

```

---

## CI/CD DEPLOYMENT PIPELINE

```
┌───────────────────────────────┐
│ CI STAGE                      │
│ - Clone repo                  │
│ - Install dependencies        │
│ - Build Docker image          │
│ - Push to ECR                 │
│ - Export image tag            │
└───────────────────────────────┘

┌───────────────────────────────┐
│ CD STAGE                      │
│ - Apply K8s manifests         │
│ - Update Deployment image     │
│ - Rolling update              │
│ - Wait for pods Ready         │
│ - Update Service/Ingress      │
└───────────────────────────────┘

┌───────────────────────────────┐
│ VERIFICATION STAGE            │
│ - p95 latency check           │
│ - Error rate check            │
│ - Prediction distribution     │
│ - Pod health check            │
│ - Compare to baseline         │
└───────────────────────────────┘

┌───────────────────────────────┐
│ APPROVE / ROLLBACK            │
│ - Auto-approve if healthy     │
│ - Auto-rollback if degraded   │
│ - Notify Slack                │
│ - Store deployment metadata   │
│ - Mark version as “live”      │
└───────────────────────────────┘

```

---

## PRODUCTION MONITORING

```
┌───────────────────────────────┐
│ MODEL MONITORING              │
│ - Drift detection             │
│ - Latency monitoring          │
│ - Error spikes                │
│ - Traffic anomalies           │
│ - Prediction skew             │
└───────────────────────────────┘

(If drift detected → trigger retraining)
```

---

## RETRAINING WORKFLOW (Triggered by Drift)

```
┌───────────────────────────────┐
│ LOAD FRESH DATA               │
│ - Last 7 days S3 data         │
│ - Updated weather feeds       │
│ - New airport changes         │
│ - Seasonal patterns           │
│ - Real-time operational data  │
└───────────────────────────────┘

┌───────────────────────────────┐
│ RECOMPUTE FEATURES            │
│ - Rebuild feature vectors     │
│ - Update weather severity     │
│ - Recalculate time features   │
│ - Normalize with new stats    │
│ - Encode new carriers/routes  │
└───────────────────────────────┘

┌───────────────────────────────┐
│ RETRAIN MODEL                 │
│ - Train DelayNet_v2           │
│ - Hyperparameter tuning       │
│ - Class balancing             │
│ - GPU acceleration            │
│ - Save new weights            │
└───────────────────────────────┘

┌───────────────────────────────┐
│ EVALUATE VS PROD MODEL        │
│ - Compare accuracy            │
│ - Compare recall              │
│ - Compare latency             │
│ - Compare robustness          │
│ - Compare drift resilience    │
└───────────────────────────────┘

(If better → continue, else stop)

┌───────────────────────────────┐
│ REGISTER NEW MODEL            │
│ - Save model_v2               │
│ - Store metrics               │
│ - Store dataset version       │
│ - Store drift reason          │
│ - Store lineage metadata      │
└───────────────────────────────┘

┌───────────────────────────────┐
│ PACKAGE NEW MODEL             │
│ - Update FastAPI weights      │
│ - Build new Docker image      │
│ - Tag: model_v2               │
│ - Push to ECR                 │
│ - Emit new image tag          │
└───────────────────────────────┘

```

---

## CI/CD DEPLOYMENT (AGAIN)

The same CI/CD pipeline deploys the new model_v2, verifies it, and promotes it to production.

---

## BACK TO MONITORING

The loop continues:

Monitor → Drift → Retrain → Deploy → Verify → Monitor

