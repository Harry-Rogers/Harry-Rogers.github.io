---
title: "Automating the Clock Drawing Test with Deep Learning and Saliency Maps"
excerpt: "Medical-AI imaging collaboration on automated screening for cognitive impairment via the Clock Drawing Test."
collection: portfolio
permalink: /portfolio/clock-drawing-test/
---

The **Clock Drawing Test (CDT)** is one of the most widely used bedside screens for cognitive impairment — patients are asked to draw a clock face showing a specified time, and clinicians score the result for spatial, temporal, and executive-function errors. The test is cheap, fast, and well-validated, but scoring is **subjective and clinician-dependent**, which limits its consistency at scale and its usefulness in remote or unsupervised settings.

This project — a collaboration with Mayne, Sami, and Prof. De La Iglesia — applies **deep learning** to the CDT scoring problem and pairs the classifier with **saliency maps** so that the model's decision is inspectable rather than a black box. The clinically meaningful question is not just "is the drawing impaired" but "*where* in the drawing did the model find evidence of impairment", because that is what a clinician can argue with, validate, or override. The work is published as [Automating the Clock Drawing Test with Deep Learning and Saliency Maps (EPIA 2024)](/publication/2024-09-01-epia-clock-drawing/).

For me this project marked the transition from precision-agriculture computer vision into **medical AI**, while keeping the through-line of **explainable AI in high-stakes, human-in-the-loop deployments**. The same architectural intuitions — interpretability is not optional, evaluation must respect the deployment context, the model is a collaborator rather than an oracle — port directly from the field to the clinic.

<!-- TODO(owner): add link to code repository if public -->
