---
title: JMA
about: 'resource:Animation data'
keywords:
  - jma
  - animation
thanks:
  "Abstract Ingenuity": Documentation for types of animation, description of file format
  Ctrl-Alt-Destroy: Information about frame info for various types of animation
  Dagger: Information about versions 16390 and 16391
  General_101: Information about versions 16390 to 16935
  Krevil: Information about version 16395
---
**Jointed Model Animation** (JMA) is an intermediate file format for animation in Halo. Unlike most file formats, there are different extensions, for different types of animation. The data written inside the file when exporting will be the same. The file has information about the animation data at the top, and the position, rotation, and scale of each node for each frame. 

Animation can be exported as JMA using community-made tools, such as [Bluestreak](~) for 3ds Max, and the [Halo Asset Blender Development Toolset](~) for Blender. [Tool](~h1-tool) can convert animation exported as [FBX](~) to JMA. Tool compiles the source data into a tag for Halo, such as a [model_animations](~) tag for Halo 1, or into a [model_animation_graph](~h2/tags/model_animation_graph) tag for later games, such as Halo 2 and Halo 3. 

JMA was used for first-person animation in Halo: Reach, but Granny (GR2) was used instead for everything else. GR2 fully replaced JMA in Halo 4. 

# Types of animation

## JMM

JMM is for base animation with no frame info. 

This can be used for actions that do not physically move the object, which means the root of its skeleton stays in place. Such actions include breathing in and out when crouching or standing, entering and exiting vehicles, and being airborne after jumping or falling. 

First-person animation usually is exported as JMM. There are some exceptions. JMA can be used instead of JMM, but the frame info is not needed. The character seen outside of first-person view is what actually interacts with the world. 

When used as [animation relative to another object](~h2/engine/scripting#functions-custom-animation-relative), the action can physically move the object, but that normally would not be allowed. 

## JMA

JMA is for base animation with the frame info (dx, dy) for change in position on X axis, and on Y axis. 

This can be used for actions that physically move the object, from one place to another along the XY plane. Such actions include walking, running, rolling away, diving to the ground, and charging forward to hit a target at close range. 

This type of animation is also used for reaction to major damage, known as hard pings, which interrupt whatever the character was doing. 

## JMT

JMT is for base animation with the frame info (dx, dy, dyaw) which includes change in rotation around Z axis. 

This can be used for actions that physically turn the object, to face a different direction. Such actions include fully turning 90 degrees to the right or the left, and turning 180 degrees around to face the other way, when surprised by someone or something from behind. 

## JMZ

JMZ is for base animation with the frame info (dx, dy, dz, dyaw) which includes change in position on Z axis. 

This can be used for actions that physically move the object, to a different elevation in a specific way. Such actions include leaping up and out from a pit of a specific depth, and jumping upward to travel a specific distance, according to animation instead of physics. 

## JMW

JMW is for world-relative base animation with no frame info. 

World-relative animation is based on the origin at (0, 0, 0) in the world. If the animation moves the object forward from the position (1, 2, 3) the object will be moved to (1, 2, 3) in the level, and then move forward from there. 

This can be used for actions that precisely moves the object within a specific environment. One common use of world-relative animation is for [a dropship flying to specific places within the level](~h1/guides#animations), to drop off or pick up troops. In Halo 1, [recorded animations](~h1-sapien/recorded-animations) are also an option for dropships and other units. 

## JMO

JMO is for overlay animation. 

The transforms of each node may change or not change over time. The difference in transforms for each node, between the first frame and the later frames, will be added on top of the current transforms, from any animation already applied to the nodes. 

This can be used for reaction to acceleration when in a vehicle, recoil from firing a weapon, and reaction to minor damage, known as soft pings. Overlay animation is also used for doors, elevators, and vehicles, such as the cockpit of the Banshee, and the wheels of the Warthog. 

Pose overlay animation is a subtype of overlay animation. Unlike most types of animation, each frame is a "pose" that describes what an object looks like in a given state. For example, when a character aims in different directions, the body gets adjusted according to a set of poses. That set usually includes a pose for aiming 90 degrees to the right, a pose for aiming 90 degrees up, etc. The animation system handles the blending between those poses.

## JMR

JMR is for replacement animation. 

The transforms of each node may change or not change over time. Based on the difference in transforms, between the first frame and the later frames, some nodes will be animated, and others will not be animated. If the transforms of a node changes, it will be animated.

Nodes that are animated will use the transforms of the replacement animation, replacing the transforms from any animation already applied to them. The other nodes will not be affected, which means base animation and overlay animation will be applied normally to those nodes.

This can be used for actions that affect only the upper body of a character, or parts of a different type of object. Many actions for players are replacement animation, which includes hitting a target at close range, reloading a weapon, and throwing a grenade. 


## JMRX

JMRX was added in Halo 2, for replacement animation in local space instead of object space. 

This can be used to adjust the fingers of a character, to better hold a specific weapon. 

# File format

## Changelog

{% table %}
* Version number
* Notes
---
* 16390
* First version, only header and transforms
---
* 16391
* Added names for nodes after header
---
* 16392
* Added hierarchy for nodes, based on first child and first sibling of each node
---
* 16393
* 
---
* 16394
* Moved checksum from seventh line to second line of header, switched to hierarchy based on parent of each node, switched from local transforms to global transforms
---
* 16395
* Added animation data for biped control node, from root object for skeleton, not bones of skeleton
{% /table %}

## Version 16392

The first seven lines are for general information about the animation data. 

{% table %}
* Property
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

The bones of a skeleton for a model are nodes. They are listed after the header and before the transforms. 
An index corresponds to the position of a node in the list. For example, the index 1 refers to the second node. 

{% table %}
* Property
* Notes
---
* Name
* The length should not be more than 32. 
---
* First child index
* This is -1 for bones that have no children. 
---
* First sibiling index
* This is -1 for bones that have no siblings, and for bones that are the final sibling. 
{% /table %}

The tranforms are listed in order, by node and then by frame. The transforms of the first node are listed before the transforms of the second node. 
The transforms of all nodes for the first frame are listed before the transforms for the second frame. 

{% table %}
* Transform
* Notes
---
* Position
* This is represented by three numbers, in the order X, Y, and Z. 
---
* Rotation
* This is represented by four numbers of a quaternion, in the order X, Y, Z, and W. 
---
* Scale
* This is represented by one number, that applies to all three dimensions of X, Y, and Z. 
{% /table %}

## Version 16395

The first seven lines are for general information about the animation data. 

{% table %}
* Property
* Notes
---
* Version number
* [H2 Tool](~h2-tool) and [H3 Tool](~h3-tool) expect version 16395, 16934, or 16393. 
---
* Checksum
* This appears to be ignored when compiling animation data. The checksum in the tag is also different. 
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

The bones of a skeleton for a model are nodes. They are listed after the header and before the transforms. 
An index corresponds to the position of a node in the list. For example, the index 1 refers to the second node. 

{% table %}
* Property
* Notes
---
* Name
* The length should not be more than 32. 
---
* Parent index
* This is -1 for the root. The root should be the first node. 
{% /table %}

The tranforms are listed in order, by node and then by frame. The transforms of the first node are listed before the transforms of the second node. 
The transforms of all nodes for the first frame are listed before the transforms for the second frame. 

{% table %}
* Transform
* Notes
---
* Position
* This is represented by three numbers, in the order X, Y, and Z. 
---
* Rotation
* This is represented by four numbers of a quaternion, in the order X, Y, Z, and W. 
---
* Scale
* This is represented by one number, that applies to all three dimensions of X, Y, and Z. 
{% /table %}

The number 0 usually is written at the end of the file. That indiciates there is no animation data for the biped control node. 
Tool supposedly can process such data, but past attempts to include and use it did not lead to any noticeable results. 
