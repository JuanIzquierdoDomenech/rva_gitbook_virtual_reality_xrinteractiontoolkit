---
icon: person-ski-jumping
---

# Jumping

Jumping requires two additional providers: a **Gravity Provider** to pull the player back down, and a **Jump Provider** to launch the player upward.

## Add Gravity Provider

Create a child GameObject under **Locomotion** called **Gravity**. Add a **Gravity Provider** component.

Configure it:

* **Mediator**: Reference the **Locomotion** GameObject
* **Transformation Priority**: Set to **10** (higher than other providers)

> 💡 **Why Transformation Priority 10?** The priority determines which provider takes precedence when multiple providers want to transform the player. Gravity should have a higher priority than movement (default 0) so it's always applied — you don't want movement to cancel out falling.

> 💡 **Gravity Provider settings:**

| Property                          | Description                                           |
| --------------------------------- | ----------------------------------------------------- |
| **Use Gravity**                   | Enable/disable gravity application                    |
| **Terminal Velocity**             | Maximum falling speed (default: 90 m/s)               |
| **Gravity Acceleration Modifier** | Multiplier for gravity strength                       |
| **Sphere Cast Layer Mask**        | Which layers count as "ground" for grounded detection |

> 💡 **What does the Gravity Provider do?** It continuously applies downward force to the XR Origin when the player is not grounded. It also provides a **grounded check** using sphere casts, which the Jump Provider uses to know if jumping is allowed.

***

## Step 2: Configure Layer to Avoid Self-Collision

The Gravity Provider uses sphere casts to detect the ground. By default, it might detect the **Character Controller's own collider**, thinking the player is always grounded.

To fix this, set the **XR Origin (VR)** GameObject's **Layer** to **Ignore Raycast**.

> ⚠️ **Important:** Only change the layer on the **XR Origin (VR)** GameObject itself, **not its children**. The cameras and hands should remain on their default layers for interactions to work properly.

> 💡 **Why does this work?** The Gravity Provider's sphere cast uses a layer mask that excludes "Ignore Raycast" by default. By putting the XR Origin on this layer, the sphere cast passes through the Character Controller and detects the actual floor beneath the player.

***

#### Step 3: Add Jump Provider

Create a child GameObject under **Locomotion** called **Jump**. Add a **Jump Provider** component.

Configure it:

* **Mediator**: Reference the **Locomotion** GameObject
* **Jump Input**: `XRI Right Locomotion/Jump` (or assign for both hands)

> 💡 **Jump Provider settings:**

| Property                        | Description                                                    |
| ------------------------------- | -------------------------------------------------------------- |
| **Jump Height**                 | How high the player jumps in meters (default: 1.25)            |
| **Jump Forgiveness Window**     | Time after leaving ground where jump still works (coyote time) |
| **Disable Gravity During Jump** | Prevents gravity from fighting the upward motion               |

> 💡 **Air Jump Settings:**

| Property                   | Description                                               |
| -------------------------- | --------------------------------------------------------- |
| **Unlimited In Air Jumps** | Allow infinite double-jumps                               |
| **In Air Jump Count**      | Number of jumps allowed while airborne (if not unlimited) |

> 💡 **Variable Height Jump Settings:**

| Property                         | Description                                     |
| -------------------------------- | ----------------------------------------------- |
| **Variable Height Jump**         | Holding the button longer jumps higher          |
| **Min Jump Hold Time**           | Minimum time for a short jump                   |
| **Max Jump Hold Time**           | Time held for maximum jump height               |
| **Early Out Deceleration Speed** | How quickly to stop rising when button released |

> 💡 **What is Jump Forgiveness Window (Coyote Time)?** This is a game-feel improvement where the player can still jump for a short time after walking off a ledge. It makes platforming feel more responsive and forgiving. Named after the cartoon coyote who runs off cliffs and doesn't fall until he looks down!

***

#### Jump Button Location

The **Jump** action is bound to the **thumbstick press** (clicking the thumbstick down).

> 💡 **Default XRI button mapping for locomotion:**

| Button                 | Action |
| ---------------------- | ------ |
| Left Thumbstick        | Move   |
| Right Thumbstick       | Turn   |
| Right Thumbstick Press | Jump   |

***

#### Step 4: Test Jumping

Run the project and press the right thumbstick to jump.

> 💡 **If jumping doesn't work, check:**
>
> 1. The **Gravity Provider** has a higher **Transformation Priority** than other providers
> 2. The **XR Origin (VR)** layer is set to **Ignore Raycast**
> 3. The **Jump Input** action is assigned correctly
> 4. There's a floor with a collider for the player to stand on

***

### Updated hierarchy

After adding Jump, your Locomotion hierarchy should look like this:

```
XR Origin (VR)                    [Layer: Ignore Raycast]
├── Camera Offset
│   ├── Main Camera
│   ├── Left Hand
│   └── Right Hand
└── Locomotion                    [XR Body Transformer, Locomotion Mediator]
    ├── Move                      [Continuous Move Provider]
    ├── Turn                      [Continuous Turn Provider, Snap Turn Provider]
    ├── Gravity                   [Gravity Provider - Priority 10]
    └── Jump                      [Jump Provider]
```

***

### Provider priority

| Provider             | Recommended Priority | Reason                                         |
| -------------------- | -------------------- | ---------------------------------------------- |
| Continuous Move      | 0 (default)          | Base movement                                  |
| Continuous/Snap Turn | 0 (default)          | Base rotation                                  |
| Jump                 | 0 (default)          | Triggered action                               |
| Gravity              | 10                   | Must always apply, even during other movements |
| Teleport             | 0 (default)          | Triggered action                               |

> 💡 **Priority rule of thumb:** Continuous effects (gravity) need higher priority than triggered actions (jump, teleport) and continuous input (move, turn). This ensures gravity is never "skipped" while other providers are active.
