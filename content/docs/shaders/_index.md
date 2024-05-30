---
weight: 14
bookFlatSection: true
title: "Shaders"
---

# Shaders

There are a number of shaders included in the SDK that LSD:R uses.

You'll notice there's identical 'Classic' and 'Revamped' shaders - this is to support switching between Classic and Revamped graphics modes on the fly in LSD:R. The Classic shaders have affine texture mapping and vertex snapping designed to emulate PlayStation 1 hardware. These features are not present on the Revamped shaders, thus they are essentially 'normal' shaders.

If you want your assets to support switching between Classic and Revamped graphics, you should use Classic/Revamped shaders in your materials and ensure any renderer objects have a [ShaderSetter](../entities/#shadersetter) component added (it switches the shaders between their Classic and Revamped modes).

Also notice that there are 'Set' shaders (i.e. ClassicDiffuseSet and RevampedDiffuseSet) with 4 textures - these are to support the different texture sets from the original game. If you want your assets to support this too you'll need to use these 'Set' shaders.

## Classic

Classic shaders have affine texture mapping and vertex snapping designed to emulate PlayStation 1 rendering.

### ClassicDiffuse

![ClassicDiffuse shader](/img/shaders/ClassicDiffuse.png)

ClassicDiffuse is used for displaying diffuse textures with optional alpha cutout transparency.

### ClassicDiffuseAlphaBlend

![ClassicDiffuseAlphaBlend shader](/img/shaders/ClassicDiffuseAlphaBlend.png)

ClassicDiffuseAlphaBlend is used for displaying diffuse textures with alpha blending. If your texture does not need blending then use [ClassicDiffuse](#classicdiffuse).

### ClassicDiffuseSet

![ClassicDiffuseSet shader](/img/shaders/ClassicDiffuseSet.png)

ClassicDiffuseSet is the same as [ClassicDiffuse](#classicdiffuse) except it supports texture sets.

### ClassicDiffuseSetAlphaBlend

![ClassicDiffuseSetAlphaBlend shader](/img/shaders/ClassicDiffuseSetAlphaBlend.png)

ClassicDiffuseSetAlphaBlend is the same as [ClassicDiffuseAlphaBlend](#classicdiffusealphablend) except it supports texture sets. If your texture does not need blending then use [ClassicDiffuseSet](#classicdiffuseset).

### ClassicWater

![ClassicWater shader](/img/shaders/ClassicWater.png)

ClassicWater emulates how the water from the original game worked - it's a palette cycling shader that uses a greyscale water map and a palette texture, cycling through the palette for each pixel. The water map should map to the palette by the intensity of the pixels in the red channel.

It only supports texture sets. If you don't need to support texture sets use the same textures for A, B, C, and D.

## Revamped

Revamped shaders are just regular shaders with no extra effects.

### RevampedDiffuse

![RevampedDiffuse shader](/img/shaders/RevampedDiffuse.png)

RevampedDiffuse is used for displaying diffuse textures with optional alpha cutout transparency.

### RevampedDiffuseAlphaBlend

![RevampedDiffuseAlphaBlend shader](/img/shaders/RevampedDiffuseAlphaBlend.png)

RevampedDiffuseAlphaBlend is used for displaying diffuse textures with alpha blending. If your texture does not need blending then use [RevampedDiffuse](#revampeddiffuse).

### RevampedDiffuseSet

![RevampedDiffuseSet shader](/img/shaders/RevampedDiffuseSet.png)

RevampedDiffuseSet is the same as [RevampedDiffuse](#revampeddiffuse) except it supports texture sets.

### RevampedDiffuseSetAlphaBlend

![RevampedDiffuseSetAlphaBlend shader](/img/shaders/RevampedDiffuseSetAlphaBlend.png)

RevampedDiffuseSetAlphaBlend is the same as [RevampedDiffuseAlphaBlend](#revampeddiffusealphablend) except it supports texture sets. If your texture does not need blending then use [RevampedDiffuseSet](#revampeddiffuseset).

### RevampedWater

![RevampedWater shader](/img/shaders/RevampedWater.png)

RevampedWater emulates how the water from the original game worked - it's a palette cycling shader that uses a greyscale water map and a palette texture, cycling through the palette for each pixel. The water map should map to the palette by the intensity of the pixels in the red channel.

It only supports texture sets. If you don't need to support texture sets use the same textures for A, B, C, and D.

## Other

These are other shaders that are used in-game (that don't need to be swapped out for Classic/Revamped graphics).

### Background

![Background shader](/img/shaders/Background.png)

Background is used for objects that should show in the sky (like the sun, and the sunburst effect). Objects will be rendered as billboards. The shader is set up so that the rendering should work correctly.

You can adjust the scale, tint, alpha cutoff, and rotation angle of the object.
