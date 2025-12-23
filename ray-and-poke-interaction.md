# Ray and Poke interaction

Ray and Poke are two essential interaction methods for VR user interfaces. **Ray interaction** allows users to point at UI elements from a distance, while **Poke interaction** enables direct touch-based input, similar to pressing physical buttons.

<figure><img src=".gitbook/assets/Gemini_Generated_Image_2vg9e92vg9e92vg9.png" alt=""><figcaption></figcaption></figure>

## Ray Interaction with UI

Ray interaction is ideal for interacting with UI panels that are out of arm's reach. The user points a ray at buttons, sliders, or other UI elements and uses the trigger to interact.

### Create a UI Canvas

Create a standard Unity UI Canvas with some interactive elements like buttons and sliders. Right-click in the Hierarchy and select **UI > Canvas**, then add child elements such as Panel, Text, Button, and Slider.

![Creating a UI Canvas with interactive elements](.gitbook/assets/01_create_ui.png)

{% hint style="info" %}
**Standard Unity UI**:&#x20;

The XR Interaction Toolkit works with Unity's built-in UI system (uGUI). You don't need special VR-specific UI components, just configure the Canvas correctly.
{% endhint %}

### Convert Canvas to World Space

By default, Canvas uses **Screen Space - Overlay** mode, which renders UI flat on the screen. For VR, we need **World Space** so the UI exists as a physical object in the 3D environment.

Select the **Canvas** and configure:

* **Render Mode**: World Space
* **Event Camera**: Main Camera
* **Scale**: 0.001 for X, Y, and Z (to convert pixel units to meters)

Position the Canvas in your scene where you want it to appear.

![Canvas configured for World Space](.gitbook/assets/02_worldspace.png)

{% hint style="info" %}
**Why Scale 0.001?**&#x20;

Unity UI uses pixel coordinates (e.g., 1920×1080). At scale 1, your Canvas would be 1920 meters wide! Scaling by 0.001 converts pixels to millimeters, making a 1000px wide panel roughly 1 meter in VR.
{% endhint %}

### Add Ray Interactor to Right Hand

Navigate to **Assets > Samples > XR Interaction Toolkit > \[version] > Starter Assets > Prefabs > Interactors** and drag the **Ray Interactor** prefab as a child of **Right Hand**.

![Adding Ray Interactor prefab to Right Hand](.gitbook/assets/03_drag_ray_interactor.png)

> 💡 **Starter Assets Prefabs**: The XR Interaction Toolkit samples include pre-configured interactor prefabs. These save significant setup time and follow best practices.

### Configure the Ray Interactor

Select the **Ray Interactor** and configure:

* **Position**: Reset to 0, 0, 0 (so the ray originates from the hand)
* **Handedness**: Right
* **UI Interaction**: ✓ Enabled
* **UI Press Input**: `XRI Right Interaction/UI Press` and `XRI Right Interaction/UI Press Value`
* **UI Scroll Input**: `XRI Right Interaction/UI Scroll`

![Ray Interactor configuration](https://claude.ai/chat/images/04_setup_interactor.png)

> 💡 **UI Interaction checkbox**: This enables the ray to interact with Unity UI elements. Without this, the ray would only work with 3D interactables.

> 💡 **UI Press vs Select**: UI Press (typically the trigger) is used for clicking UI elements. Select (grip button) is used for grabbing 3D objects. Keep these separate for intuitive interactions.

### Replace Input Module on EventSystem

The default **Input System UI Input Module** on the EventSystem doesn't understand XR input. We need to replace it with the **XR UI Input Module**.

Select the **EventSystem** GameObject and **remove** the Input System UI Input Module component.

![Remove the default Input System UI Input Module](https://claude.ai/chat/images/05_remove.png)

> 💡 **EventSystem**: This GameObject manages all UI input in Unity. Every scene with UI needs one. When you create a Canvas, Unity automatically creates an EventSystem if one doesn't exist.

### Add XR UI Input Module

With the **EventSystem** still selected, add the **XR UI Input Module** component.

![Add XR UI Input Module component](https://claude.ai/chat/images/06_add_new.png)

> 💡 **XR UI Input Module**: This component translates XR controller input (rays, pokes) into UI events that Unity's EventSystem understands. It's the bridge between VR controllers and uGUI.

### Apply the XRI Preset

Click the **Preset selector** (icon in the top-right of the component) and select the **XRI Default XR UI Input Module** preset from the Starter Assets.

![Selecting the XRI preset for XR UI Input Module](https://claude.ai/chat/images/07_select_preset.png)

> 💡 **Presets**: These pre-configure all the input action references automatically. Without the preset, you'd need to manually assign each action (Point, Click, Scroll, etc.).

### Add Tracked Device Graphic Raycaster

Select your **Canvas** and add the **Tracked Device Graphic Raycaster** component.

![Adding Tracked Device Graphic Raycaster to Canvas](https://claude.ai/chat/images/08_add_graphic_raycaster_component.png)

> 💡 **Tracked Device Graphic Raycaster**: This component allows XR rays to interact with the Canvas. It replaces the standard Graphic Raycaster which only works with mouse/touch input.

> 💡 **Keep both raycasters?** You can keep the standard Graphic Raycaster alongside the Tracked Device one if you want the UI to work with both mouse (for testing in Editor) and VR controllers.

### Hide Ray When Not Pointing at UI (Optional)

To hide the ray when it's not hovering over any valid target, configure the **XR Interactor Line Visual** component on the Ray Interactor:

* **Invalid Color Gradient**: Set alpha to 0 (fully transparent)

![Setting Invalid Color Gradient alpha to 0](https://claude.ai/chat/images/09_hide_when_not_pointing.png)

> 💡 **Ray Visual States**:

| State             | Description                                            |
| ----------------- | ------------------------------------------------------ |
| **Valid Color**   | Ray is hovering over an interactable target            |
| **Invalid Color** | Ray is not pointing at anything interactable           |
| **Blocked Color** | Ray is blocked by an object before reaching the target |

Run the project and point the ray at the UI. Press the trigger to interact with buttons and sliders.

![Ray interaction with UI in action](https://claude.ai/chat/images/10_example_interacting_ui.gif)

***

## Enabling Object Grabbing with Ray

With the current configuration, the ray can only interact with UI — it cannot grab 3D objects. Let's fix that.

Currently, pointing at grabbable objects does nothing. The ray only responds to UI elements.

![Ray cannot interact with grabbable objects](https://claude.ai/chat/images/11_cannot_interact_with_other.gif)

### Configure Select Input for Grabbing

Select the **Ray Interactor** and scroll to the **Input Configuration** section. Configure:

* **Select Input**: `XRI Right Interaction/Select` and `XRI Right Interaction/Select Value`
* **Activate Input**: `XRI Right Interaction/Activate` and `XRI Right Interaction/Activate Value`

![Configuring Select Input on Ray Interactor](https://claude.ai/chat/images/12_setup_ray_interactor_select.png)

> 💡 **Select vs UI Press**: Now the ray uses two different inputs:

| Input                  | Action            | Use                       |
| ---------------------- | ----------------- | ------------------------- |
| **UI Press** (Trigger) | Click UI elements | Buttons, sliders          |
| **Select** (Grip)      | Grab 3D objects   | Pick up items at distance |

### Test Ray Grabbing

Run the project. You can now point at grabbable objects and use the grip button to grab them at a distance.

![Grabbing objects with ray at distance](https://claude.ai/chat/images/13_grabbing_with_ray.gif)

> 💡 **Ray Grabbing Use Cases**: Ray grabbing is useful for picking up objects that are far away or in hard-to-reach places. However, direct grabbing (with hands) usually feels more natural for nearby objects.

***

## XR Interaction Group

When you have multiple interactors on the same hand (Direct Interactor, Ray Interactor, Teleport Interactor), you may want to prevent them from being active simultaneously.

### Add XR Interaction Group

Add an **XR Interaction Group** component to the **Right Hand** GameObject. Configure the **Starting Group Members** with all interactors on that hand:

* Element 0: Right Direct Interactor
* Element 1: Teleport Interactor
* Element 2: Ray Interactor

![XR Interaction Group configuration](https://claude.ai/chat/images/14_interaction_group.png)

> 💡 **What does XR Interaction Group do?** It prevents multiple interactors from selecting objects at the same time. For example, you can't accidentally grab an object with your hand while also trying to interact with UI via ray. Only one interactor in the group can be active at a time.

> 💡 **Override Configuration**: The matrix at the bottom lets you control which interactors can interrupt others. By default, all interactors can override each other.

***

## Poke Interaction

Poke interaction allows users to physically press UI buttons with their fingers, like touching a real touchscreen. This creates a more immersive and intuitive experience for nearby UI panels.

### Prepare the Canvas for Poke

For poke interaction, the Canvas should be set up the same way as for ray interaction (World Space, Tracked Device Graphic Raycaster, etc.). Make sure your Canvas has interactive elements like buttons.

![Canvas prepared for poke interaction](https://claude.ai/chat/images/15_poke_canvas_after_setup_uievents.png)

### Add Poke Interactor to Left Hand

Navigate to **Assets > Samples > XR Interaction Toolkit > \[version] > Starter Assets > Prefabs > Interactors** and drag the **Poke Interactor** prefab as a child of **Left Hand**.

![Adding Poke Interactor prefab to Left Hand](https://claude.ai/chat/images/16_drag_poke_interactor.png)

> 💡 **Why Left Hand?** In this setup, we're using the right hand for ray/grab interactions and the left hand for poke. This allows natural two-handed interaction: point and click with right, poke with left. You can add poke to both hands if desired.

### Configure the Poke Interactor

Select the **Poke Interactor** and configure:

* **Handedness**: Left
* **UI Interaction**: ✓ Enabled

![Poke Interactor configuration](https://claude.ai/chat/images/17_right_handedness.png)

> 💡 **Poke Interactor Key Settings**:

| Property             | Description                                           |
| -------------------- | ----------------------------------------------------- |
| **Poke Depth**       | How far the finger must push to trigger a press       |
| **Poke Width**       | Width of the poke detection area                      |
| **UI Interaction**   | Enable to interact with Canvas UI elements            |
| **Click UI On Down** | Trigger click immediately on contact (vs. on release) |

> 💡 **Poke Point child**: The Poke Interactor prefab includes a **Poke Point** child Transform. This defines exactly where the "fingertip" is for poke detection. Position it at your hand model's index finger tip for accurate poking.

### Test Poke Interaction

Run the project and move your left hand toward the UI. Touch buttons with your finger to press them.

![Poke interaction in action](https://claude.ai/chat/images/18_demo_poke.gif)

> 💡 **Poke vs Ray**: Both interaction methods can coexist. Use poke for nearby panels (menus attached to your wrist, control panels) and ray for distant UI elements.

***

## Updated Hand Hierarchy

After setting up ray and poke interactions, your hand hierarchy should look like this:

```
Camera Offset
├── Main Camera
├── Left Hand                     [Tracked Pose Driver]
│   ├── Sphere                    (visual)
│   ├── Left Direct Interactor    [XR Direct Interactor]
│   └── Poke Interactor           [XR Poke Interactor]
│       └── Poke Point            (fingertip position)
└── Right Hand                    [Tracked Pose Driver, XR Interaction Group]
    ├── Sphere                    (visual)
    ├── Right Direct Interactor   [XR Direct Interactor]
    ├── Teleport Interactor       [XR Ray Interactor]
    └── Ray Interactor            [XR Ray Interactor]
```

***

### Documentation

* [XR Ray Interactor Documentation](https://docs.unity3d.com/Packages/com.unity.xr.interaction.toolkit@3.3/manual/xr-ray-interactor.html)
* [XR Poke Interactor Documentation](https://docs.unity3d.com/Packages/com.unity.xr.interaction.toolkit@3.3/manual/xr-poke-interactor.html)
* [XR UI Input Module Documentation](https://docs.unity3d.com/Packages/com.unity.xr.interaction.toolkit@3.3/manual/xr-ui-input-module.html)
* [Tracked Device Graphic Raycaster](https://docs.unity3d.com/Packages/com.unity.xr.interaction.toolkit@3.3/manual/tracked-device-graphic-raycaster.html)
* [XR Interaction Group](https://docs.unity3d.com/Packages/com.unity.xr.interaction.toolkit@3.3/manual/xr-interaction-group.html)
