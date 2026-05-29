# Adversarial ML Researcher · AI Red Teamer · Agentic Security Engineer

> I break AI systems. Then I build ones that hold.
>
> Every model in this portfolio was attacked after it was trained — with domain-constrained adversarial examples, black-box transfer attacks, and shortcut learning exploitation. The agentic systems were designed from the start assuming a capable adversary controls the input. The same question drives all of it: **can a security system hold when the attacker understands it?**

---

## The Progression

### Stage 1 — Finding Where Models Break

Three classifiers. Three different attack surfaces. The same methodology each time: train the model, use GradCAM to find what it's actually responding to, then exploit it.

**Fashion-MNIST CNN — Adversarial Robustness**

GradCAM reveals the Shirt classifier has no stable discriminative region — its decision boundary simultaneously borders Pullover, Coat, and T-shirt/top. Any gradient step finds a neighboring class almost instantly. At ε=0.10, accuracy collapses from 82% → 4%. Clean test accuracy: **82.3%**.

<img src="./img/fashionmnist_gradcam.png" alt="GradCAM activations — all 10 garment classes" width="720"/>
<img src="./img/fashionmnist_fgsm_per_class.png" alt="Per-class accuracy drop under FGSM (ε=0.10)" width="600"/>

---

**VGG-11 Traffic Sign Classification — Adversarial Robustness**

Two-phase fine-tuning of VGG-11 on GTSRB (43 classes, 39K images): frozen backbone 63.9% → full fine-tuning **93.2%**. The 29-point gap is itself a finding — frozen ImageNet features fail to generalize to this visual domain, which means the model's confidence is not grounded in traffic sign geometry.

GradCAM confirms it: the 30 km/h classifier fires on background traffic lights and urban intersection context — not the sign itself. The model learned a proxy for speed limits. That proxy is the attack surface. This is the same failure mode behind physical-world adversarial patches on stop signs.

<img src="./img/vgg11_gradcam.png" alt="GradCAM — VGG-11 attention heatmaps on GTSRB" width="720"/>

---

**Wine Color Classification — Adversarial Analysis**

EDA → LR vs XGBoost → SHAP → FGSM on 6,497 samples. **F1: 0.9938 · ROC-AUC: 0.9999.** Minimum perturbation to flip a classification: **+0.09 mg/L SO₂** — below winery measurement noise. The model is statistically unassailable; geometrically, it is one imperceptible nudge from failure.

The features SHAP identifies as most important are the exact features FGSM identifies as most exploitable. **Explainability is a roadmap to the attack surface.** Transfer attack: 16.9% of adversarial examples crafted against logistic regression fool XGBoost — black-box evasion with no access to the target model's gradients or architecture.

<img src="./img/wine_epsilon.png" alt="Decision Boundary Distance + Robustness vs Confidence" width="620"/>

---

### Stage 2 — Adversarial ML Research: Network Intrusion Detection

The research question: *can a deployed NIDS classifier be evaded by an adversary who only understands the feature space — and do published evaluations actually measure that threat, or something easier?*

Two projects answer it. The first builds and attacks a real IDS. The second formalizes what was wrong with every evaluation that came before it.

---

**Network Intrusion Detection — IDS Red Teaming & Adversarial Hardening**

*sklearn · XGBoost · PyTorch · FGSM/PGD · SHAP · UNSW-NB15 (2.54M flows)*

► **Baseline**: Full ML lifecycle on 2.54M real network flows. XGBoost F1: **0.9640**, ROC-AUC: **0.9997**. SHAP TreeExplainer identifies the features driving every prediction — which also identifies the attack surface.

► **Domain-constrained adversarial attacks**: FGSM and PGD are applied with constraint projection — adversarial flows stay physically plausible (TTL ∈ [0,255], no negative packet counts, ports ∈ [0,65535]). Most published NIDS attack work skips this, producing inputs impossible on real networks and conclusions that don't hold operationally.

► **Black-box transfer attack**: A PyTorch MLP surrogate approximates the XGBoost decision boundary. Adversarial examples crafted against the surrogate — with no access to XGBoost weights — transfer at **16–18% evasion: a 15× increase in false negatives**. An adversary who understands the feature space can evade a classifier they cannot directly inspect.

► **Adversarial hardening**: Madry PGD adversarial training closes the gap with no clean-accuracy cost.

| | Clean F1 | F1 at ε=0.10 | F1 at ε=0.20 |
|---|---|---|---|
| Standard MLP | 0.9519 | 0.8865 | **0.2658** |
| Adversarially Trained MLP | 0.9524 | **0.9520** | **0.9494** |

At ε=0.20 the standard model collapses to near-random detection. The hardened model retains **99.7% of clean F1**. No accuracy-robustness tradeoff.

<p align="center">
  <img src="./img/fig_shap_beeswarm.png" alt="SHAP Beeswarm — Top Features by Impact" width="560"/>
</p>
<p align="center">
  <img src="./img/fig_hardening_comparison.png" alt="Standard vs. Adversarially Trained MLP — F1 and Evasion Rate vs Epsilon" width="800"/>
</p>

---

**CATT-CCS — Constraint Inflation in Adversarial NIDS Evaluation**

*Research paper targeting ACM CCS 2027 · PyTorch · scikit-learn · XGBoost · UNSW-NB15 · CICIDS-2017 · NSL-KDD*

The IDS work above raised a harder question: if unconstrained gradient attacks produce physically impossible inputs, what does it mean when a published paper reports 80% evasion? This paper answers it.

► **The finding**: Unconstrained PGD is free to push features into regions that cannot exist on a real network — TTL of −119, rates of 2.4, negative durations. Every percentage point of evasion bought by violating domain physics is not a real attack. It is a measurement error.

► **The gap, quantified across three datasets and three classifier architectures (MLP, Random Forest, XGBoost), three independent seeds each:**

| Dataset | Constrained PGD | Unconstrained PGD | Gap |
|---------|-----------------|-------------------|-----|
| UNSW-NB15 (ε=0.20) | 7.1% | 79.1% | **+72.0 pp** |
| NSL-KDD (ε=0.10) | 16.7% | 84.1% | **+67.4 pp** |
| CICIDS-2017 (ε=0.30) | 74.9% | 87.1% | **+12.2 pp** |

The gap scales with how many features have tight documented bounds. This is a mechanism, not a dataset artifact.

► **Transfer**: MLP surrogate attacks transfer near-perfectly to RF and XGBoost on CICFlowMeter features; moderately on mixed-type NIDS features. The white-box inflation finding holds regardless of architecture.

► **Infrastructure**: `netadv` library, 65 unit tests, three Colab benchmark notebooks, fully reproducible across seeds and datasets.

---

### Stage 3 — The Same Principle at the System Level

An LLM-based security agent faces an analogous threat: an attacker who can write to logs, craft alert metadata, or control file system artifacts can influence what the agent sees and concludes. Prompt-level guardrails are the equivalent of a standard (non-hardened) classifier — they work until the adversary pushes past ε.

The architectural answer at the model level was adversarial training with separation between clean and adversarial loss. The architectural answer at the system level is the same kind of separation: agents that receive findings but not reasoning, auditors that have a mandate to refute rather than confirm, tool servers that validate before any subprocess executes.

---

**VERITAS — Autonomous Windows Forensic Investigation**

*SANS FIND EVIL! Hackathon 2026 · Tested on SIFT Workstation*

**MCP · Windows Forensics · Dual-Agent Architecture · MITRE ATT&CK**

Three-phase pipeline for dead-disk and memory forensics on Windows images:

1. **Deterministic triage** — 25 SIFT commands, corpus-calibrated log-odds scoring across 9 MITRE techniques. No LLM in the loop, no hallucination surface.
2. **Agentic investigation** — Claude Sonnet sequences tool calls like a senior examiner: event logs → prefetch → registry hives → MFT → shellbags → hash verification. Receives raw artifacts only — no Pass 1 scores, no technique labels.
3. **Forensic auditor** — receives the finding list only, no access to prior reasoning. Mandate: assume every finding is a false positive until the filesystem proves otherwise.

Detection rules are trained via **automated Red Teaming**: a Red Agent generates evasion variants of known attack patterns against real Mordor/OTRF Sysmon telemetry; a Blue Agent learns to catch them. 3,000 iterations, 1,245 evasion variants evolved, 83 signals learned — with zero human intervention. The Red Agent is not a data augmentation trick. It is an adversary.

On the nfury test image: triage pass scored 9 techniques. The adversarial auditor confirmed 2, refuted 7. Without architectural separation, 7 false accusations would have entered the report. Prompt instructions do not prevent this. Separation does.

<p align="center">
  <img src="./img/adversa-architecture.png" alt="VERITAS Layered Forensic Architecture" height="200"/>
  <img src="./img/adversa-guardrails.png" alt="VERITAS Guardrails — 4-gate validator" height="200"/>
</p>

> A full disk + memory investigation runs in 17 minutes at $14 in API cost. LLMs hallucinate. In forensics, a hallucination is a false accusation. The architecture has to be defensible, not the prompt.

---

**Elastic IR Agent**

*Elastic Agent Builder × Google Cloud Agent Builder Hackathon 2026*

**Elasticsearch · Gemini · ES|QL · MCP**

Autonomous IR agent with hybrid semantic + ES|QL search over 73,909 real Windows attack events. Write-back memory builds persistent investigation context across sessions — with structural session isolation: `search_memory` is hard-scoped to the current `session_id` at the dispatch layer. The model cannot query across investigations regardless of what it requests. **IOC contamination between cases is blocked architecturally, not by prompt instruction.**

Memory content is sanitized before any Elasticsearch write — control characters stripped, input capped at 10,000 chars — explicitly to block indirect prompt injection via poisoned retrieval. An independent Forensic Auditor pass re-queries Elastic with read-only tools and labels every MITRE claim VERIFIED / REFUTED / UNVERIFIABLE with raw event evidence.

---

**Splunk IR Agent**

*Splunk Agentic Ops Hackathon 2026*

**Splunk · MITRE ATT&CK · SPL · Python**

End-to-end autonomous incident investigation triggered by a single alert — brute force, lateral movement, credential access, mapped to MITRE ATT&CK, IR report generated before an analyst opens their laptop.

The security boundary treats the model as untrusted input. All six SPL templates are read-only `search` queries — no `collect`, `outputlookup`, or write-back commands exist anywhere in the codebase. Format-substituted fields are validated before insertion: `earliest`/`latest` against a strict regex allowlist, `index` against `[a-zA-Z0-9_\-]` only. A malicious model output cannot append `| outputlookup evil` to a query. The tool dispatch allowlist blocks unknown tool names before any Splunk call executes.

---

## Foundations

| Project | What it demonstrates |
|---------|---------------------|
| Gradient Descent from Scratch | Manual gradient descent vs. autograd — what optimizers actually compute, no black box |
| GAN: Oxford Flowers | Adversarial training dynamics: generator vs. discriminator across 250 epochs |

<img src="./img/flowers progression.png" alt="Generator Progression — Noise to Flowers across 250 epochs" width="720"/>

**MIT xPro — Deep Learning: Mastering Neural Networks** <img src="./img/Deep Learning_ Mastering Neural Networks.png" alt="Cert" width="90"/>

---

## Deployed Applications

**MNIST Digit Recognition** · [![Live](https://img.shields.io/badge/Live-digits.di--sasso.com-blue?style=flat-square)](https://digits.di-sasso.com)

Draw a digit → Flask API → PyTorch CNN → per-digit confidence scores + Conv layer filter visualization in real time. Deployed on AWS (Lambda + API Gateway + CloudFront), containerized with Docker.

<img src="./img/draw.png" alt="MNIST draw canvas" width="340"/> <img src="./img/hiddenlayer.png" alt="Conv layer filter visualization" width="330"/>

**LSTM Text Generation** · [![Live](https://img.shields.io/badge/Live-lstm.di--sasso.com-blue?style=flat-square)](https://lstm.di-sasso.com)

Prompt → two-layer PyTorch LSTM → top-10 next-word probabilities. Intentionally undertrained to demonstrate why attention mechanisms exist. Deployed on Render.

<img src="./img/lstm.png" alt="LSTM next-word probability bars" width="700"/>

---

![Python](https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=white)
![PyTorch](https://img.shields.io/badge/PyTorch-EE4C2C?style=for-the-badge&logo=pytorch&logoColor=white)
![Claude](https://img.shields.io/badge/Claude_API-D97757?style=for-the-badge&logo=anthropic&logoColor=white)
![MCP](https://img.shields.io/badge/MCP_Server-000000?style=for-the-badge&logo=anthropic&logoColor=white)
![sklearn](https://img.shields.io/badge/scikit--learn-F7931E?style=for-the-badge&logo=scikitlearn&logoColor=white)
![XGBoost](https://img.shields.io/badge/XGBoost-189AB4?style=for-the-badge&logo=python&logoColor=white)
![Flask](https://img.shields.io/badge/Flask-000000?style=for-the-badge&logo=flask&logoColor=white)
![Docker](https://img.shields.io/badge/Docker-2496ED?style=for-the-badge&logo=docker&logoColor=white)
![AWS](https://img.shields.io/badge/AWS-232F3E?style=for-the-badge&logo=amazonaws&logoColor=white)
![Linux](https://img.shields.io/badge/Linux-FCC624?style=for-the-badge&logo=linux&logoColor=black)
![Git](https://img.shields.io/badge/Git-F05032?style=for-the-badge&logo=git&logoColor=white)
![Snyk](https://img.shields.io/badge/Snyk-4C4A73?style=for-the-badge&logo=snyk&logoColor=white)
