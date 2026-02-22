# Technical Guides

Advanced technical information for Ship of Harkinian mod development.

## Fast64 Export and O2R Format

### Understanding the Resource Loading Pipeline

Resources in the ROM go through a multi-step process to be loaded into the game:

#### Step 1: ZAPD Extraction

ZAPD is used in combination with the ROM XMLs to pull resources out of the ROM and into memory as various resource types in `ZAPDTR/ZAPD/*`:
- `ZRoom` - Room/scene data
- `ZDisplayList` - Display lists
- `ZTextMM` - Text resources
- And many more...

#### Step 2: OTR Export

OTRExporter is used to write these ZAPD resources into the custom O2R format within the O2R file.

**Key Changes:**
- Sometimes the result is very similar to what was pulled from the ROM
- Sometimes it's changed significantly
- Anything that references another resource gets that reference replaced with either:
  - A string path to the resource, OR
  - A CRC64 hash of the path to that resource
- This replaces the pointer references that the ROM used

#### Step 3: Resource Factory Import

Resources are read from the O2R by resource factories located in `mm/2s2h/resource/importer/*`

These factories place resources into LUS objects that mostly reflect their in-game counterparts, with possible subtle differences.

#### Step 4: Game Code Usage

The game then loads these objects from LUS using calls like:
```cpp
std::static_pointer_cast<SOH::TextMM>(
    Ship::Context::GetInstance()->GetResourceManager()->LoadResource(filePath)
);
```

### Example: Text Resources End-to-End

Following a text resource through all stages:

1. **ZAPD:** `ZAPDTR/ZAPD/ZTextMM.cpp` - Parses raw ROM data into `ZTextMM`
2. **OTRExporter:** `OTRExporter/OTRExporter/TextMMExporter.cpp` - Converts `ZTextMM` into custom format stored in O2R
3. **Factory:** `mm/2s2h/resource/importer/TextMMFactory.cpp` - Reads from O2R into LUS's `SOH::ResourceType::TSH_TextMM`
4. **Game Code:** `mm/2s2h/z_message_OTR.cpp` - Loads data from LUS and uses it

### Creating Custom Resources

When creating tooling for custom resources:

**Requirements:**
- Export data in the EXACT format that the Resource Factory (Step 3) expects
- Match what OTRExporter (Step 2) would have written to the O2R
- Match types precisely (u8 vs s16, etc.)
- Match order exactly
- Include CRC64 hashes where the factory expects them

:::warning Critical
The format must match **perfectly**. Any mismatch in type, order, or structure will cause loading to fail.
:::

### Fast64 O2R Export

#### Working Diff for 2Ship Models

**DisplayLists, Materials, and Skeletons:**
- https://github.com/Yanis002/fast64/compare/mm_dev...garrettjoecox:fast64:main

**Full Scene Export (Work in Progress):**
- Exports geometry, DLs, and materials (not fully working yet)
- https://github.com/garrettjoecox/fast64/commit/7d2f984466444beecd3357db143931d665c55f45

#### Key Concepts

**Display Lists and Scenes:**
- These resources are quite complicated
- All handled in one resource export file in Step 2
- Requires careful study of the export process

**Reference Implementation:**
When implementing new export features, study these three components:
1. ZAPD resource class (how it's parsed from ROM)
2. OTRExporter for that resource (how it's written to O2R)
3. Resource Factory for that resource (how it's read from O2R)

## Advanced Modding Techniques

### Scene Mesh Replacement

Scene mesh replacement follows the typical DL replacement workflow, but requires manual path discovery.

**Finding DisplayList Paths:**
1. Use the GFX debugger to locate specific DLs
2. Look for `G_MARKER:` entries
   - Example: `G_MARKER: scenes/shared/spot01_scene/spot01_room_0DL_009A70`
3. This path is what you replace with your custom DL

**Limitations:**
- Only affects scene mesh geometry
- Does not affect collision
- Does not affect other scene properties (lighting, exits, etc.)

### Custom Asset Internal Paths

When exporting custom assets, internal path structure matters:

**Alt Equipment Example:**
- Export items to: `object/object_custom_equip`
- This allows Ship to handle hand merging and effects automatically
- See asset spreadsheet for all required DisplayLists

**Player Model FPS Hands:**
- `gCustomAdultFPSHandDL` - Adult Link first-person hands
- `gCustomChildFPSHandDL` - Child Link first-person hands
- Export these with player model mods for FPS item viewing

## Development Resources

### Source Code References

**Main Repository:**
- https://github.com/HarbourMasters/Shipwright

**Fast64 (Community Fork):**
- https://github.com/HarbourMasters/fast64

**2Ship2Harkinian:**
- https://github.com/HarbourMasters/2ship2harkinian

### Understanding the Codebase

**Key Directories:**
- `soh/` or `mm/` - Game-specific code
- `2s2h/resource/` - Resource factories and importers
- `OTRExporter/` - ROM to O2R conversion
- `ZAPDTR/ZAPD/` - ROM extraction and parsing

**Important Files:**
- `*Factory.cpp` - Resource loading from O2R
- `*Exporter.cpp` - Resource export to O2R
- `Z*.cpp` (in ZAPD) - ROM resource parsing

## Debugging Tips

### Using the GFX Debugger

The built-in GFX debugger is invaluable for:
- Finding DisplayList paths
- Understanding render order
- Debugging visual issues
- Identifying which assets are being used

**Workflow:**
1. Pause the game at the frame you want to inspect
2. Open GFX Debugger from the enhancements menu
3. Step through commands to find your target resource
4. Note the path from the G_MARKER

### Common Issues

**Export Format Mismatch:**
- Symptom: Crashes on loading a custom resource
- Cause: Data format doesn't match what factory expects
- Solution: Compare your export to OTRExporter output for that resource type

**Missing CRC64 Hash:**
- Symptom: Resource references fail to load
- Cause: String path used where CRC64 expected (or vice versa)
- Solution: Check factory code to see which format is expected

**Incorrect Segment Calls:**
- Symptom: Texture effects not working (scrolling, fading, etc.)
- Cause: Segment calls not preserved in export
- Solution: Manually add segment calls or use Segment Integrator tool

## Contributing to Development

Ship of Harkinian is open source and welcomes contributions:

1. **Fork the repository** on GitHub
2. **Create a feature branch** for your changes
3. **Test thoroughly** - ensure no regressions
4. **Submit a Pull Request** with clear description
5. **Engage with feedback** from maintainers

**Areas for Contribution:**
- Bug fixes
- New features and enhancements
- Documentation improvements
- Performance optimizations
- Platform-specific improvements

## Further Learning

For deep technical knowledge:
- Study the decomp repository to understand game internals
- Review merged PRs to see how features are implemented
- Join technical discussions in the Discord
- Experiment with small changes before attempting large features

:::tip
The best way to learn the system is to pick a simple resource type (like text or audio samples) and trace it through all four steps of the pipeline.
:::
