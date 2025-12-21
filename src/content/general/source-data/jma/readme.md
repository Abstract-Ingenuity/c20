---
title: JMA
stub: false
about: 'resource:Animation data'
keywords:
  - jma
  - animation
thanks:
  "Abstract Ingenuity": Documentation for types of animation
  General_101: Documentation on animation types
  ODX: JMO frames
---
**Jointed Model Animation** (JMA) is an intermediate file format for animation in Halo. Unlike most file formats, there are different extensions, for different types of animation. The data written inside the file will be the same, regardless of the extension. The file has information about the animation data at the top, followed by the position, rotation, and scale of each node for each frame.

Animation can be exported as JMA using community-made tools, such as [Bluestreak](~) for 3ds Max, and the [Halo Asset Blender Development Toolset](~) for Blender. [Tool](~h1-tool) can convert animation exported as [FBX](~) to JMA. Animation data exported as JMA can be compiled into a [model_animations](~) tag for Halo 1, or into a [model_animation_graph](~h2/) tag for later games, such as Halo 2 and Halo 3.

JMA was used for first-person animation in Halo: Reach, but for everything else, Granny (GR2) was used instead. GR2 fully replaced JMA in Halo 4.

# Types of animation

## JMM
JMM is for base animation with no frame info. This is used for actions that do not need to move the object from one place to another, such as breathing when standing, entering a vehicle, and first-person animations.

## JMA
JMA is for base animation with frame info for change in X and Y coordinates of position. This is used for actions that move the object from one place to another, such as diving, running, and close range attacks that involve charging forward.

## JMT
JMT is like JMM with frame info for change in rotation around Z axis. This is used for turning left or right when standing, turning around when surprised, and so on.

## JMZ
JMZ is like JMT with frame info for change in Z coordinate of position. This is used for leaping to an elevated position.

## JMW
JMW is for base animation that relative to the world, at the origin of the level. This can be used for precise movement of a dropship in a specific level.

## JMO
JMO is for overlay animation added on top of base animation. This is used for aiming poses, recoil from firing a weapon, and rotation of the wheels on a car.

## First person weapon overlays
When the player moves and aims, the first person weapon model can react to give a sense of weight and realism. This overlay animation is used to define the default and extreme location and orientations of the weapon under movement and aiming.

| Frame number | Purpose  |
|--------------|----------|
| 0 | Default position
| 1 | Move forwards
| 2 | Move backwards
| 3 | Move right
| 4 | Move left
| 5 | Look left
| 6 | Look right
| 7 | Look up
| 8 | Look down
| 9 | Fully automatic firing

## JMR (Replacement)
JMR is for replacement animation that partially animates the object. This is used for reloading which involves only the nodes of the upper body, allowing the legs to be animated by a different animation
