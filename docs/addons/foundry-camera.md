# <img class="addon-heading" src="/foundry-camera.svg" alt="Foundry Camera icon"> Foundry Camera

The Foundry Camera plugin controls the layout camera in 3D with runtime mode
switching, target management, transitions, obstruction handling, and shake.

## Install and dependencies

1. Import the Foundry Camera addon package.
2. Add one Foundry Camera object to your project.
3. Optional: add Foundry Camera Target behavior to one or more objects and
	switch camera target mode to behavior-driven targeting.

Foundry Camera has no hard dependency, but integrates with Foundry Camera
Target for multi-target framing.

## Scripting

When using JavaScript or TypeScript, keep script-driven camera values in sync
with event-sheet camera actions, since both update the same runtime state.

## Camera modes

- Orbit (`orbit`): target-centric free orbit controls.
- Follow (`follow`): third-person follow behavior with recenter controls.
- Character Orbit (`character-orbit`): character-focused orbit behavior.

## Camera properties

### Initial mode

Selects the startup mode (`orbit`, `follow`, or `character-orbit`).

### Enabled

If disabled, camera logic is not applied until re-enabled.

### User input

If enabled, camera input actions are honored.

### Target offset X, Y, Z

Adds a runtime offset to the resolved target point.

### Distance, Min distance, Max distance

Defines initial camera distance and zoom clamps.

### Yaw, Pitch, Min pitch, Max pitch

Sets initial orientation and pitch clamp range.

### Position smoothing, Rotation smoothing

Damping factors used when interpolating camera movement/rotation.

### Follow height

Additional vertical offset used by follow behavior.

### Mouse sensitivity, Zoom speed

Input multipliers for pointer look and zoom.

### Obstruction properties

Defines line-of-sight behavior when geometry blocks the target:

- Query mode (`none`, `raycast`, `shapecast`, `multi-ray`)
- Hit mode (`closest`, `any`, `all`)
- Offset space (`world-locked`, `camera-plane-relative`)
- Collision mask
- Padding, min clear distance
- Smooth in/out and restore delay
- Shape and shape radius
- Multi-ray preset/width/height/resolution

### Debug logging

Enables runtime debug output and diagnostics.

## Camera conditions

### General and target

- Is enabled
- Has target
- Is using target behavior

### Modes and transitions

- Is mode active
- Is transitioning
- On mode changed
- On transition finished

### Obstruction

- Is obstructed
- On obstruction result updated
- On obstruction hit
- On obstruction clear

### Shake

- Is shaking

## Camera actions

### General

- Set enabled: enable or disable camera runtime application.
- Enable user input: toggle input handling.
- Set position smoothing: update translation damping.
- Set rotation smoothing: update rotation damping.
- Set mouse sensitivity: update look multiplier.
- Set zoom speed: update zoom step/speed.

### Target

- Set target: assign a target object instance.
- Clear target: remove object target binding.
- Set target offset: update target offset vector.
- Set fixed target point: use explicit world position target.
- Use camera target behavior: read weighted target from target behavior.
- Use manual target: return to manual object/fixed point targeting.

### Mode switching

- Set mode: immediate mode swap.
- Transition to mode: eased mode blend over duration seconds.

### Orbit

- Set yaw / Set pitch: set target orientation angles.
- Set distance: set target boom length.
- Set zoom range: set min/max distance clamps.
- Set pitch range: set min/max pitch clamps.

### Follow

- Set follow height: update follow vertical offset.
- Suspend auto-follow: temporarily hold recenter behavior.
- Set camera following style: choose recenter policy.
- Set camera following tuning: set delay/speed/default pitch/max turn rate.
- Recenter behind target: snap camera behind controlled target.

### Obstruction

- Set obstruction enabled.
- Set obstruction query mode.
- Set obstruction hit mode.
- Set obstruction response mode.
- Set obstruction offset space.
- Set obstruction offsets.
- Set obstruction multi-ray.
- Set obstruction shape.
- Set obstruction filter.
- Set obstruction ignore UID.
- Set obstruction response tuning.
- Clear obstruction result.
- Clear custom obstruction probes.
- Remove custom obstruction probe.
- Add custom obstruction ray probe.
- Add custom obstruction shape probe.

### Shake

- Start shake: add trauma value.
- Stop shake: clear active trauma.

### Debug

- Set debug: toggle diagnostics.
- Debug dump state: print runtime camera state.

## Camera expressions

### Position and orientation

- `CameraX`, `CameraY`, `CameraZ` and aliases `PosX`, `PosY`, `PosZ`.
- `LookAtX`, `LookAtY`, `LookAtZ`.
- `Yaw`, `Pitch` and aliases `CamYaw`, `CamPitch`.
- `TargetYaw`, `TargetPitch`, `TargetDistance`.
- `Distance` and alias `Zoom`.

### Movement basis and tuning

- `CamForwardX`, `CamForwardY`, `CamRightX`, `CamRightY`.
- `MinDistance`, `MaxDistance`, `MinPitch`, `MaxPitch`.
- `MouseSensitivity`, `ZoomSpeed`, `PositionSmoothing`, `RotationSmoothing`.

### Target and mode

- `CurrentMode`.
- `TargetX`, `TargetY`, `TargetZ`.
- `TargetBehaviorCount`.
- `FollowHeight`, `FollowStyle`.

### Obstruction

- State: `Obstructed`, `HitCount`, `DesiredDistance`, `EffectiveDistance`,
	`ActiveProbeCount`.
- Closest hit: `HitX/Y/Z`, `HitNormalX/Y/Z`, `HitDistance`, `HitFraction`,
	`HitBodyUID`, `HitBodyName`.
- Indexed hit accessors: `HitXAt(index)` through `HitBodyNameAt(index)`.
- JSON payloads: `ClosestHitJSON`, `HitsJSON`.

### Shake

- `ShakeTrauma`.

## Typical usage

1. Basic orbit camera:
	 set mode to Orbit, set target, then tune yaw/pitch range and distance.
2. Character follow:
	 set mode to Follow, assign target object, tune follow and recenter.
3. Multi-target framing:
	 add Camera Target behavior to actors, then switch to behavior targeting.
4. Obstruction-safe camera:
	 enable obstruction and tune response smoothing for your level geometry.

## Troubleshooting

- Camera not moving: verify camera is enabled and has a valid target or fixed
  target point.
- Multi-target not used: ensure Use camera target behavior is active and at
  least one target behavior instance is enabled.
- Obstruction seems too aggressive: reduce padding and increase smooth-out.
- Obstruction does nothing: verify query mode is not `none` and collision mask
  includes relevant bodies.

## Related pages

- [Foundry Camera Target](./foundry-camera-target)
- [Issue Reporting](../issue-reporting)
- [Troubleshooting](../troubleshooting)
