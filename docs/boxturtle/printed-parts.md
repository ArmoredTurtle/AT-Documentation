# Printed Parts

!!!warning

    This is NOT a [VORON Design](https://vorondesign.com) project, we strongly recommend that you run a
    profile that is specific to BoxTurtle. Voron parts profiles are not recommended for BoxTurtle printed parts, as the
    tolerance expectations are different.

Specifically, make sure you are tuning for skew and filament shrinkage. An excellent tool for this is
Vector3D's [Califlower Mk2](https://vector3d.shop/products/califlower-calibration-tool-mk2) (yes, it's worth the $14).
Do this *before* you print 1.5-2kg worth of filament for BoxTurtle parts!

If you purchased a kit, all the parts you need to print are located in
the [Base_Build](https://github.com/ArmoredTurtle/BoxTurtle/tree/main/STLs/Base_Build) folder under STLs on this
repository. There is also a [web-based configurator](https://armoredturtle.xyz/stl-configurator.html) available for easy
identification and download of what STLs to print outside a base kit build.

All parts are pre-oriented for printing and have all required support pre-built into the STLs. OrcaSlicer/BambuStudio (
and possibly others) may give warnings about certain parts having 'floating cantilevers', these are breakaway support
structures and these errors can safely be ignored.

If you have an LDO BoxTurtle kit, these are the options to pick on the web-based configurator:

- `Would you like normal or monolithic skirts?` - your choice. If you can fit monolithic on your printer, it has a
  more unified look vs the split parts. We recommend a 350mm minimum size bed for this, but you may be able to get them
  to squeak in on a ~300-305mm plate.
- `Would you prefer a hub or pass through skirts?` - Hub is our recommended default choice.
- `Are you using printed corners or extrusion corners?` - Printed corners
- `LED corners or plain?` - Plain
- `Are you using D2F or D2HW style switches?` - D2HW
- `Would you like to print the Hardware or No-Hardware version of the trays?` - Choose No Hardware if you have a
  minimum 235x235mm bed. Some Ender-style printers have a maximum 220x220mm printable volume, and so the 'bars' that
  slot into the extrusion require hardware (heat sets/screws) to attach to the tray.
- `What style tray would you like to print? Plain, Inlay, Multicolor, or Desiccant?` - Again, a bit of personal
  choice. Plain would be the base option, but there's no reason you couldn't print the others. An enclosure
  is [in the works](https://www.youtube.com/watch?v=Jjgi8q28Y2o) so if you wanted to future-proof for that, desiccant
  trays are available (in two sizes).
- `Would you like to print a TurtleNeck buffer?` - Yes

If all of the above sounds a bit overwhelming, you can print all the parts
in [LDO_BoxTurtle_Kit_STLs.zip,](https://github.com/ArmoredTurtle/BoxTurtle/tree/main/STLs/STLs/LDO_BoxTurtle_Kit_STLs.zip)
and they will be the right ones to build a functioning BoxTurtle.

For the 'No Hardware' trays, you may wish to enable 'Thick bridges' or use a 0.25mm first layer height, as otherwise the
built-in supports may generate 0.4mm gap between the support and the printed part. The N20 motor mounts may also benefit
from an 0.25mm first layer height.

There is a calibration print that you may like to print before getting
started [here](https://www.printables.com/model/1004303-box-turtle-calibration-fidget).

# How to create a meshed look on the skirts

There are a few ways to create a 'meshed' look on the hexes on the skirts, using your slicer.

- On the print settings profile, set top and bottom layers to 0. Adjust infill pattern, density and angle to suit your
  preference.
- You can also do a height range modifier on the part from 0-3.6mm to set top/bottom layers to 0, and infill pattern,
  density and angle to suit your preference.
- If you would like a different color for the hexes, you can do a color swap at layer 19 (3.8mm with 0.2mm layer
  heights).
- [SteveBuilds](https://youtube.com/@SteveBuilds) created a custom set of skirt options with modeled in hexes and
  multi-body parts, these can be
  found [here (multi-body)](https://github.com/ArmoredTurtle/BoxTurtle/tree/main/STLs/Option_Parts/Skirts/Multibody/)
  and [here (modeled hex meshes)](https://github.com/ArmoredTurtle/BoxTurtle/tree/main/STLs/Option_Parts/Skirts/Hex_Mesh/).
  To get a two-tone color look, perform a color change at 3.2mm (layer 16 at 0.2mm print heights).