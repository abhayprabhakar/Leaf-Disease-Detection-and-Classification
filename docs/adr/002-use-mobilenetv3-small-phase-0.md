# ADR-002: Use MobileNetV3-Small for Phase 0 Leaf Validation

* **Date:** 2025-07-23

## Context

The initial system architecture assumed that every input image would be a leaf. This is an unsafe assumption for a production environment, where input could include irrelevant subjects like the hydroponics equipment, hands, or other background objects. Processing these non-leaf images through the entire pipeline would be computationally wasteful and could lead to unpredictable errors in later phases.

A pre-filtering "gatekeeper" phase was needed to validate that an input image is, in fact, a leaf before it enters the main classification pipeline. The primary requirements for this gatekeeper model are maximum speed (low latency) and a small memory footprint, while still maintaining high accuracy for a simple binary task.

## Decision

We will implement a **"Phase 0: Leaf Validation"** as the first step in the architecture.

For this task, we have selected the **MobileNetV3-Small** model. The model will be fine-tuned on a dataset of "leaf" vs. "non-leaf" images to perform a binary classification. An image will only proceed to Phase 1 if it is confidently classified as a "leaf" by this model.

## Consequences

### Positive

* **Dramatic Improvement in Efficiency:** By rejecting irrelevant images at the very beginning, we prevent the larger, more computationally expensive models (in Phases 1, 2, and 3) from running unnecessarily. This significantly reduces average inference time and compute cost.
* **Increased System Robustness:** The pipeline becomes more reliable by ensuring only valid data enters the core logic.
* **Minimal Latency Overhead:** MobileNetV3-Small is specifically designed for high-speed execution on edge devices. The time it adds to process a valid leaf image is negligible (typically a few milliseconds).
* **Low Memory Footprint:** The model is extremely lightweight, making it easy to deploy and requiring minimal system resources.

### Negative

* **Additional Model Maintenance:** This introduces another model that must be trained, evaluated, and maintained over the project's lifecycle.
* **Potential for False Negatives:** There is a small risk that the Phase 0 model could incorrectly classify a real leaf as "non-leaf," causing the image to be rejected. This risk can be mitigated with a comprehensive training dataset and careful selection of the classification threshold.