# Pixel Crawler — AI-Ready Asset Kit for Sproutvale

This kit was generated from the supplied `Pixel Crawler - Free Pack (Exterior 0.1).zip`. It preserves the original PNG artwork and folder paths, and adds a machine-readable asset manifest plus visual contact sheets.

## What is included
- 181 original PNG assets, extracted without changing their pixel data.
- `asset_manifest.json`: exact relative path, image dimensions, color mode, file size, category, and available animation metadata read from matching Aseprite project files.
- `ANIMATION_AND_MOVEMENT_GUIDE.md`: practical movement-state mapping grounded in the animation filenames and source frame metadata.
- `PIXELFORK_BUILD_PROMPT.txt`: ready-to-paste instructions for an AI game builder.
- `previews/`: labeled contact sheets for visual inspection.

## Important facts about animation
The Body_A player animations use 64×64 frame cells. Their PNG strips are horizontal: frame count = image width / 64. Matching Aseprite project files report per-frame timing; where timing is 100 ms, use that as the initial playback duration. The PNG-only runtime assets are preserved; the original `.aseprite` editor project files are not copied into this compact kit.

Enemy and NPC sprite sheets are catalogued with their actual dimensions and their Idle/Run/Death/Walk filenames. Do not assume a universal frame size for these sheets: inspect the individual image and infer the grid from transparent cell boundaries or source metadata before slicing.

## Use in Pixelfork
Upload this ZIP if Pixelfork accepts ZIP asset imports. If it accepts only images, upload the relevant PNG sheets from `assets/`. Provide `PIXELFORK_BUILD_PROMPT.txt` as the build prompt and `ANIMATION_AND_MOVEMENT_GUIDE.md` as additional instructions.

## Integrity
All extracted PNG files are copied byte-for-byte from the supplied ZIP. No sprite has been recolored, resized, cropped, or re-encoded.
