---
title: "Closing The Loop on Precision Spraying"
excerpt: "DPhil thesis project on automated evaluation of agricultural precision sprayers, in collaboration with Syngenta."
collection: portfolio
permalink: /portfolio/precision-spraying-closing-the-loop/
---

**Closing The Loop on Precision Spraying** is the topic of my DPhil/PhD thesis at the University of East Anglia, completed within the [AgriFoRwArdS CDT](https://agriforwards-cdt.ac.uk/) and in industrial partnership with **Syngenta**, supervised by Prof. Beatriz De La Iglesia. The work attacks a gap in agricultural automation: precision sprayers can target weeds with high spatial accuracy, but there is no closed-loop way to *verify* that a spray actually landed where it was supposed to. Without that feedback signal, the agronomic and environmental benefits of precision application cannot be audited at scale.

The project's contributions span the full sense-act-evaluate loop:

- An **automated evaluation system** for precision sprayers, built around computer vision pipelines that can analyse post-spraying imagery in field-like conditions — [TAROS 2023](/publication/2023-taros-precision-spraying-evaluation/) (Best Application nominee).
- A **deposit identification system** that detects and localises individual spray deposits on plant surfaces — [IEEE CASE 2023](/publication/2023-ieee-case-deposit-identification/).
- An investigation of **interpretable quantized CNNs** for deployment on the constrained hardware that lives on a real sprayer — [KDIR 2023](/publication/2023-kdir-interpretable-quantized-cnn/) (Best Paper).
- **Domain-specific augmentations and robustness testing** that push these models toward generalising across crops, lighting, and growth stages — [Neural Computing and Applications, 2024](/publication/2024-ncaa-domain-specific-augmentations/).
- A follow-up [arXiv preprint, 2024](/publication/2024-arxiv-post-spraying-evaluation/) extending the post-spraying evaluation work.

Taken together, this thread of work is an end-to-end argument that **explainability, edge-deployability, and rigorous evaluation** must be co-designed in agricultural AI — not bolted on. The closed loop is not just a control-systems metaphor; it is the condition under which precision spraying can be trusted, regulated, and scaled.

<!-- TODO(owner): add link to thesis PDF / repository when finalised -->
