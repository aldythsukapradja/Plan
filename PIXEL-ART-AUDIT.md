# Swordbound Pixel-Art Audit

## Scope

- Audited 204 embedded animation frames across hero, slime, red slime, goblin, goblin brute, skeleton, bat, and boss banks.
- Audited the embedded 1448x1086 lava concept sheet and every external biome reference.
- Verified sprite transparency, canvas dimensions, opaque bounds, bottom anchors, horizontal centers, frame availability, draw aspect ratios, camera shake, and background scaling.

## Findings and repairs

### Slime and red-slime banks

- Multiple frames contained isolated debris copied into the upper-right corner.
- Removed 1,956 pixels belonging to detached, non-animation fragments.
- Preserved intentional squash, bounce, splatter, and death debris.

### Boss bank

- 35 of 50 frames contained long opaque horizontal or vertical scan-line artifacts.
- Removed 11,141 verified scan-line pixels while preserving detached fire arcs, weapon trails, impact fragments, and death particles.
- Corrected the runtime destination from `160x188` to `188x150`, restoring the 150x120 source aspect ratio.
- Corrected the shared bottom-center anchor so feet and attack poses remain attached to the collision surface.

### Other monster banks

- Goblin, goblin brute, skeleton, and bat banks had consistent transparency and usable frame sequences.
- Their source/destination aspect ratios were already within approximately two percent and were retained.
- Bat anchor lift was reduced so the sprite no longer floats excessively above its collision box.

### Hero bank

- All 21 frames use a stable 240x240 canvas, eight-pixel bottom padding, and a centered opaque footprint.
- All 21 frames contained a pale neutral matte along transparency-facing edges.
- Recolored 21,484 verified exterior halo pixels to the game's dark outline color.
- Preserved interior whites, eye highlights, sword blades, and slash arcs; attack effects now carry a clean dark exterior contour.

### Pixel stability

- Screen shake is now rounded to whole canvas pixels, preventing fractional-frame shimmer.
- Enemy death alpha uses each species' real death duration instead of a fixed 26-frame divisor.
- Canvas image smoothing remains disabled and all sprite destinations use integer coordinates.

## Background repair

- Replaced the missing Forest, Crystal Cave, Desert, Castle, Volcano, and Sky Island files.
- Cropped the lava arena from the clean upper concept panel; removed the title strip and the entire lower HUD/mockup panel.
- All biome backgrounds are exactly 960x430 RGB PNGs.
- Each uses a controlled 192-color palette.
- Every background passes a strict 2x2 nearest-neighbor grid test.
- Backgrounds render at their intended aspect ratio without being stretched across the 1,900-pixel world width.

## Verification

- Both JavaScript blocks compile successfully under Node.js 24.
- All 204 embedded frames load in the runtime harness.
- All six current stages complete a render pass with their expected background source.
- The external volcano arena loads successfully for both Infernal Fortress and Cursed Warlord.
- The original HTML remains unchanged; `Swordbound-Pixel-Perfect.html` is the repaired build.

## Generated background art

The five missing scenic backgrounds were generated with the built-in image-generation workflow using the existing lava arena as a style reference. They were then cropped, palette-quantized, and nearest-neighbor resampled locally. The volcano background was salvaged from the original concept sheet rather than regenerated.
