---
layout: about
title: About me
permalink: /about/
---

# From Full-Stack Developer to AI Engineer

## Video Intro

<iframe width="100%" height="400" src="https://www.youtube.com/embed/_gvWV7GGmH4?si=yxVVyzZo1sJsw73e" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

# Greetings!

I am **Ali Mehdi**, an **AI Engineer** based in Lahore. I design and ship production AI systems—retrieval-augmented generation (RAG), LLM-powered APIs, agent workflows, and the MLOps infrastructure that keeps models reliable in production.

My path did not start in a research lab. I spent years as a **full-stack developer** and **cloud/DevOps engineer**, which means I care about the whole stack: data pipelines, inference latency, cost controls, observability, and secure deployments—not just notebook prototypes.

---

## What I Work On Today

- **LLM applications**: chatbots, document Q&A, summarization, and internal copilots using **OpenAI**, **Anthropic**, and open models via **Hugging Face**
- **RAG systems**: chunking strategies, embedding models, vector search, reranking, and evaluation with **LangChain** / **LlamaIndex**
- **Agents & orchestration**: tool-calling flows, structured outputs, and guardrails for business workflows
- **MLOps & deployment**: containerized inference on **Kubernetes (EKS)**, **SageMaker**, **FastAPI** services, CI/CD for models, and experiment tracking with **MLflow**
- **AI observability**: tracing prompts, logging token usage, monitoring drift, and debugging bad retrievals with **LangSmith**, **Phoenix (Arize)**, and custom metrics in **Grafana**

---

## Tools & Technologies

### LLMs & Frameworks
OpenAI API (GPT-4o, embeddings) · Anthropic Claude · Hugging Face Transformers · LangChain · LlamaIndex · vLLM · Ollama (local dev)

### Data & Vector Stores
Pinecone · Chroma · Weaviate · pgvector (PostgreSQL) · Amazon OpenSearch · S3 data lakes

### ML & Experimentation
Python · PyTorch · scikit-learn · Pandas · Jupyter · MLflow · Weights & Biases · SageMaker Training & Endpoints

### Application Layer
FastAPI · Flask · Django · React · REST & WebSocket APIs · Redis (caching) · Celery (async jobs)

### Cloud & MLOps
AWS (SageMaker, Bedrock, Lambda, S3, ECR) · Docker · Kubernetes · Terraform · GitHub Actions · EKS · GPU node pools

### Quality & Safety
Prompt versioning · RAG evaluation (Ragas, custom golden sets) · PII redaction · rate limiting · content filters · SSRF-safe fetch patterns for RAG crawlers

---

## My Journey

### **2013–2017: Full-Stack Foundations**
Started as a **WordPress and PHP developer**, then moved into **Laravel**, **MongoDB**, and **JavaScript** APIs. This phase taught me how real users interact with software—skills I still use when building AI product UIs and backend integrations.

### **2017–2020: Cloud & Scalable Backends**
Deepened work in the **AWS ecosystem** (EC2, RDS, S3, CloudFormation) and earned **AWS Developer Associate (2017)**. Learned to design systems that scale—essential before putting LLM workloads behind production traffic.

### **2020–2023: Platform Engineering & Kubernetes**
Led **Kubernetes** deployments, **Terraform** IaC, and observability stacks (**Prometheus**, **Grafana**, **OpenTelemetry**). Earned **CKAD (2021)** and **CKA**. This became the foundation for **MLOps**: serving models, autoscaling inference, secrets management, and secure cluster hardening.

### **2023–Present: AI Engineering**
Shifted focus to **generative AI in production**:
- Built **RAG pipelines** over internal PDFs and wikis with hybrid search and citation-backed answers
- Shipped **FastAPI + OpenAI** microservices with streaming responses and Redis-backed session memory
- Fine-tuned and evaluated smaller models on domain data; tracked runs in **MLflow**
- Deployed inference on **EKS** with horizontal pod autoscaling and GPU nodes where needed
- Wrote about **foundation models**, **Kubernetes security**, and **scheduled scaling** on this blog

---

## Selected Projects

| Project | Stack | Outcome |
|---------|-------|---------|
| Enterprise document assistant | LangChain, OpenAI embeddings, Pinecone, FastAPI, React | Reduced support lookup time with cited answers from 10k+ internal docs |
| MLOps on EKS | Kubernetes, MLflow, S3, GitHub Actions | Repeatable model deploys with versioned artifacts and rollback |
| LLM API gateway | FastAPI, OpenAI, Redis, Prometheus | Centralized API keys, rate limits, and cost dashboards per team |
| Fine-tune & evaluate (domain QA) | SageMaker, Hugging Face, MLflow | Improved accuracy on niche terminology vs. zero-shot GPT |

---

## Certifications & Continuous Learning

- AWS Developer Associate
- Certified Kubernetes Application Developer (CKAD)
- Certified Kubernetes Administrator (CKA)
- Ongoing: advanced RAG evaluation, agent design patterns, and efficient inference (quantization, batching)

---

## How I Approach AI Work

1. **Start with the user task**, not the model name—define success metrics before picking GPT vs. open-weights.
2. **Prototype with APIs**, then optimize cost/latency with caching, smaller models, or self-hosted inference.
3. **Measure retrieval quality**—most production failures are bad chunks or missing metadata, not weak LLMs.
4. **Treat prompts and schemas as code**—version them, review in PRs, and test regressions.
5. **Deploy like any critical service**—health checks, autoscaling, secrets, and incident runbooks.

---

## Let's Connect

I share lessons on building AI systems that survive real traffic—not just demos. If you're working on RAG, agents, or MLOps, reach out via [email](mailto:alimehdi.official@gmail.com) or [GitHub](https://github.com/yesidevelop).

**The journey continues—always learning, always shipping.** 🚀
