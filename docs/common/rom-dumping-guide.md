import image1 from "./image1.jpg"
import image2 from "./image2.jpg"

# ROM Dumping Guide

The below steps will explain how to obtain a ROM from your legally sourced, official GameCube disc.

:::note
Reminder: Obtaining the Official Game Disc is your responsibility.
:::

You can also get a supported ROM from an N64 cartridge, but that requires specialized hardware [like this](https://github.com/sanni/cartreader) and is beyond the scope of this guide.


## GCIE-PS
We have a script available to run in Windows Powershell (and Linux, if Powershell Core and .NET Core are installed) to make it easy. You can find it [here](https://github.com/Spodi/GCIE-PS). Use it after Step 1 below.
If you can't run that, then continue reading for a guide using Dolphin emulator.

## Requirements

- Softmodded [Wii](https://wii.guide/) or [Wii U](https://wiiu.hacks.guide/)
- GameCube disc of appropriate region, listed below (PAL = European)
- Windows PC (it could work on Linux/macOS, but is currently untested)
- [Dolphin](https://dolphin-emu.org/)

## Procedure

1. Dump your game disc using your modded Wii or Wii U. Transfer the dump to your PC. If using the GCIE script from above, use it now. Otherwise continue the guide.

  a. [Dumping Wii/Gamecube Games](https://wii.guide/dump-games.html) @ Wii Guide
  b. [Ripping Games](http://wiki.dolphin-emu.org/index.php?title=Ripping_Games) @ Dolphin Wiki

2. Add the dump to your Dolphin games folders. If you don't have any configured, add one in Options -> Configuration -> Paths. You will need this for the next step.

<img src={image1} alt="dump dolphin game list" width="1000" />

3. Right-click the game in the list, click Properties, and scroll the tabs to the right to find Filesystem. Find the appropriate TGC file listed below, and extract it into your Dolphin games directory.

<img src={image2} alt="dump dolphin filesystem" width="1000" />


Disc | Game | Filename
Ocarina of Time & Master Quest (PAL) | Ocarina of Time & Master Quest | files/zlj_f.tgc
Zelda Collector's Edition (PAL) | Ocarina of Time | files/tgc/zelda\_PAL\_093003.tgc
Zelda Collector's Edition (US) | Majora's Mask | files/tgc/majora\_ENG\_091003.tgc

4. Find the TGC file listed in Dolphin's game list, and go to its Filesystem properties like in step 3. Find the .n64 file in there, and extract it to your port folder.

- Ocarina of Time (PAL)  -> zlp\_f.n64
- OoT Master Quest (PAL)   -> urazlp\_f.n64
- Majora's Mask (US)    -> zelda2e.n64

5. Validate your dumped ROM using one of our verification sites
- For SoH, use [ship.equipment](https://ship.equipment/)
- For 2ship, use [2ship.equipment](https://2ship.equipment/)

6. Proceed with setting up your port as dictated in your Platform Guide.

