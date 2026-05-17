---
title: "Automating the Clock Drawing Test with Deep Learning and Saliency Maps"
excerpt: "XAI applied to a clinical screening tool — interpretable scoring of the Clock Drawing Test via saliency maps."
collection: portfolio
permalink: /portfolio/clock-drawing-test/
---

The **Clock Drawing Test (CDT)** is a widely used bedside screen for cognitive impairment — patients draw a clock face showing a specified time, and clinicians score the drawing for spatial, temporal, and motor errors. The test is cheap and well-validated, but scoring is subjective, which limits consistency at scale.

For me the interesting problem here is **eXplainable AI**, not the clinical task per se. A deep learning classifier on its own gives a verdict; pairing it with **saliency maps** (Grad-CAM and related techniques) turns it into something a clinician can interrogate — *where* in the drawing did the model find evidence, and does that evidence agree with what a human would point at? That is the XAI question the project answers, with the CDT serving as a concrete high-stakes vision task. The work is published as [*Automating the Clock Drawing Test with Deep Learning and Saliency Maps*](/publication/2024-epia-clock-drawing-test/) (Mayne, Sami, De La Iglesia, **Rogers** — EPIA 2024).

It also marked a bridge from XAI in precision agriculture into XAI applied to clinical imaging — the same interpretability stack (saliency methods, evaluation against expert attention, sanity checks) transfers between domains without losing its shape.

<!-- TODO(owner): add link to code repository if public -->
