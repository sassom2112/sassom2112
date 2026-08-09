# Adversarial ML Researcher · AI Red Teamer · Agentic Security Engineer

> I break AI systems. Then I build ones that hold.
>
> Every model here was attacked after it was trained, and every agentic system assumes the adversary controls the input.

---

## Adversarial ML

**Fashion-MNIST CNN**

GradCAM traced the Shirt class to an unstable decision boundary, and FGSM at ε=0.10 collapsed accuracy from 82% to 4%.

<img src="./img/fashionmnist_gradcam.png" alt="GradCAM activations - all 10 garment classes" width="720"/>
<img src="./img/fashionmnist_fgsm_per_class.png" alt="Per-class accuracy drop under FGSM (ε=0.10)" width="600"/>

**VGG-11 Traffic Sign Classification**

Fine-tuned VGG-11 to 93.2% on GTSRB, where GradCAM shows the speed limit classifier keys on background context rather than the sign itself, the same failure mode behind physical adversarial patches.

<img src="./img/vgg11_gradcam.png" alt="GradCAM - VGG-11 attention heatmaps on GTSRB" width="720"/>

**Wine Color Classification**

A statistically near-perfect classifier (F1 0.9938) flips on a 0.09 mg/L SO₂ perturbation, and the features SHAP ranks most important are exactly the ones FGSM exploits.

<img src="./img/wine_epsilon.png" alt="Decision Boundary Distance + Robustness vs Confidence" width="620"/>

**Network Intrusion Detection: IDS Red Teaming and Hardening**

Built and attacked an XGBoost IDS on 2.54M UNSW-NB15 flows with domain-constrained FGSM/PGD and black-box transfer attacks, then PGD adversarial training restored F1 at ε=0.20 from 0.27 to 0.95 with no clean-accuracy cost.

<p align="center">
  <img src="./img/fig_shap_beeswarm.png" alt="SHAP Beeswarm - Top Features by Impact" width="560"/>
</p>
<p align="center">
  <img src="./img/fig_hardening_comparison.png" alt="Standard vs. Adversarially Trained MLP - F1 and Evasion Rate vs Epsilon" width="800"/>
</p>

**CATT-CCS: Constraint Inflation in Adversarial NIDS Evaluation**

Research paper targeting ACM CCS 2027 showing that unconstrained gradient attacks inflate published NIDS evasion rates by 14 to 71 percentage points across three datasets by generating physically impossible traffic.

**OT Anomaly Detection: Replay Attack Blind Spot**

HDBSCAN and K-Means both miss Modbus/TCP replay attacks on ICSSim data because replayed traffic is statistically normal at every layer a single-layer detector can observe.

---

## Agentic Security

**VERITAS: Autonomous Windows Forensic Investigation** · *SANS FIND EVIL! Hackathon 2026*

Three-phase forensic pipeline in which an isolated adversarial auditor refuted 7 of 9 triage findings before they reached the report, blocking false accusations architecturally rather than by prompt.

<p align="center">
  <img src="./img/adversa-architecture.png" alt="VERITAS Layered Forensic Architecture" height="200"/>
  <img src="./img/adversa-guardrails.png" alt="VERITAS Guardrails - 4-gate validator" height="200"/>
</p>

**Elastic IR Agent** · *Elastic × Google Cloud Agent Builder Hackathon 2026*

Autonomous IR agent over 73,909 real Windows attack events, with memory hard-scoped per session so IOC contamination between cases is blocked at the dispatch layer, not by prompt instruction.

**Splunk IR Agent** · *Splunk Agentic Ops Hackathon 2026*

Alert-triggered autonomous investigation that treats the model as untrusted input: every SPL template is read-only and every substituted field is allowlist-validated before execution.

---

## Foundations

**Gradient Descent from Scratch**

Manual gradient descent implemented against autograd to show exactly what optimizers compute.

**GAN: Oxford Flowers**

Generator versus discriminator training dynamics across 250 epochs.

<img src="./img/flowers progression.png" alt="Generator Progression - Noise to Flowers across 250 epochs" width="720"/>

**MIT xPro · Deep Learning: Mastering Neural Networks** <img src="./img/Deep Learning_ Mastering Neural Networks.png" alt="Cert" width="90"/>

---

## Deployed Applications

**MNIST Digit Recognition** · [![Live](https://img.shields.io/badge/Live-digits.di--sasso.com-blue?style=flat-square)](https://digits.di-sasso.com)

Draw a digit and a PyTorch CNN returns per-digit confidences with live conv-layer activation maps, served at zero idle cost on Lambda, API Gateway, and S3.

<img src="./img/draw.png" alt="MNIST draw canvas" width="340"/> <img src="./img/hiddenlayer.png" alt="Conv layer filter visualization" width="330"/>

**GPT-Nano Text Generation** · [![Live](https://img.shields.io/badge/Live-lstm.di--sasso.com-blue?style=flat-square)](https://lstm.di-sasso.com)

A 7M-parameter GPT-style transformer built from scratch, trained on WikiText-2 through an AWS SageMaker pipeline and deployed serverless at zero idle cost.

<img src="./img/lstm.png" alt="GPT-nano next-token probability bars" width="700"/>

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
