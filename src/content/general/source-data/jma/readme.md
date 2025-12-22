---
title: JMA
stub: false
about: 'resource:Animation data'
keywords:
  - jma
  - animation
thanks:
  "Abstract Ingenuity": Documentation for types of animation
  General_101: Information about various versions
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

## First person weapon overlays To be relocated
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

## JMR
JMR is for replacement animation that partially animates the object. This is used for reloading which involves only the nodes of the upper body, allowing the legs to be animated by a different animation

# File format

## Changelog

{% table %}
* Version number
* Notes
---
* 16390
* initial version
---
* 16391
* Added node names
---
* 16392
* Added child/sibling skeleton hierarchy to nodes
---
* 16393
* ???
---
* 16394
* Switch from child/sibling to parent for skeleton hierarchy. Switch order of variables in intermediate file so that the file checksum is 2nd instead of last. Switch from local transforms to global transforms.
---
* 16395
* Add biped control node data (I don't actually know if anything in the game uses this. IIRC Tool had code to handle the data coming in from the biped controller data but I couldn't get it to do anything ingame. Biped controller data is animation data for the root skeleton object itself. Basically movement data on the armature instead of bones in Blender terms.)
{% /table %}

## Version 16392

The first seven lines are for general information about the animation data.

{% table %}
* Attribute
* Notes
---
* Version number
* [H1 Tool](~h1-tool) expects version 16392, 16391, or 16390.
---
* Frame count
* This should be greater than 0 and less than 2048.
---
* Frame rate
* This usually is set to 30 by default. Different values seem to have no effect on the result.
---
* Actor count
* This should be set to 1, no more and no less.
---
* Actor name
* This usually is set to "unnamedActor" by default. Different values seem to have no effect on the result.
---
* Node count
* This should be greater than 0 and [less than 64](~gbxmodel#nodes).
---
* Checksum
* This should match the checksum in the [gbxmodel](~) tag.
{% /table %}

Each bone of the skeleton involved in the animation is a node. Nodes are listed after the head and before the transforms.
An index corresponds to the position of a node in the list. For example, the index 1 refers to the second node.

{% table %}
* Attribute
* Notes
---
* Name
* No more than 32 characters, including prefix, such as `bip01 ` in `bip01 pelvis`
---
* First child index
* `-1` for bones that have no children
---
* First sibiling index
* `-1` for bones that have no siblings, and for final sibling
{% /table %}

The tranforms of each node are listed in order, by node and then by frame. For example, the transforms of the first frame are listed before the transforms of the second frame.

{% table %}
* Attribute
* Notes
---
* Position
* Three numbers in the order XYZ
---
* Rotation
* Four numbers of a quaternion in the order XYZW
---
* Scale
* Number that applies to all three dimensions of XYZ
{% /table %}

## Version 16395

The first seven lines are for general information about the animation data.

{% table %}
* Attribute
* Notes
---
* Version number
* [H2 Tool](~h2-tool) and [H3 Tool](~h3-tool) expect version 16395, 16934, or 16393.
---
* Checksum
* This appears to be ignored during compilation, and the checksum written in the tag is different.
---
* Frame count
* This should be greater than 0 and less than 2048.
---
* Frame rate
* This usually is set to 30 by default. Different values seem to have no effect on the result.
---
* Actor count
* This should be set to 1, no more and no less.
---
* Actor name
* This usually is set to "unnamedActor" by default. Different values seem to have no effect on the result.
---
* Node count
* This should be greater than 0 and less than 256.
{% /table %}

Each bone of the skeleton involved in the animation is a node. Nodes are listed after the header and before the transforms. An index corresponds to the position of a node in the list. For example, the index 1 refers to the second node.

{% table %}
* Attribute
* Notes
---
* Name
* No more than 32 characters, including prefix, such as `b_` in `b_pelvis`
---
* Parent index
* `-1` for the root, which should be the first node
{% /table %}

The tranforms of each node are listed in order, by node and then by frame. For example, the transforms of the first frame are listed before the transforms of the second frame.

{% table %}
* Attribute
* Notes
---
* Position
* Three numbers in the order XYZ
---
* Rotation
* Four numbers of a quaternion in the order XYZW
---
* Scale
* Number that applies to all three dimensions of XYZ
{% /table %}

The value 0 usually is written at the end of the file. That indicates there is no animation data for the biped controller. Such data appears to have no tangible effect when included.
