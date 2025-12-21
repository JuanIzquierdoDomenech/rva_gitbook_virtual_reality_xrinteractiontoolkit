---
description: >-
  Setting up a Unity project for VR development with the XR Interaction Toolkit
  targeting Meta Quest 3.
---

# Project setup

### Create a New Project

Open **Unity Hub** and create a new project using:

* **Editor version**: Unity 6 (6000.3.0f1 or later)
* **Template**: Universal 3D (URP)

<figure><img src=".gitbook/assets/01_creating_project.png" alt=""><figcaption></figcaption></figure>

> 💡 **Why URP?** The Universal Render Pipeline offers better performance on standalone VR headsets like Meta Quest 3.

***

### Install XR Plugin Management

Navigate to **Edit → Project Settings → XR Plug-in Management** and click **Install XR Plugin Management**.

<figure><img src=".gitbook/assets/02_install_plugin.png" alt=""><figcaption></figcaption></figure>

***

### Enable OpenXR

In **XR Plug-in Management**, check **OpenXR** as your plug-in provider.

<figure><img src=".gitbook/assets/03_activate_openxr.png" alt=""><figcaption></figcaption></figure>

> ⚠️ You may see a warning icon — this will be resolved in the next steps.

***

### Configure OpenXR for Meta Quest

Select the **Android tab** (robot icon) in XR Plug-in Management, then go to **OpenXR** settings:

1. Add the **Interaction Profiles** you need (Oculus Touch, Meta Quest Touch Pro, Hand Interaction, etc.)
2. Enable **Meta Quest Support** under OpenXR Feature Groups

<figure><img src=".gitbook/assets/04_enable_interaction_profiles.png" alt=""><figcaption></figcaption></figure>

***

### Fix Project Validation Issues

Go to **Project Validation** under XR Plug-in Management and click **Fix All** to resolve any configuration issues.

<figure><img src=".gitbook/assets/05_fix_all.png" alt=""><figcaption></figcaption></figure>

> ✅ Green checkmarks indicate resolved issues. Yellow warnings are optional but recommended to fix.

***

### Basic Camera Tracking (Manual Approach)

For a minimal VR setup, add a **Tracked Pose Driver** component to the **Main Camera**:

* **Device**: Generic XR Device
* **Pose Source**: Center Eye - HMD Reference
* **Tracking Type**: Rotation And Position

<figure><img src=".gitbook/assets/06_add_tracked_pose_driver.png" alt=""><figcaption></figcaption></figure>

> 💡 This is the simplest way to get head tracking working. We'll use XR Origin for a more complete setup.

***

### Install XR Interaction Toolkit

Open **Window → Package Manager**, select **Unity Registry**, and install **XR Interaction Toolkit**.

<figure><img src=".gitbook/assets/08_interaction_toolkit.png" alt=""><figcaption></figcaption></figure>

***

### Import Starter Assets

After installation, go to the **Samples** tab and import **Starter Assets**.

<figure><img src=".gitbook/assets/09_starter_assets.png" alt=""><figcaption></figcaption></figure>

> 💡 Starter Assets include pre-configured input actions and presets that save significant setup time.

***

### Switch Build Platform to Meta Quest

Open **File → Build Profiles**, select **Meta Quest**, and click **Switch Platform**.

<figure><img src=".gitbook/assets/09_switch_platform_meta.png" alt=""><figcaption></figcaption></figure>

Connect your Meta Quest via USB and use **Build And Run** to deploy directly to the headset.

***

### Add XR Origin

Delete the default **Main Camera** and add an **XR Origin (VR)**:

**GameObject → XR → XR Origin (VR)**

<figure><img src=".gitbook/assets/10_XROrigin.png" alt=""><figcaption></figcaption></figure>

***

### Configure Tracking Origin Mode

Select the **XR Origin** and set **Tracking Origin Mode** to **Floor**.

<figure><img src=".gitbook/assets/11_floor_takes_account_height.png" alt=""><figcaption></figcaption></figure>

> 💡 **Floor** mode uses the guardian/boundary floor level, accounting for the user's real height.

***

### Create Hand Tracking Objects

Under **XR Origin → Camera Offset**, create two empty GameObjects:

* **Left Hand**
* **Right Hand**

Add a **Tracked Pose Driver** component to each.

<figure><img src=".gitbook/assets/12_empty_go_left_hand_dup.png" alt=""><figcaption></figcaption></figure>

***

### Configure Left Hand Input

In the **Tracked Pose Driver** on **Left Hand**, assign the XRI Left input actions:

<figure><img src=".gitbook/assets/13_left_position.gif" alt=""><figcaption></figcaption></figure>

Final configuration for Left Hand:

* **Position Input**: XRI Left/Position
* **Rotation Input**: XRI Left/Rotation
* **Tracking State Input**: XRI Left/Tracking State

<figure><img src=".gitbook/assets/14_left_hand_data.png" alt=""><figcaption></figcaption></figure>

***

### Configure Right Hand Input

Apply the same configuration to **Right Hand** using XRI Right actions:

* **Position Input**: XRI Right/Position
* **Rotation Input**: XRI Right/Rotation
* **Tracking State Input**: XRI Right/Tracking State

<figure><img src=".gitbook/assets/15_right_hand_data.png" alt=""><figcaption></figcaption></figure>

***

### Add Visual Representation for Hands

Add a **Sphere** as a child of each hand to visualize controller positions:

1. Right-click on **Left Hand** → **3D Object → Sphere**
2. Scale it down (e.g., 0.03, 0.03, 0.03)
3. **Remove the Sphere Collider** component
4. Repeat for **Right Hand**

<figure><img src=".gitbook/assets/16_child_sphere_no_collider.png" alt=""><figcaption></figcaption></figure>

> ⚠️ Remove the collider to prevent unintended physics interactions.

***

### Install XR Interaction Simulator

To test without a VR headset, import the **XR Interaction Simulator** sample from Package Manager.

<figure><img src=".gitbook/assets/17_install_simulator.png" alt=""><figcaption></figcaption></figure>

***

### Add Simulator to Scene

Drag the **XR Interaction Simulator** prefab into your scene hierarchy.

Location: `Assets/Samples/XR Interaction Toolkit/[version]/XR Interaction Simulator/`&#x20;

<figure><img src=".gitbook/assets/18_drag_simulator.png" alt=""><figcaption></figcaption></figure>

> 💡 The simulator allows you to test VR interactions using keyboard and mouse in the Editor.

***

### Useful links

* [XR Interaction Toolkit Documentation](https://docs.unity3d.com/Packages/com.unity.xr.interaction.toolkit@3.0/manual/index.html)
* [OpenXR Plugin Documentation](https://docs.unity3d.com/Packages/com.unity.xr.openxr@1.8/manual/index.html)
* [Meta Quest Developer Setup](https://developer.oculus.com/documentation/unity/unity-gs-overview/)
