---
description: One codebase for multiple devices
---

# Unity XR Stack

VR hardware varies widely: Oculus, Vive, Windows Mixed Reality, PlayStation VR...

Unity's XR architecture provides a **unified API** that abstracts hardware differences.

<figure><img src="../.gitbook/assets/XR_stack_unity.png" alt=""><figcaption></figcaption></figure>

***

## Stack Layers

### 1. Provider Implementations (Bottom)

Hardware-specific plugins that communicate with each device:

* **ARCore XR Plugin:** Android AR
* **ARKit XR Plugin:** iOS AR
* **Oculus XR Plugin:** Meta Quest, Rift
* **Windows XR Plugin:** Windows Mixed Reality
* **Magic Leap XR Plugin:** Magic Leap headsets
* **VSP/3rd Party:** Other vendors

> You install only the plugins for your target platforms.

### 2. Unity XR SDK

The **abstraction layer** that normalizes input from all providers.

Your code talks to the XR SDK — not to specific hardware.

### 3. XR Subsystems

Modular systems that handle specific XR features:

<table><thead><tr><th width="211.0390625">Subsystem</th><th>Function</th></tr></thead><tbody><tr><td><strong>Display</strong></td><td>Stereoscopic rendering (VR)</td></tr><tr><td><strong>Input</strong></td><td>Controllers, hand tracking</td></tr><tr><td><strong>Camera</strong></td><td>Passthrough, background rendering (MR)</td></tr><tr><td><strong>Planes</strong></td><td>Surface detection (AR)</td></tr><tr><td><strong>Image Tracking</strong></td><td>Marker recognition (AR)</td></tr><tr><td><strong>Meshing</strong></td><td>Environment scanning (MR)</td></tr><tr><td><strong>Raycast</strong></td><td>Hit testing against real/virtual world</td></tr></tbody></table>

### 4. Developer Tools (Top)

High-level tools that simplify development:

<table><thead><tr><th width="246.671875">Tool</th><th>Purpose</th></tr></thead><tbody><tr><td><strong>AR Foundation</strong></td><td>Cross-platform AR development</td></tr><tr><td><strong>XR Interaction Toolkit</strong></td><td>Ready-made interactions (grab, teleport, ray)</td></tr><tr><td><strong>MARS</strong></td><td>Authoring tool for context-aware AR</td></tr></tbody></table>

***

## Why this matters

| Without XR Stack                     | With XR Stack                   |
| ------------------------------------ | ------------------------------- |
| Write separate code for each headset | Write once, deploy everywhere   |
| Learn each vendor's SDK              | Learn one unified API           |
| Maintain multiple codebases          | Single project, multiple builds |

***
