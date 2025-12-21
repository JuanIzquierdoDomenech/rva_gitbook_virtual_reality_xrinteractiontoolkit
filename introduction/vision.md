---
description: 'VR Optics: How We See in 3D'
icon: eyes
---

# Vision

## Stereoscopic vision

VR creates the illusion of depth by mimicking how **human eyes work**.

Each eye sees a slightly different image. The brain combines them to perceive **depth**.

<figure><img src="../.gitbook/assets/2_cameras_per_eye.png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
In VR, we use **two cameras** separated by a distance called **IPD** (Interpupillary Distance).
{% endhint %}

## The role of lenses

A screen held close to the eyes would show a **tiny portion** of the image.

<figure><img src="../.gitbook/assets/curved_lenses.png" alt=""><figcaption></figcaption></figure>

**Curved lenses** bend light to expand the **Field of View (FOV)**.

| Without Lenses                     | With Lenses              |
| ---------------------------------- | ------------------------ |
| Small FOV                          | Large FOV                |
| Most of the screen is "dead space" | Screen fills your vision |

## Lens distortion correction

Lenses expand FOV, but they also **distort the image**.

To compensate, VR applies **barrel distortion** to the rendered image, so it looks correct _after_ passing through the lenses.

<figure><img src="../.gitbook/assets/stereo_vision.png" alt=""><figcaption></figcaption></figure>

## Stereoscopic rendering in Unity

Unity handles all of this automatically when using their own XR framework.

<figure><img src="../.gitbook/assets/stereo_vision_unity.jpg" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
In older versions of Unity, IPD was simulated using 2 cameras slightly separated.
{% endhint %}
