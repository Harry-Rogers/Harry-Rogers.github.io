---
title: "Quality Assurance for Home Spirometry using Machine Learning"
excerpt: "Machine learning for signal-quality assurance of at-home spirometry, enabling reliable remote respiratory monitoring."
collection: portfolio
permalink: /portfolio/home-spirometry-qa/
---

**Spirometry** — the measurement of how forcefully and quickly a patient can exhale — is the workhorse test of respiratory medicine, used to diagnose and monitor asthma, COPD, cystic fibrosis, and post-transplant lung function. Modern home spirometers let patients record measurements daily from their own living rooms, which is transformative for longitudinal monitoring — but it surfaces an unsolved problem: **without a respiratory physiologist in the room to coach the manoeuvre, a large fraction of at-home readings are technically invalid** (premature exhalation termination, coughs, sub-maximal effort). Feeding invalid traces into clinical decision-making is worse than having no data at all.

This project — with Gardiner, Lines, Wilson, and Aung — develops **machine learning models that automatically flag the quality of a home spirometry trace** against ATS/ERS-style acceptability criteria, so that clinicians can trust the upstream signal before making any inference downstream. The aim is a QA layer that turns noisy at-home telemetry into a clinically usable longitudinal signal, without forcing patients to learn to score their own manoeuvres. The work is published as [Quality Assurance for Home Spirometry using Machine Learning (IEEE ISCC 2025)](/publication/2025-ieee-iscc-spirometry-qa/).

This is the second strand of my **medical-AI imaging / signals** work, and it cements a methodological commitment: **the model's first job in a clinical pipeline is to know when its inputs are bad**. Calibration and quality assurance are upstream of every downstream claim.

<!-- TODO(owner): add link to code / dataset release if public -->
