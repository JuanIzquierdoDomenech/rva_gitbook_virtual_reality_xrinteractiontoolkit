---
icon: arrow-left-long-to-line
---

# Drawers

<figure><img src="../.gitbook/assets/Gemini_Generated_Image_2i7tbv2i7tbv2i7t.png" alt=""><figcaption></figcaption></figure>

Drawers work similarly to doors but use a **Configurable Joint** instead of a Hinge Joint, since drawers slide linearly rather than rotate.

Unlike doors where we grab a handle, drawers can often be **grabbed directly** since the entire drawer face acts as the interaction point.

## Creating the Drawer

Select the drawer child object (e.g., **cabinet\_drawer\_01**) and add:

* **Box Collider** (fit it to the drawer shape)
* **Rigidbody**
* **Configurable Joint**
* **XR Grab Interactable**

> 💡 **Difference from doors:** For drawers, we add the XR Grab Interactable directly to the drawer itself, not to a separate handle. This is because the drawer front panel serves as the natural grab point.

### Configure the Configurable Joint

The Configurable Joint allows precise control over which axes can move and how far.

Configure it for linear sliding motion:

* **Axis**: (1, 0, 0) — the primary axis direction
* **X Motion**: Locked
* **Y Motion**: Locked
* **Z Motion**: Limited (the sliding direction)
* **Angular X Motion**: Locked
* **Angular Y Motion**: Locked
* **Angular Z Motion**: Locked
* **Linear Limit**: 0.1 (how far the drawer can slide, in meters)

> 💡 **Motion settings explained:**

| Setting     | Meaning                                          |
| ----------- | ------------------------------------------------ |
| **Locked**  | No movement allowed on this axis                 |
| **Limited** | Movement allowed within the Linear Limit         |
| **Free**    | Unlimited movement (not recommended for drawers) |

> 💡 **Why lock all Angular motions?** Drawers should only slide, not rotate. Locking all angular axes prevents the drawer from tilting or spinning when grabbed.

> 💡 **Choosing the sliding axis:** The axis depends on your drawer's orientation:
>
> * **Z Motion Limited**: Drawer slides forward/backward (most common)
> * **X Motion Limited**: Drawer slides left/right
> * Adjust based on how your model is oriented in the scene

> 💡 **Linear Limit value:** This is in meters. A value of 0.1 means the drawer can slide 10cm from its starting position. Measure your drawer depth and set appropriately.

### Configure XR Grab Interactable

Configure the XR Grab Interactable for constrained physics movement:

* **Movement Type**: **Velocity Tracking**
* **Track Rotation**: ✗ Disabled
* **Track Scale**: ✗ Disabled

> ⚠️ **Critical settings:**

| Setting            | Value             | Why                                 |
| ------------------ | ----------------- | ----------------------------------- |
| **Movement Type**  | Velocity Tracking | Required for physics joints to work |
| **Track Rotation** | Disabled          | Drawer shouldn't rotate with hand   |
| **Track Scale**    | Disabled          | Drawer shouldn't scale              |

> 💡 **Why disable Track Rotation?** When you grab the drawer, you only want it to follow your hand's position (pull motion), not rotation. If Track Rotation were enabled, twisting your wrist would try to rotate the drawer, fighting against the locked angular constraints and causing jittery behavior.

### Test the Drawer

Run the project, grab the drawer, and pull to open it!

> 💡 **Expected behavior:**
>
> 1. Grab the drawer front with the grip button
> 2. Pull your hand back — the drawer slides open
> 3. Push forward — the drawer slides closed
> 4. The drawer stops at the Linear Limit (fully open) and at its starting position (fully closed)

***

## Cabinet hierarchy

```
SM_cabinet
├── cabinet_door_01              [Box Collider, Rigidbody, Hinge Joint]
│   └── Handle                   [Box Collider, Rigidbody, Fixed Joint, XR Grab Interactable]
├── cabinet_door_02              [Box Collider, Rigidbody, Hinge Joint]
│   └── Handle                   [Box Collider, Rigidbody, Fixed Joint, XR Grab Interactable]
├── cabinet_door_03              [Box Collider, Rigidbody, Hinge Joint]
│   └── Handle                   [Box Collider, Rigidbody, Fixed Joint, XR Grab Interactable]
├── cabinet_door_04              [Box Collider, Rigidbody, Hinge Joint]
│   └── Handle                   [Box Collider, Rigidbody, Fixed Joint, XR Grab Interactable]
├── cabinet_drawer_01            [Box Collider, Rigidbody, Configurable Joint, XR Grab Interactable]
└── cabinet_drawer_02            [Box Collider, Rigidbody, Configurable Joint, XR Grab Interactable]
```

***

## Troubleshooting Drawers

| Problem                          | Cause                               | Solution                              |
| -------------------------------- | ----------------------------------- | ------------------------------------- |
| Drawer doesn't move              | Movement Type not Velocity Tracking | Change to Velocity Tracking           |
| Drawer rotates when grabbed      | Track Rotation enabled              | Disable Track Rotation                |
| Drawer slides wrong direction    | Wrong axis set to Limited           | Change which Motion axis is Limited   |
| Drawer slides too far/not enough | Linear Limit incorrect              | Adjust Linear Limit value             |
| Drawer jiggles or shakes         | Physics conflict                    | Check Rigidbody settings, add damping |
| Drawer falls through cabinet     | Missing colliders                   | Add colliders to cabinet interior     |

***

## Physics Joint Summary

| Joint Type             | Use Case                  | Movement                                |
| ---------------------- | ------------------------- | --------------------------------------- |
| **Hinge Joint**        | Doors, lids, levers       | Rotation around single axis             |
| **Fixed Joint**        | Connecting handle to door | No relative movement (locked together)  |
| **Configurable Joint** | Drawers, sliders          | Customizable linear/angular constraints |

***

## Documentation

* [Configurable Joint Documentation](https://docs.unity3d.com/Manual/class-ConfigurableJoint.html)
* [Hinge Joint Documentation](https://docs.unity3d.com/Manual/class-HingeJoint.html)
* [Physics Joints Overview](https://docs.unity3d.com/Manual/Joints.html)
