┌──────────────────────────────────────────────────────────────────────────────┐
│                               DATA & ML PIPELINE                             │
└──────────────────────────────────────────────────────────────────────────────┘

┌───────────────────────────────┐
│        DATA INGESTION         │
│  Example: Load flight logs    │
│  from S3 + weather from API   │
└───────────────┬───────────────┘
                ▼
┌───────────────────────────────┐
│        DATA VALIDATION        │
│  Example: Check schema,       │
│  missing values, anomalies    │
└───────────────┬───────────────┘
                ▼
┌───────────────────────────────┐
│     FEATURE ENGINEERING       │
│  Example: Encode carrier,     │
│  scale distance, extract hour │
└───────────────┬───────────────┘
                ▼
┌───────────────────────────────┐
│        MODEL TRAINING         │
│  Example: Train PyTorch       │
│  DelayNet on engineered data  │
└───────────────┬───────────────┘
                ▼
┌───────────────────────────────┐
│       MODEL EVALUATION        │
│  Example: Accuracy, recall,   │
│  fairness, robustness checks  │
└───────────────┬───────────────┘
                ▼
┌───────────────────────────────┐
│     MODEL REGISTRATION        │
│  Example: Store model v1.3    │
│  in MLflow Model Registry     │
└───────────────┬───────────────┘
                ▼
┌───────────────────────────────┐
│       MODEL PACKAGING         │
│  Example: FastAPI inference   │
│  service + Docker container   │
└───────────────┬───────────────┘
                │
                ▼
┌──────────────────────────────────────────────────────────────────────────────┐
│                               HARNESS/AzureDevops CI/CD PIPELINE             │
└──────────────────────────────────────────────────────────────────────────────┘

┌───────────────────────────────┐      ┌───────────────────────────────┐
│            CI STAGE           │      │            CD STAGE           │
│     Build & Push to ECR       │      │        Deploy to EKS          │
│  Example: docker build/push   │      │  Example: Rolling update      │
└───────────────┬───────────────┘      └───────────────┬───────────────┘
                │                                        │
                ▼                                        ▼
   ┌──────────────────────────┐             ┌──────────────────────────┐
   │ Clone Repository         │             │ Apply K8s Manifests      │
   │ Example: GitHub main     │             │ Example: deployment.yaml │
   ├──────────────────────────┤             ├──────────────────────────┤
   │ Install Dependencies     │             │ Rolling Deployment       │
   │ Example: pip install     │             │ Example: update pods     │
   ├──────────────────────────┤             ├──────────────────────────┤
   │ Build Docker Image       │             │ Wait for Rollout         │
   │ Example: delay-ml:tag    │             │ Example: pods Ready      │
   ├──────────────────────────┤             └───────────────┬──────────┘
   │ Push Image to ECR        │                             │
   │ Example: tag=v23         │                             ▼
   └───────────────┬──────────┘             ┌──────────────────────────┐
                   │                        │     VERIFICATION STAGE   │
                   ▼                        │ Prometheus Metrics Check │
   (Output: image tag → CD)                 │ Example: latency < 500ms │
                                            └───────────────┬──────────┘
                                                            │
                                                            ▼
                                            ┌──────────────────────────┐
                                            │     Approve / Rollback   │
                                            │ Example: auto-rollback   │
                                            └───────────────┬──────────┘
                                                            │
                                                            ▼
┌──────────────────────────────────────────────────────────────────────────────┐
│                               PRODUCTION MONITORING                           │
└──────────────────────────────────────────────────────────────────────────────┘

┌───────────────────────────────┐
│       MODEL MONITORING        │
│  Example: Drift detection,    │
│  latency, error spikes        │
└───────────────┬───────────────┘
                ▼
┌───────────────────────────────┐
│       MODEL RETRAINING        │
│  Example: Trigger new model   │
│  training when drift > 20%    │
└───────────────┬───────────────┘
                ▼
(Back to Training → Evaluation → Registration → Packaging → CI/CD)
