# Adversarial ML Researcher · AI Red Teamer · Agentic Security Engineer

## Adversarial ML

**Fashion-MNIST CNN**

FGSM at ε=0.10 drops accuracy from 82% to 4%.

<img src="./img/fashionmnist_gradcam.png" alt="GradCAM activations for all 10 garment classes" width="720"/>

*GradCAM activations across all 10 classes.*

<img src="./img/fashionmnist_fgsm_per_class.png" alt="Per-class accuracy drop under FGSM" width="600"/>

*Per-class accuracy under FGSM attack.*

**VGG-11 Traffic Sign Classification**

Fine-tuned to 93.2% on GTSRB, but GradCAM shows the model reads background context instead of the sign.

<img src="./img/vgg11_gradcam.png" alt="GradCAM attention heatmaps on GTSRB" width="720"/>

*GradCAM attention heatmaps on GTSRB.*

**Wine Color Classification**

A 0.9938 F1 classifier flips on a 0.09 mg/L SO₂ perturbation.

<img src="./img/wine_epsilon.png" alt="Decision boundary distance and robustness vs confidence" width="620"/>

*Decision boundary distance and robustness vs confidence.*

**Network Intrusion Detection**

Attacked an XGBoost IDS on 2.54M UNSW-NB15 flows, then adversarial training restored F1 at ε=0.20 from 0.27 to 0.95.

<img src="./img/fig_shap_beeswarm.png" alt="SHAP beeswarm of top features" width="560"/>

*SHAP top features by impact.*

<img src="./img/fig_hardening_comparison.png" alt="Standard vs adversarially trained MLP under attack" width="800"/>

*Standard vs adversarially trained MLP under attack.*

**CATT-CCS**

Research paper targeting ACM CCS 2027: unconstrained gradient attacks inflate published NIDS evasion rates by 14 to 71 percentage points.

**OT Anomaly Detection**

HDBSCAN and K-Means both miss Modbus/TCP replay attacks because replayed traffic is statistically normal at the layer they observe.

## Agentic Security

**VERITAS** · *SANS FIND EVIL! Hackathon 2026*

Autonomous Windows forensic pipeline whose isolated auditor refuted 7 of 9 triage findings before they reached the report.

<img src="./img/adversa-architecture.png" alt="VERITAS layered forensic architecture" height="200"/> <img src="./img/adversa-guardrails.png" alt="VERITAS 4-gate validator" height="200"/>

*Layered architecture and 4-gate validator.*

**Elastic IR Agent** · *Elastic × Google Cloud Agent Builder Hackathon 2026*

Autonomous IR agent over 73,909 Windows attack events with session-scoped memory that blocks cross-case IOC contamination.

**Splunk IR Agent** · *Splunk Agentic Ops Hackathon 2026*

Alert-triggered autonomous investigation with read-only, allowlist-validated SPL.

## Foundations

**Gradient Descent from Scratch**

Manual gradient descent implemented and compared against autograd.

**GAN: Oxford Flowers**

Generator vs discriminator across 250 epochs.

<img src="./img/flowers progression.png" alt="Generator progression from noise to flowers" width="720"/>

*Generator progression from noise to flowers.*

**MIT xPro · Deep Learning: Mastering Neural Networks** <img src="./img/Deep Learning_ Mastering Neural Networks.png" alt="Cert" width="90"/>

## Deployed Applications

**MNIST Digit Recognition** · [![Live](https://img.shields.io/badge/Live-digits.di--sasso.com-blue?style=flat-square)](https://digits.di-sasso.com)

Draw a digit and a PyTorch CNN returns per-digit confidences with live conv activations, serverless on AWS.

<img src="./img/draw.png" alt="MNIST draw canvas" width="340"/> <img src="./img/hiddenlayer.png" alt="Conv layer filter visualization" width="330"/>

*Draw canvas and conv layer visualization.*

**GPT-Nano Text Generation** · [![Live](https://img.shields.io/badge/Live-lstm.di--sasso.com-blue?style=flat-square)](https://lstm.di-sasso.com)

7M-parameter GPT-style transformer trained on WikiText-2 via SageMaker, deployed serverless.

<img src="./img/lstm.png" alt="GPT-nano next-token probability bars" width="700"/>

*Next-token probability output.*

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
