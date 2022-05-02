# glvis

Introduction
glVIS is a PVS (Potentially Visible Set) builder specially designed to be used with OpenGL ports of the DOOM game engine. It extends the “GL-Friendly Nodes” with a new lump named “GL_PVS”. Basicly it’s a subsector accept table designed for rendering engine to skip the subsectors that can’t be visible from viewer’s subsector. PVS is added to both versions of “GL Nodes”.
Currently only Vavoom and ZDoomGL use PVS. Other source ports will ignore it.

Using glVIS
To create PVS for a wad file (e.g. FOO.WAD) simply type:

glvis foo.wad
glVIS first searches for a GWA file. If it finds it, it will work with it. Otherwise it assumes that wad file contains “GL Nodes” and will work with wad file. glVIS will save old file with extension .~gw for .gwa file or .~wa for a .wad file.

PVS specification
Loading PVS:

Check lump name, if it’s not “GL_PVS”, the data is missing. If you want, you can treat this case as empty lump, but you can also abort with error (like Vavoom does).
Check lump size, if it’s 0, it’s empty lump. In this case you allocate ((numsubsectors + 7) / 8) * numsubsectors bytes and fill them with 0xff.
Otherwise PVS data is present, simply load it.
Unlike REJECT, vis data for a subsector is aligned to byte. Offset in data for subsector i is ((numsubsectors + 7) / 8) * i. Usage is similar to that of REJECT. Example of checking if subsector i is visible:

     byte *vis_data;    // PVS lump
     int view_sub;      // Num of subsector, where the player is
     byte *vis;
     vis = vis_data + (((numsubsectors + 7) / 8) * view_sub);
     ...
     if (vis[i >> 3] & (1 << (i & 7)))
     {
         // Subsector is visible
         ...
     }
     else
     {
         // Subsector is not visible
         ...
     }
Status
The current version of glVIS is 1.6. It has been tested and known to work with numerous large wads, including Ultimate DOOM, DOOM II, Heretic and Hexen.
NOTE: glVIS is very expensive, complete vis data for a large wad can take several hours.
