---
layout: about
title: About me
permalink: /about/
---

<div class="about-page">

  <header class="about-intro" data-aos="fade-up">
    <span class="about-intro-label">About</span>
    <h1>From code to cloud to AI</h1>
    <p class="about-intro-lead">
      I'm <strong>Ali Mehdi</strong>, an <strong>AI Engineer</strong> in {{ site.location }}.
      I design production LLM systems—RAG, agents, APIs—and the MLOps stack that keeps them reliable under real traffic.
    </p>
    <p class="about-intro-sub">
      My career moved from full-stack web development → cloud &amp; Kubernetes platform work → generative AI in production—including <strong>ComfyUI</strong> pipelines teams use for real marketing deliverables.
      Scroll the timeline below to see how each phase built the next.
    </p>
  </header>

  <section class="experience-timeline" aria-label="Career timeline">

    <div class="timeline-track" aria-hidden="true"></div>

    <article class="timeline-item" data-aos="fade-up">
      <div class="timeline-node">
        <span class="timeline-dot"></span>
        <time class="timeline-year" datetime="2013">2013</time>
      </div>
      <div class="timeline-card">
        <span class="timeline-phase">The beginning</span>
        <h3>WordPress &amp; PHP Developer</h3>
        <p>Started my career building client websites and custom themes. Learned how to ship for real users—performance, plugins, and maintainable PHP.</p>
        <ul class="timeline-highlights">
          <li>WordPress, jQuery, JavaScript front ends</li>
          <li>PHP backends and theme customization</li>
        </ul>
        <div class="timeline-tags">
          <span>WordPress</span><span>PHP</span><span>jQuery</span>
        </div>
      </div>
    </article>

    <article class="timeline-item" data-aos="fade-up" data-aos-delay="50">
      <div class="timeline-node">
        <span class="timeline-dot"></span>
        <time class="timeline-year" datetime="2014">2014–17</time>
      </div>
      <div class="timeline-card">
        <span class="timeline-phase">Expanding stack</span>
        <h3>Full-Stack Web Developer</h3>
        <p>Moved beyond CMS work into APIs and modern frameworks. Collaborated with international clients on web products end to end.</p>
        <ul class="timeline-highlights">
          <li>Laravel applications and REST APIs</li>
          <li>MongoDB, testing, and deployment workflows</li>
        </ul>
        <div class="timeline-tags">
          <span>Laravel</span><span>MongoDB</span><span>JavaScript</span><span>APIs</span>
        </div>
      </div>
    </article>

    <article class="timeline-item" data-aos="fade-up" data-aos-delay="100">
      <div class="timeline-node">
        <span class="timeline-dot"></span>
        <time class="timeline-year" datetime="2017">2017</time>
      </div>
      <div class="timeline-card timeline-card--highlight">
        <span class="timeline-badge"><i class="bi bi-award"></i> Certification</span>
        <span class="timeline-phase">Cloud foundations</span>
        <h3>AWS Developer Associate</h3>
        <p>Earned my first cloud certification and began architecting scalable backends on AWS for production workloads.</p>
        <ul class="timeline-highlights">
          <li>EC2, RDS, S3, CloudFormation</li>
          <li>High-availability patterns for client platforms</li>
        </ul>
        <div class="timeline-tags">
          <span>AWS</span><span>EC2</span><span>RDS</span><span>S3</span>
        </div>
      </div>
    </article>

    <article class="timeline-item" data-aos="fade-up" data-aos-delay="50">
      <div class="timeline-node">
        <span class="timeline-dot"></span>
        <time class="timeline-year" datetime="2018">2018–20</time>
      </div>
      <div class="timeline-card">
        <span class="timeline-phase">Cloud era</span>
        <h3>Cloud &amp; Backend Engineer</h3>
        <p>Deepened AWS expertise with enterprise clients—designing infrastructure that scales before traffic spikes, not after.</p>
        <ul class="timeline-highlights">
          <li>EFS, EBS, multi-AZ database design</li>
          <li>Infrastructure automation and cost-aware architecture</li>
        </ul>
        <div class="timeline-tags">
          <span>AWS</span><span>CloudFormation</span><span>EFS</span><span>Architecture</span>
        </div>
      </div>
    </article>

    <article class="timeline-item" data-aos="fade-up" data-aos-delay="100">
      <div class="timeline-node">
        <span class="timeline-dot"></span>
        <time class="timeline-year" datetime="2020">2020–23</time>
      </div>
      <div class="timeline-card">
        <span class="timeline-phase">Platform engineering</span>
        <h3>DevOps &amp; Kubernetes Engineer</h3>
        <p>Owned cluster operations, IaC, and observability. Built the platform skills that later became my MLOps foundation.</p>
        <ul class="timeline-highlights">
          <li>Kubernetes deployments, Terraform, GitOps</li>
          <li>Prometheus, Grafana, OpenTelemetry pipelines</li>
          <li>CKAD (2021) and CKA certifications</li>
        </ul>
        <div class="timeline-tags">
          <span>Kubernetes</span><span>Terraform</span><span>Prometheus</span><span>Grafana</span>
        </div>
      </div>
    </article>

    <article class="timeline-item" data-aos="fade-up" data-aos-delay="50">
      <div class="timeline-node">
        <span class="timeline-dot"></span>
        <time class="timeline-year" datetime="2021">2021</time>
      </div>
      <div class="timeline-card timeline-card--highlight">
        <span class="timeline-badge"><i class="bi bi-award"></i> CKAD</span>
        <span class="timeline-phase">Kubernetes depth</span>
        <h3>Certified Kubernetes Application Developer</h3>
        <p>Validated hands-on skills designing and operating workloads on Kubernetes—deployments, services, networking, and troubleshooting.</p>
        <div class="timeline-tags">
          <span>CKAD</span><span>K8s</span><span>Helm</span>
        </div>
      </div>
    </article>

    <article class="timeline-item" data-aos="fade-up" data-aos-delay="100">
      <div class="timeline-node">
        <span class="timeline-dot"></span>
        <time class="timeline-year" datetime="2023">2023</time>
      </div>
      <div class="timeline-card timeline-card--ai">
        <span class="timeline-badge timeline-badge--ai"><i class="bi bi-stars"></i> Pivot</span>
        <span class="timeline-phase">AI engineering</span>
        <h3>Generative AI in Production</h3>
        <p>Shifted focus from pure infrastructure to shipping LLM products—RAG, APIs, evaluation, and model deployment.</p>
        <ul class="timeline-highlights">
          <li>Enterprise RAG over internal docs (LangChain, Pinecone, FastAPI)</li>
          <li>Streaming LLM APIs with Redis session memory</li>
          <li>MLflow experiment tracking and SageMaker fine-tuning</li>
          <li>ComfyUI workflows for repeatable image generation (see 2024)</li>
        </ul>
        <div class="timeline-tags">
          <span>LangChain</span><span>OpenAI</span><span>RAG</span><span>FastAPI</span><span>MLflow</span>
        </div>
      </div>
    </article>

    <article class="timeline-item" data-aos="fade-up" data-aos-delay="50">
      <div class="timeline-node">
        <span class="timeline-dot"></span>
        <time class="timeline-year" datetime="2024">2024</time>
      </div>
      <div class="timeline-card timeline-card--ai">
        <span class="timeline-badge timeline-badge--ai"><i class="bi bi-image"></i> Real-world</span>
        <span class="timeline-phase">Generative media</span>
        <h3>ComfyUI Production Pipelines</h3>
        <p>
          A marketing team was spending days in Photoshop creating product hero images and social crops for each SKU.
          I built <strong>ComfyUI</strong> node workflows that take a plain product photo and output on-brand backgrounds,
          lighting, and sizes—then wired them to a <strong>FastAPI</strong> job queue and <strong>GPU</strong> worker so the team could batch hundreds of assets overnight to <strong>S3/CDN</strong>.
        </p>
        <ul class="timeline-highlights">
          <li>ControlNet + IP-Adapter for consistent brand look across variants</li>
          <li>ComfyUI API automation—no manual clicking through the UI for production runs</li>
          <li>Replaced ~3 days/week of designer batch work with a repeatable pipeline</li>
        </ul>
        <div class="timeline-tags">
          <span>ComfyUI</span><span>Stable Diffusion</span><span>ControlNet</span><span>FastAPI</span><span>GPU</span><span>S3</span>
        </div>
      </div>
    </article>

    <article class="timeline-item timeline-item--current" data-aos="fade-up" data-aos-delay="100">
      <div class="timeline-node">
        <span class="timeline-dot timeline-dot--pulse"></span>
        <time class="timeline-year" datetime="2025">Now</time>
      </div>
      <div class="timeline-card timeline-card--current">
        <span class="timeline-badge timeline-badge--live"><i class="bi bi-circle-fill"></i> Present</span>
        <span class="timeline-phase">Today</span>
        <h3>AI Engineer · LLMs · MLOps · ComfyUI</h3>
        <p>Building agent workflows, vector search, ComfyUI image pipelines, and inference platforms on EKS—with observability, guardrails, and cost controls baked in.</p>
        <ul class="timeline-highlights">
          <li>Agents &amp; tool-calling with LangGraph and structured outputs</li>
          <li>ComfyUI + LLM stacks: text copilots and generative visuals in one platform</li>
          <li>LLM observability: LangSmith, Phoenix, Grafana dashboards</li>
          <li>GPU inference on EKS, Bedrock, and SageMaker endpoints</li>
        </ul>
        <div class="timeline-tags">
          <span>LangGraph</span><span>ComfyUI</span><span>Pinecone</span><span>EKS</span><span>LangSmith</span>
        </div>
      </div>
    </article>

  </section>

  <section class="about-connect" data-aos="fade-up">
    <h2>Let's connect</h2>
    <p>I write about RAG, agents, and MLOps on the <a href="/blog">blog</a>. Open to conversations on production AI—reach out anytime.</p>
    <div class="about-connect-links">
      <a href="mailto:{{ site.email }}" class="btn-get-started">Email me</a>
      <a href="https://github.com/{{ site.github_username }}" class="btn-get-started btn-outline" target="_blank" rel="noopener">GitHub</a>
      <a href="https://www.linkedin.com/in/{{ site.linkedin_username }}" class="btn-get-started btn-outline" target="_blank" rel="noopener">LinkedIn</a>
    </div>
  </section>

</div>
