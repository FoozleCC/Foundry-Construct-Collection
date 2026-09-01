# <img class="addon-heading" src="/foundry-physics-body.svg" alt="Foundry Physics Body icon"> Foundry Physics Body

Foundry Physics Body is the per-instance rigid-body behavior that creates and
manages Jolt bodies through the Foundry Physics world plugin.

## Install and dependency

1. Import the Foundry Physics Body addon package.
2. Ensure one Foundry Physics world object exists in the project.
3. Add Foundry Physics Body behavior to relevant objects.

Without a Foundry Physics world object, body creation fails.

## Scripting

When using JavaScript or TypeScript, use this behavior as the per-instance body
owner and keep body lifecycle updates coordinated with Foundry Physics world
initialization.

## Core capabilities

- Static, dynamic, and kinematic body types.
- Multiple collider shapes with source/override sizing controls.
- Force, impulse, torque, and angular impulse application.
- Material tuning: mass, friction, restitution, damping, gravity scale, density.
- Collision filtering via 8-bit layer/mask.
- Collision triggers including object-parameter variants.

## Collider shapes

Supported shapes:

- Box
- Sphere
- Capsule
- Cylinder
- Tapered capsule
- Tapered cylinder
- Convex hull
- Triangle mesh (static only)
- Plane (static only)
- Height field (static only)

If host geometry for the selected shape is unavailable, runtime falls back to a
safe shape and logs a warning.

## Physics Body properties

### Setup

- Enabled on start.
- Body type.
- Collider shape and source.
- Override size/offset/scale and manual values.
- Tapered shape radius scales.
- Height field resolution.
- Mesh flip axis.

### Filtering and trigger

- Collision layer and collision mask.
- Is trigger (sensor behavior without physical response).

### Material and motion constraints

- Mass, density.
- Friction, restitution.
- Linear damping, angular damping.
- Gravity scale.
- Treat as bullet.
- Rotation locks X/Y/Z.
- Position locks X/Y/Z.

## Physics Body conditions

### Setup state

- Has body.
- Is enabled.
- Is static / Is dynamic / Is kinematic.
- Is treated as bullet.
- Is layer enabled.
- Is mask enabled.

### Collision triggers

- Is colliding with object.
- On collision with object.
- On collision ended with object.

## Physics Body actions

### Setup and lifecycle

- Create body.
- Destroy body.
- Set enabled.
- Set body type.
- Set collider shape.
- Set collider size/size axes.
- Set collider offset.
- Set auto-size from bounds.
- Set auto-resize with instance scale.
- Sync from instance bounds.

Use this group to create/destroy bodies and to configure collider state.

### Transform and velocity

- Set sync mode (`physics-to-object`, `object-to-physics`, `manual`).
- Set body position.
- Set body rotation (Euler degrees) or quaternion.
- Set linear velocity.
- Set angular velocity.
- Set rotation locks.
- Set position locks.

Use this group to manage body transform and kinematic/dynamic velocity values.

### Joints

- Create distance joint. Two dynamic bodies remain free to translate and orbit as a pair; make one body static when one end should stay fixed in the world.
- Create revolute joint.
- Create limited revolute joint. Limits are signed stops relative to the orientation at creation: `-90` to `90` permits 90° each way and 180° stop-to-stop; `-120` to `120` permits 120° each way and 240° stop-to-stop, not 240° from the starting pose in one direction.
- Create prismatic joint with optional signed translation limits relative to the zero creation position and a velocity motor.
- Remove all joints.

Joint anchors and axes use world-space coordinates. The selected object's first
picked instance is connected. Joint actions can run directly in **On start of
layout** and initialize both bodies before queuing the constraint. Leave
**Collide connected** set to **No** for the usual joint setup, especially when
the colliders touch or overlap. For a door, connect the dynamic door to a static
frame with a limited revolute joint at the hinge position, normally around world Z.

For limited revolute joints, the creation pose is angle zero. The signed stops
apply to the selected **Other** body's rotation relative to **This** body around
the supplied axis. They are not absolute Construct Euler angles and are not
measured from the lower stop.

### Forces

- Apply impulse.
- Apply force.
- Apply force at point.
- Apply impulse at point.
- Apply force toward position.
- Apply impulse toward position.
- Apply torque.
- Apply angular impulse.

Use this group for continuous forces or one-shot impulses.

### Material

- Set mass.
- Set friction.
- Set restitution.
- Set linear damping.
- Set angular damping.
- Set gravity scale.
- Set density.
- Set treat as bullet.

Use this group to tune mass response, bounce, friction, damping, and CCD.

### Collision filtering

- Set collision layer.
- Set collision mask.
- Enable/disable layer bit.
- Enable/disable mask bit.

Use this group to control what can collide with this body.

## Physics Body expressions

### Identity and shape

- `BodyId`, `BodyType`, `ColliderShape`, `HasBody`.
- `ColliderSizeX/Y/Z`, `ColliderOffsetX/Y/Z`.

### Collision context

- `CollidingBodyId`.
- `CollidingUID`.

These values are most useful inside collision trigger events.

### Transform and velocity

- `SyncMode`.
- `BodyX`, `BodyY`, `BodyZ`.
- `VelX`, `VelY`, `VelZ`.
- `AngVelX`, `AngVelY`, `AngVelZ`.
- `Speed`, `AngularSpeed`.

### Material and mass properties

- `Mass`, `Friction`, `Restitution`.
- `LinearDamping`, `AngularDamping`.
- `GravityScale`, `Density`.
- `MassCenterX/Y/Z`.
- `InertiaAroundX/Y/Z`.

### Filtering and diagnostics

- `CollisionLayer`, `CollisionMask`.
- `RotationLockedX/Y/Z`.
- `PositionLockedX/Y/Z`.
- `LastError`.

## Collision filtering rule

Two bodies collide only if both checks pass:

1. bodyA layer intersects bodyB mask
2. bodyB layer intersects bodyA mask

## Typical usage

1. Dynamic prop:
	set body type dynamic, set collider shape, tune mass and damping.
2. Trigger volume:
	set Is trigger true and react via object-scoped collision triggers.
3. Projectile body:
	enable bullet treatment and use impulse toward target position.

## Troubleshooting

- Body not simulating: verify sync mode and body type are appropriate.
- Wrong collider size: check source mode and size override flags.
- Missing collisions: inspect both layer and mask settings.
- No collision callbacks: ensure body exists and behavior is enabled.

## Related pages

- [Foundry Physics](./foundry-physics)
- [Foundry Physics Character](./foundry-physics-character)
- [Issue Reporting](../issue-reporting)
