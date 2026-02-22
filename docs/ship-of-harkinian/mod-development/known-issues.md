# Known Issues

## Index out of range

Affects: Model import/export through Fast64 plugin.
Description: A significant number of DLs cannot be imported into Blender, reporting the error "Index out of range" when attempting to do so. A null object imports into the Blender scene. Attempting a blind export to affected DLs does not produce functional results.
Most affected DLs appear to be parented to joints or objects; this relationship is broken in a blind export.

Workarounds: None discovered.

## Base texture extraction and manifest generation failing in Retro

Affects: OTR/O2R texture extraction with current versions of Retro
Description: Attempting to unpack asset archives with the latest versions of Retro produces incomplete results; often just a font file, but sometimes an incomplete set of textures.

Workarounds: Retro 0.0.5 alpha does not appear to have this issue - using this version to unpack the archives, and the latest version to pack them, appears to be the best solution for now.

## Retro cannot process images with dots in the filename

Affects: Replacing the SD textures with HD versions in custom models
Description: Unpacking a custom model's mod archive and replacing the textures when the texture filenames include dots before the extension, those textures are not recognised as and do not produce usable images.
This would be avoidable, but newer versions of Fast64 forcibly include identifying format data between dots, making it impossible.

Workarounds: Must use an older version of Fast64, and avoid using texture filenames with multiple dots, or alter the exported texture and material files in a text editor to remove the extra dots and correct the data.
