# 📘 Documentation & Diagrams — Architecture & Knowledge Base

> **Purpose:** Centralized documentation hub for all system components — models, APIs, architecture diagrams, and engineering knowledge base.  
> **Audience:** Engineers, MLOps, DevOps, Researchers, and Technical Writers.  
> **Scope:** Maintains versioned documentation for models, datasets, APIs, and deployment architectures.

---

## 🧠 Model Documentation

Each model has a dedicated card documenting architecture, training setup, datasets, metrics, dependencies, and deployment state.

| Model Name | Architecture | Dataset | Key Metrics | Deployment | Docs Location | Notes |
|-------------|---------------|----------|--------------|-------------|----------------|--------|
| **LLM-Core v2.1** | Transformer (13B) — LoRA fine-tuned | MixCorpus v5 | Accuracy 87.2%, BLEU 86.4 | 🟢 Production | `/docs/models/llm-core.md` | Main generative model used in RAG & Chatbot |
| **TTS-Neo v1.5** | FastSpeech2 + HiFi-GAN | VoxSynth | MOS 4.5, WER 3.4% | 🟢 Production | `/docs/models/tts-neo.md` | Multilingual TTS voice generator |
| **Avatar-Vision v3.0** | CLIP + Diffusion Fusion | FaceData v2 | Vision Accuracy 92%, FPS 30 | 🟡 Beta | `/docs/models/avatar-vision.md` | 3D avatar perception + synthesis |
| **RAG-Indexer v1.2** | BERT + FAISS | DocEmbed v3 | Recall@5 = 93% | 🟢 Production | `/docs/models/rag-indexer.md` | Text retrieval + knowledge search |
| **Speech-Cleanser v0.9** | Conv-Autoencoder | VoxSynth Raw | +12 dB SNR | 🧪 Testing | `/docs/models/speech-cleanser.md` | Audio denoising before TTS/STT |

Each model doc includes:
- **Overview:** Purpose and architecture summary.  
- **Training Details:** Framework, hyperparameters, and dataset lineage.  
- **Evaluation Metrics:** Reported KPIs and comparison to baselines.  
- **Deployment Info:** Current environment (Prod/Beta/Test) and version history.  
- **Dependencies:** Datasets, tools, and runtime stack.  
- **Change Log:** Updates and performance notes per version.  

---

## 🏗️ System Architecture

| Component | Description | Diagram | Notes |
|------------|--------------|----------|--------|
| **Overall System View (C4)** | High-level overview showing data flow between user → API Gateway → AI Services → Databases → Monitoring | `/docs/architecture/overview.png` | Updated with ECS & private subnet mapping |
| **Service Mesh** | Internal network of ECS services communicating via Cloud Map | `/docs/architecture/service_mesh.png` | Includes DNS-based service discovery |
| **Model Lifecycle** | Pipeline from dataset ingestion → training → evaluation → deployment | `/docs/architecture/model_lifecycle.png` | Synced with MLflow experiments |
| **Infra Layer** | Cloud + On-prem hybrid setup | `/docs/architecture/infra_stack.png` | VPC, ALB, ECS, S3, Jenkins, MLflow |
| **Monitoring Stack** | Metrics collection and alerting setup | `/docs/architecture/monitoring_stack.png` | Prometheus + Grafana + LangFuse |

---

## 📚 Knowledge Base & ADRs (Architecture Decision Records)

| Section | Description | Path |
|----------|--------------|------|
| **Knowledge Base** | Engineering best practices, FAQs, and tutorials | `/docs/wiki/` |
| **ADRs (Design Decisions)** | Architecture Decision Records — each major technical choice | `/docs/adr/` |
| **Runbooks** | Step-by-step resolution guides for incidents | `/docs/runbooks/` |
| **Infra Playbooks** | Operational playbooks for deployment, scaling, rollback | `/docs/playbooks/` |
| **Data Contracts** | Schema + interface contracts between services | `/docs/contracts/` |

---

# 🌐 API & Integration Tracker — Auth & Connectivity

> **Purpose:** Centralized reference for all internal and external service APIs, authentication mechanisms, performance SLAs, and documentation links.  
> **Audience:** Backend engineers, API consumers, integrators, QA, and DevOps.  
> **Scope:** Includes REST, gRPC, and WebSocket endpoints across the AI platform.

---

## 🧩 API Overview

| API Service | Type | Auth Method | Endpoint | Avg Latency (P95) | Uptime | Docs | Integration Notes |
|--------------|------|-------------|-----------|--------------------|----------|------|----------------|
| **Chatbot API** | REST | JWT | `/api/v1/chatbot` | 50 ms | 99.99% | [chatbot.md](../api/chatbot.md) | Core conversational interface for web + chat clients |
| **LLM Service (Core)** | gRPC | OAuth2 | `grpc://llm.service:9000` | 40 ms | 99.9% | [llm.md](../api/llm.md) | Main generative model; accessed via Ocelot gateway |
| **TTS API** | REST | API Key | `/api/v1/tts` | 60 ms | 99.8% | [tts.md](../api/tts.md) | Speech synthesis with multilingual support |
| **STT API** | REST | API Key | `/api/v1/stt` | 70 ms | 99.7% | [stt.md](../api/stt.md) | Converts audio input → text with diarization |
| **Avatar Stream** | WebSocket | JWT | `/ws/avatar` | 75 ms | 99.6% | [avatar.md](../api/avatar.md) | Bidirectional video/audio stream for 3D avatars |
| **RAG Search API** | REST | OAuth2 | `/api/v1/rag` | 45 ms | 99.9% | [rag.md](../api/rag.md) | Knowledge retrieval service for internal docs |
| **Metrics API** | REST | HMAC | `/api/v1/metrics` | 20 ms | 99.99% | [metrics.md](../api/metrics.md) | Internal metrics ingestion (Grafana + Prometheus) |

---

## 🔐 Authentication Reference

| Method | Used By | Description | Token Lifetime | Notes |
|---------|----------|--------------|----------------|--------|
| **JWT** | Chatbot, Avatar, WebSocket APIs | JSON Web Tokens for per-session user auth | 60 min | Issued via central Identity Service |
| **OAuth2** | LLM, RAG Services | Standard client credential flow | 24 hr | For internal service-to-service auth |
| **API Key** | TTS/STT Services | Static token for programmatic clients | Unlimited | Rotated monthly for external apps |
| **HMAC-Signed Requests** | Metrics + Internal APIs | Signature-based internal API validation | 10 min | Used by telemetry agents only |

---

## 📈 Performance & Observability

| Metric | Source | Tool | SLA / Threshold | Alert | Dashboard |
|---------|---------|------|----------------|--------|------------|
| **API Latency (P95)** | LangFuse + APM | Grafana | <120 ms | ⚠️ Slack #alerts | [Latency Board](grafana/latency) |
| **Error Rate (5xx)** | Prometheus | Grafana | <0.5% | 🟥 PagerDuty | [API Health](grafana/api-health) |
| **Throughput (req/s)** | API Gateway Logs | CloudWatch | Monitored hourly | 🟢 | [Traffic View](grafana/throughput) |
| **Auth Failures** | Identity Logs | CloudWatch | <1% | ⚠️ Email Ops | [Auth Board](grafana/auth) |

---

## 🔄 Integration & Dependencies

| Integration | Type | Connected Services | Status | Notes |
|--------------|------|--------------------|----------|--------|
| **LangFuse** | Observability | Chatbot, LLM-Core | ✅ Active | Traces + latency analytics |
| **MLflow** | Model Registry | LLM-Core, TTS-Neo, RAG-Indexer | ✅ Active | Experiment tracking + deployment metadata |
| **Prometheus → Grafana** | Metrics | All APIs | ✅ Active | Unified metrics dashboard |
| **Ocelot API Gateway** | Routing | External → Internal services | 🟢 Healthy | Handles path-based routing and auth |
| **ECS Service Discovery (Cloud Map)** | DNS | Internal ECS Tasks | 🟢 Healthy | Enables service-to-service calls via DNS name |


# 🧠 AI Company System Management & Tracking Console  

> **Purpose:** Centralized operations and governance console to monitor, manage, and align all AI systems — models, datasets, pipelines, infrastructure, APIs, and products — across the organization.  
> **Audience:** Engineering, MLOps, DevOps, Data Science, Research, and Product teams.  
> **Scope:** From dataset ingestion → model lifecycle → deployment → monitoring → cost → incidents → documentation.  

---

## Table of Contents  
1. [Executive Console](#1-executive-console--organization-health--summary)  
2. [Product & Service Tracker](#2-product--service-tracker--ai-products--endpoints)  
3. [Model Management](#3-model-management--model-registry--deployment-tracking)  
4. [Dataset Registry](#4-dataset-registry--data-lineage--access-governance)  
5. [Pipeline Tracker](#5-pipeline-tracker--workflow--execution-stages)  
6. [Infra & DevOps Tracker](#6-infra--devops-tracker--compute--network--storage)  
7. [API & Integration Tracker](#7-api--integration-tracker--auth--connectivity)  
8. [Monitoring & Analytics](#8-monitoring--analytics--metrics--alerts--dashboards)  
9. [Incident & Runbook Management](#9-incident--runbook-management--response--resolution)  
10. [Documentation & Diagrams](#10-documentation--diagrams--architecture--knowledge-base)  
11. [Dashboard Integrations](#11-dashboard-integrations--tools--data-links)  
12. [Project Templates](#12-project-templates--reusable-structures--pipelines)  

---

## 1. Executive Console — Organization Health & Summary  

| Category | Description | Key KPIs | Linked Views |
|-----------|--------------|----------|---------------|
| **System Health** | Aggregate uptime, latency, API reliability across all AI services. | ✅ 99.95% uptime, ⚙️ Avg latency 48 ms | [Monitoring Dashboard](#8-monitoring--analytics--metrics--alerts--dashboards) |
| **Model Quality KPIs** | Track overall ML performance (BLEU, WER, accuracy). | 🎯 92.4 avg model accuracy | [Model Management](#3-model-management--model-registry--deployment-tracking) |
| **Product Status Summary** | Deployment and operational readiness by product line. | 🟢 Active: 4   🟡 Testing: 2   🔴 Offline: 1 | [Product Tracker](#2-product--service-tracker--ai-products--endpoints) |
| **Infra & Cost Overview** | Real-time compute usage, cloud costs, GPU utilization. | 💰 $14.2 / hr GPU avg, 🧾 Monthly $8.9k spend | [Infra Tracker](#6-infra--devops-tracker--compute--network--storage) |
| **Alert Summary** | Critical & high-severity incidents within last 7 days. | 🔴 2 open  🟡 1 under investigation | [Incident Center](#9-incident--runbook-management--response--resolution) |

---

## 2. Product & Service Tracker — AI Products & Endpoints  

| Product | Category | Lifecycle Stage | Core Metrics | API Endpoint | Owner | Dependencies | Notes |
|----------|-----------|----------------|---------------|---------------|--------|---------------|--------|
| **LLM-Core** | Language Model | 🟢 Production | Accuracy 87.2%, latency 40 ms | `/api/v1/llm` | AI Platform | MixCorpus v5 / PyTorch v2.3 | Foundation model for chat & RAG |
| **Chatbot-Pro** | Conversational Agent | 🟢 Production | CSAT 92%, latency 65 ms | `/api/v1/chatbot` | Product AI | LLM-Core / LangFuse | Multi-channel (web, Slack, API) |
| **Avatar-Vision** | Multimodal 3D Agent | 🟡 Beta Testing | FPS 30, Audio-Sync 98% | `/api/v1/avatar` | XR Team | CLIP + Diffusion / TTS-Neo | Face-to-speech fusion prototype |
| **TTS-Neo** | Speech Synthesis | 🟢 Production | WER 3.4%, MOS 4.5 | `/api/v1/tts` | Speech Team | VoxSynth dataset | 20+ languages, voice cloning |
| **RAG-Search** | Retrieval-Augmented QA | 🟢 Stable | Top-5 Recall 93% | `/api/v1/rag` | Knowledge AI | FAISS index / LLM-Core | Internal doc assistant |

---

## 3. Model Management — Model Registry & Deployment Tracking  

| Model | Architecture | Version | Dataset | Training Date | Eval Score | Deployment | Infra | Notes |
|--------|---------------|----------|----------|----------------|--------------|-------------|--------|--------|
| **LLM-Core** | Transformer 13B (PeFT + LoRA) | v2.1 | MixCorpus v5 | 2025-09-10 | 87.2 BLEU | 🟢 Prod | A100x8 AWS | Optimized chat alignment |
| **TTS-Neo** | FastSpeech2 + HiFi-GAN | v1.5 | VoxSynth | 2025-07-01 | MOS 4.5 / WER 3.4 | 🟢 Prod | RTX A40 Local | Fine-tuned multi-lang |
| **Avatar-Vision** | CLIP + Diffusion | v3.0 | FaceData v2 | 2025-08-12 | 92% Accuracy | 🟡 Beta | Mixed GPU Cluster | Latent fusion model |
| **RAG-Indexer** | BERT + FAISS | v1.2 | DocEmbed v3 | 2025-05-20 | R@5 93% | 🟢 Prod | EC2 m6i | Embedding retriever |
| **Speech-Cleanser** | Conv-Autoencoder | v0.9 | VoxSynth raw | 2025-04-22 | SNR +12 dB | 🧪 Test | Local RTX 3090 | Noise reduction preproc |

---

## 4. Dataset Registry — Data Lineage & Access Governance  

| Dataset | Origin | Size | Quality | License | Access Level | Used By | Data Ops Notes |
|----------|---------|------|----------|-----------|---------------|----------|----------------|
| **MixCorpus v5** | Internal + Public (WebText, CC) | 4 TB | ✅ High | CC-BY-4.0 | Restricted | LLM-Core | Cleaned, deduped, toxicity-filtered |
| **VoxSynth** | Internal speech recordings | 900 GB | 🟢 Excellent | Proprietary | Limited | TTS-Neo | Multi-accent voices + SSML tags |
| **FaceData v2** | Human video dataset | 1.2 TB | 🟡 Medium | Consent-based | Private | Avatar-Vision | Anonymized, faces blurred |
| **DocEmbed v3** | Enterprise text docs | 600 GB | ✅ High | Internal | Confidential | RAG-Indexer | Indexed via FAISS RAG |
| **SensorStream** | Real-time IoT feed | Continuous | 🟢 Stable | Internal | Read-only | Predictive Models | Data contract v2.0 compliant |

---

## 5. Pipeline Tracker — Workflow & Execution Stages  

| Stage | Function | Toolchain | Frequency | Last Run | Metrics | Owner | Observations |
|--------|-----------|------------|------------|-----------|----------|--------|---------------|
| **Data Preprocessing** | Cleaning, tokenization, augmentation | Pandas, Spark, Airflow | Daily | 2025-10-06 | 4.2 TB → 3.8 TB | DataOps | Schema validated + logged |
| **Feature Engineering** | Embeddings + feature stores | Feast, NumPy | Weekly | 2025-10-05 | +2% accuracy gain | ML Team | Synced with registry |
| **Training** | Model fine-tune / distillation | PyTorch, DeepSpeed | As needed | 2025-10-02 | GPU 90% util | ML Team | Auto-resumed on failure |
| **Evaluation** | QA + benchmarking | EvalSuite, W&B | After train | 2025-10-03 | BLEU 87, MOS 4.5 | QA | Drift monitor enabled |
| **Deployment** | Container rollout / A-B test | MLflow, Helm, ECS | On release | 2025-10-04 | Latency OK | DevOps | Canary + rollback ready |

---

## 6. Infra & DevOps Tracker — Compute, Network & Storage  

| Resource | Type | Provider | Cost/hr | Status | Scaling | Region | Notes |
|-----------|------|-----------|----------|----------|----------|----------|--------|
| **GPU Cluster** | 8× A100 | AWS ECS | $12/hr | 🟢 Active | Auto-Scale (2–10 nodes) | eu-west-1 | For training + vision |
| **Inference Nodes** | RTX A40 | On-Prem | $2/hr | 🟢 Active | Manual scale | DC-01 | Chatbot & TTS |
| **Storage** | S3 / Ceph Hybrid | AWS + Local | $0.02/GB | 🟢 | Elastic | Multi-AZ | Datasets + logs |
| **Networking** | VPC + Ocelot API Gateway | AWS ECS | — | 🟢 | Load-balanced | Private subnets | ECS service discovery enabled |
| **CI/CD Pipeline** | Jenkins + GitHub Actions | Hybrid | — | 🟢 | On-commit | — | Builds, tests, ECS deployments |

---

## 7. API & Integration Tracker — Auth & Connectivity  

| Service | Type | Auth Method | Endpoint | Latency (P95) | Uptime | Documentation | Integration Notes |
|----------|------|-------------|-----------|----------------|----------|----------------|----------------|
| **Chatbot API** | REST | JWT | `/api/v1/chatbot` | 50 ms | 99.99% | [Spec](api/chatbot.md) | Serves multi-tenant clients |
| **TTS API** | REST | API Key | `/api/v1/tts` | 60 ms | 99.8% | [Docs](api/tts.md) | Caches voices per user |
| **LLM Service** | gRPC | OAuth2 | `grpc://llm.service:9000` | 40 ms | 99.9% | [Proto](api/llm.md) | Load-balanced via ECS |
| **Avatar Stream** | WebSocket | JWT | `/ws/avatar` | 75 ms | 99.6% | [Stream Docs](api/avatar.md) | Realtime bi-directional audio/video |
| **Internal Telemetry** | REST | HMAC | `/api/v1/metrics` | 20 ms | 99.99% | [Spec](api/metrics.md) | Exposed to Grafana + Prometheus |

---

## 8. Monitoring & Analytics — Metrics, Alerts & Dashboards  

| Metric | Source | Tool | Threshold | Alert Target | Dashboard |
|---------|---------|------|------------|---------------|------------|
| **GPU Utilization** | Node Exporter | Grafana + Prometheus | > 90% | ✅ PagerDuty | [GPU Dashboard](grafana/gpu) |
| **API Latency** | LangFuse + APM | Grafana / Elastic | > 120 ms | ⚠️ Slack #alerts | [Latency Board](grafana/latency) |
| **Model Drift** | EvalTracker / W&B | Weights & Biases | > 5% score drop | ⚠️ Email Ops | [Model Perf](wandb/models) |
| **Infra Cost** | AWS Billing API | CloudWatch | > $10k / mo | 🟥 Exec Channel | [Finance](grafana/costs) |
| **Disk Utilization** | Node Metrics | Prometheus | > 80% | 🟠 SysAdmin | [Infra Board](grafana/storage) |

---

## 9. Incident & Runbook Management — Response & Resolution  

| Incident ID | System | Root Cause | Severity | Status | Owner | Runbook | Resolution Notes |
|--------------|---------|-------------|-----------|----------|----------|----------|----------------|
| **INC-2025-09-20** | LLM-Core | Memory leak in fine-tune loop | 🔴 Critical | 🟢 Resolved | MLOps | [Runbook](runbooks/llm-leak.md) | Fixed with grad checkpointing |
| **INC-2025-09-29** | Avatar-Vision | GPU overload / OOM | 🟠 High | 🟡 Mitigated | Infra | [Runbook](runbooks/gpu-overload.md) | Split batches & limited fps |
| **INC-2025-10-04** | TTS API | Timeout at 95th percentile | 🟡 Medium | 🔴 Active | API Team | [Runbook](runbooks/tts-latency.md) | Load balancer investigation ongoing |
| **INC-2025-10-05** | Cost Spike | Unused GPU instances | 🟡 Medium | 🟢 Closed | DevOps | [Runbook](runbooks/cost-optimization.md) | Added auto-stop policy |

---

## 10. Documentation & Diagrams — Architecture & Knowledge Base  

| Section | Type | Description | Location |
|----------|------|--------------|-----------|
| **System Architecture** | Diagram | C4 Model showing ECS, API Gateway, Services & Databases | `/docs/architecture/overview.png` |
| **Model Cards** | Docs | Architecture, hyperparams, datasets, metrics per model | `/docs/models/` |
| **ADRs (Design Records)** | Markdown | Documented engineering decisions with rationale | `/docs/adr/` |
| **Knowledge Base** | Wiki | How-tos, FAQ, tool setup, best practices | `/docs/wiki/` |
| **Infra Runbooks** | Guides | Standard recovery & restart procedures | `/docs/runbooks/` |

---

## 11. Dashboard Integrations — Tools & Data Links  

| Tool | Purpose | Integration Status | Owner | Link |
|------|----------|--------------------|--------|------|
| **MLflow** | Model registry + tracking | ✅ Connected | MLOps | [MLflow UI](http://mlflow.local) |
| **Grafana** | Infra metrics & alerts | ✅ Connected | DevOps | [Grafana](http://grafana.local) |
| **Prometheus** | Metric collection backend | ✅ Connected | DevOps | [Prometheus](http://prometheus.local) |
| **Weights & Biases** | Experiment tracking & drift | 🟢 Active | ML Team | [W&B Project](https://wandb.ai/org/project) |
| **LangFuse** | LLM tracing / latency logs | 🟢 Active | Platform AI | [LangFuse UI](https://langfuse.com) |
| **CloudWatch** | AWS infra monitoring & cost alerts | 🟢 Enabled | Cloud Ops | [CloudWatch Dash](https://console.aws.amazon.com/cloudwatch) |

---

## 12. Project Templates — Reusable Structures & Pipelines  

| Project Type | Workflow Includes | Folder Path | Template Link | Ideal Use |
|---------------|------------------|---------------|----------------|-------------|
| **LLM Project** | Data → Train → Eval → Deploy → Monitor | `/templates/llm_project/` | [LLM Template](templates/llm_project.md) | Core chat or QA models |
| **TTS/STT Project** | Audio prep → Model train → API serve | `/templates/tts_stt/` | [TTS Template](templates/tts_stt.md) | Voice synthesis / ASR |
| **Avatar Project** | Vision + Speech → Fusion → Realtime Service | `/templates/avatar/` | [Avatar Template](templates/avatar.md) | Multimodal agents |
| **RAG Project** | Retriever → Indexer → LLM query → Cache | `/templates/rag/` | [RAG Template](templates/rag.md) | Knowledge search bots |
| **Analytics Project** | Data ETL → Metrics → Grafana integration | `/templates/analytics/` | [Analytics Template](templates/analytics.md) | System insights & dashboards |

---

> **Maintainer:** AI Platform Engineering  
> **Last Updated:** 2025-10-08  
> **Version:** 1.2  
ystem Design Summary (Final Confirmation Before Generation)

Your Notion import will include:

1. Executive Console

Global KPIs (inference latency, uptime, cost per token, etc.)

Linked views of incidents, deployments, and infra usage

Summary rollups from all sub-databases

2. Product & Service Registry

Each AI product (Chatbot, LLM, TTS, STT, RAG, Avatar, Vision, API gateway)

Links: Models ↔ Pipelines ↔ Infra ↔ APIs ↔ Dashboards

Columns: Owner, SLA, Current version, Dependencies, Active models, Latency budget, Cost cap

3. Model Registry (Central Database)

Architecture (LLM, Diffusion, TTS encoder, etc.)

Parameters, model size, framework, CUDA version

Training logs, datasets, checkpoints, MLflow / W&B URLs

Relations to Dataset Registry, Infra nodes, and Deployments

Rollups for latency, token/sec, GPU memory, accuracy, loss curves

Fine-tune lineage + rollback tracking

4. Dataset Registry

Data sources, labeling pipeline, versioning, schema

RAG chunking params, embeddings index (Weaviate/Pinecone IDs)

S3 paths, retention policies, compliance tags

Relations to Models (used_in), Pipelines (preprocess/training/eval)

5. Pipeline Tracker

DAG stages (data → preprocess → train → eval → deploy)

Toolchain versions (PyTorch, CUDA, Ray, DVC)

Start/end time, artifact paths, container tags

Relation to Models and Infra

Rollup metrics: total GPU hours, cost per run, success/failure rate

6. Infra & DevOps Tracker

Node inventory (GPUs, CPUs, storage, memory)

Cluster tags (prod/staging/dev)

Orchestration (Kubernetes namespace, pod name, scaling rules)

Metrics: GPU utilization, thermals, CUDA mem usage, network throughput

Monitoring URLs (Prometheus, Grafana, Loki)

Relation to Deployments and Models

7. API & Integration Registry

REST/gRPC endpoints, auth method, load balancer target

Endpoint latency, request volume, error rate

Linked to Product, Model, Infra

SLO/SLA compliance dashboard URLs

8. Monitoring & Analytics

Metrics registry (Prometheus keys, Grafana panels, W&B run URLs)

Latency histograms, throughput, token/sec, GPU mem by container

Cost tracking (per GPU hour / per request)

Logs: Loki/Elastic indexes, alert rules

9. Incident & Runbook Management

Incident ID, severity, affected services, timeline

Root cause, impact metrics, mitigation, follow-up tasks

Linked runbooks and on-call rotation

Rollups: MTTR, recurrence frequency

10. Documentation & Diagrams

Architecture decision records (ADRs)

System diagrams (embed draw.io / Mermaid links)

Service dependency graphs (export from Neo4j schema)

Versioned design docs

11. Dashboard Integrations

Grafana panel embeds

MLflow model registry / experiment tracking URLs

W&B projects, Prometheus dashboards, K8s metrics

Pinecone/Weaviate vector index dashboards

12. Project Templates (Pre-Linked)

LLM Project

Models: base + fine-tune

Datasets: corpus, eval, prompt logs

API: /generate, /chat, /embedding

Metrics: token/sec, latency, accuracy, hallucination rate

TTS/STT Project

Models: acoustic, vocoder

Metrics: MOS, latency, audio length ratio

Infra: GPU audio queue utilization

Avatar Project

Real-time pipeline tracking (vision + audio + animation)

Frame latency, sync error, bandwidth

RAG Project

Chunking config, retriever latency, context hit ratio
---

