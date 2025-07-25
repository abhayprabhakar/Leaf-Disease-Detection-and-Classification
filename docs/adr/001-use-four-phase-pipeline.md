# 1. Use Four-Phase Pipeline

* **Date:** 2025-07-23

## Context

*What is the issue we're trying to solve? What are the technical, business, or other drivers?*

Example: "We need a structured way to process leaf images that is both accurate and computationally efficient. The system must handle various inputs, from non-leaf images to healthy and diseased leaves of different crops."

## Decision

*What is the change we're proposing?*

Example: "We will implement a four-phase sequential architecture. Each phase acts as a filter: Leaf Validation -> Crop Identification -> Health Assessment -> Disease Localization. An image only proceeds to the next phase if it meets the criteria of the current one."

## Consequences

*What are the positive and negative consequences of this decision? What are the trade-offs?*

Example:
* **Positive:**
    * Improves computational efficiency by exiting early for irrelevant images or healthy leaves.
    * Increases accuracy by narrowing down the problem at each step.
    * Modular design makes it easier to update or replace individual components.
* **Negative:**
    * Increases initial development complexity as four separate models need to be managed.
    * Slightly increases latency for the "worst-case" scenario (a diseased leaf) due to the sequential checks.