import adult_1 from "./img/adult_1.png"
import adult_2 from "./img/adult_2.png"
import adult_3 from "./img/adult_3.png"
import adult_4 from "./img/adult_4.png"
import adult_5 from "./img/adult_5.png"
import adult_6 from "./img/adult_6.png"
import child_1 from "./img/child_1.png"
import child_2 from "./img/child_2.png"
import child_3 from "./img/child_3.png"
import child_4 from "./img/child_4.png"
import child_5 from "./img/child_5.png"
import child_6 from "./img/child_6.png"

# Modding Tips

## Setting Segment Calls in Replaced Models

Segment calls are an important feature for a number of special effects (Texture scrolling, Fading in/out etc) to function correctly when replacing models. These Segment settings are not retained upon export via the Fast64 plugin. The exported files can be manually repaired by restoring the missing lines to the XML.

Match your new model's material structure to the original - reference the imported original; failure to properly match may leave you with broken models - when done, open each exported material, and at the end, just before the line:
```xml
<EndDisplayList/>
```

add:
```xml
<CallDisplayList Path=">0xZZ000000"/>
```

Which segment value is required differs by model, and which is used by which can be found in this spreadsheet:
https://docs.google.com/spreadsheets/d/1U5ogFN-lUUkJ-yJVvMkO1eWF3l0zMaKI61qEOopbV4I/edit?gid=1210471824#gid=1210471824

The process has been automated with a terminal program for Windows and Linux, available here:
https://drive.google.com/drive/folders/1ho-0EbIEAO4CInZcMsVCfzQEJYvYKuSY
Windows: Drag and drop your Objects folder onto the executable.
Linux: Ensure the file is set as an executable, and run in a terminal from the location of the tool: `./Segment_Integrator_Linux "path/to/exported/objects/"`

Note: The tool only supports Objects as of this writing.
Objects with multiple Segment values are not supported by this tool; an error will display, prompting you to handle the files manually.

## Exploding "Body Break" Enemies

Modders seeking to replace enemies must be aware that to prevent vertex explosion on all enemies using the "body break" system will require the mesh to be designed to support it. This means all 'part' meshes should be skinned exclusively to one joint; you cannot skin one contiguous mesh surface across two or more joints that break apart. Joints that do not break apart can be skinned as normal.
Enemies that utilise the "body break" system:
- Iron Knuckles (en_ik)
- Stalfos (en_test)
- Stal Children (en_skb)
- Shell blade (en_sb)
- Tektites (en_tite)
- Bubbles (flying skulls) (en_bb)

Dev comments: https://github.com/HarbourMasters/Shipwright/pull/3436#issuecomment-2252886376

## Flipbooks

Of all aspects of custom model creation, the most finicky is the "flipbooks" - the swapping textures used for animating character faces. Link is obviously the most complicated, so here are the exact names and settings required. Note that the texture dimensions can differ from the ones shown in the screencaps, but must be set to the correct dimensions for your texture files.

<img src={adult_1} alt="Adult 1" width="300" />
<img src={adult_2} alt="Adult 2" width="300" />
<img src={adult_3} alt="Adult 3" width="300" />
<img src={adult_4} alt="Adult 4" width="300" />
<img src={adult_5} alt="Adult 5" width="300" />
<img src={adult_6} alt="Adult 6" width="300" />
<img src={child_1} alt="Child 1" width="300" />
<img src={child_2} alt="Child 2" width="300" />
<img src={child_3} alt="Child 3" width="300" />
<img src={child_4} alt="Child 4" width="300" />
<img src={child_5} alt="Child 5" width="300" />
<img src={child_6} alt="Child 6" width="300" />
