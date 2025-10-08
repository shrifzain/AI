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

---

## 🧩 API Documentation Folder Structure

```bash
/docs/api/
├── chatbot.md
├── llm.md
├── tts.md
├── stt.md
├── rag.md
├── avatar.md
└── metrics.md
