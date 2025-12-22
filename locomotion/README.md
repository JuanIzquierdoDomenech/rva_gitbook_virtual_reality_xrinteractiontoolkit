# Locomotion

Locomotion in VR refers to how the **user moves through the virtual world** beyond their physical play space.&#x20;

The XR Interaction Toolkit provides several locomotion providers that can be combined.

<figure><img src="../.gitbook/assets/moving.png" alt="" width="563"><figcaption></figcaption></figure>

### Locomotion Overview

The XRI locomotion system consists of:

* **Character Controller**: Unity's built-in component for physics-based movement (collisions, slopes, steps)
* **Locomotion Mediator**: Coordinates multiple locomotion providers to prevent conflicts
* **Locomotion Providers**: Components that implement specific movement types (move, turn, teleport, climb)
