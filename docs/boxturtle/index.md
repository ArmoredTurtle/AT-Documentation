# BoxTurtle V1.0

BoxTurtle Automated Filament Control (requires AFC-Klipper-Add-On)

![BT_Render](../assets/images/boxturtle-render.png)


BoxTurtle is exactly what it appears to be — an open-source, AMS-style filament changer
for [Klipper](https://klipper3d.org) machines.
BoxTurtle requires the AFC-Klipper Add-On (found [here](https://github.com/ArmoredTurtle/AFC-Klipper-Add-On)).
The goal of BoxTurtle is to deliver a user experience as close to an AMS as possible in vanilla Klipper, i.e., an "AMS"
for any klipperized printer regardless of form factor but [VORON Design](https://vorondesign.com) printers in
particular.

If you appreciate the work we are doing, you can support
us [here](https://www.paypal.com/donate/?hosted_button_id=J4WRJBYGFYHLE).

## How it works

BoxTurtle is a lane based automated filament changing system. Each lane is equipped with its own dedicated extruder that
moves filament to and from the tool head independently. Spool management is done with low drag, top fed electric
respoolers.

Each lane merges into a hub (combiner) equipped with a sensor and a single outlet that goes to the toolhead.

To accommodate any differences in rotation distance between the extruder in the tool head and the lane motors, BoxTurtle
uses a toolhead buffer, like [TurtleNeck](https://github.com/ArmoredTurtle/TurtleNeck) by ArmoredTurtle.

For best results, we recommend the AFC-Lite or AFC-Pro, developed by [Isik's Tech @xbst](https://github.com/xbst/AFC-Lite/) as it
has the necessary sensor ports and DC brushed motor drivers used for BoxTurtle's electric respoolers.

## For best results

### AFC Klipper Add-On

BoxTurtle was designed in tandem to work with
the [AFC Klipper Add-On](https://github.com/ArmoredTurtle/AFC-Klipper-Add-On). For information on installing and
configuring the AFC Klipper Add-On, please visit [this section](../afc-klipper-add-on/installation/getting-started.md). 
Most common
configuration options are prompted for during the installation process. Some of the options presented are covered below
at a high level.

The AFC Klipper Add-On requires root access to your SBC running Klipper and the ability to install/modify the
installation. Additionally, the minimum version for Klipper/Kalico is 0.12.0 and the minimum version for the klippy-env
Python installation is > 3.8.

### Filament cutter or tip forming?

BoxTurtle works most reliably with a toolhead filament cutter. Our recommended toolhead cutter solution
is [FilamATrix](https://github.com/thunderkeys/FilamATrix). Please visit that GitHub repository for the most up-to-date
instructions and information. Ensure that toolhead cutting is selected when installing the AFC Klipper Add-On, or
enable it manually by editing `AFC/AFC.cfg` and restarting Klipper.

If a toolhead filament cutter is not a possibility, tip forming is also an option. This is enabled via the installation
script or by editing `AFC/AFC.cfg` and restarting Klipper. Tip forming is extremely dependent on your printer,
filament and even environment and guidance on properly tuning this is outside the scope of this document. Please visit
the [ArmoredTurtle Discord](https://discord.gg/eT8zc3bvPR) for community support on tip forming.

### Filament sensor

A pre-extruder toolhead filament sensor is recommended but not required if using
a [TurtleNeck](https://github.com/ArmoredTurtle/TurtleNeck)
or [TurtleNeck 2](https://github.com/ArmoredTurtle/TurtleNeck2.0) filament buffer. A post-extruder filament sensor can
be defined in the AFC configurations, but is not currently used beyond reporting status of the sensor.

Options include:

- [FilamATrix](https://github.com/thunderkeys/FilamATrix)
- [Filatector](https://github.com/ArmoredTurtle/Filatector)
- [AFC Buffer Ram Sensor using TurtleNeck](../afc-klipper-add-on/installation/buffer-ram-sensor.md)

### Waste management

#### Pooping, Kicking, and Wiping?

One option for filament purge/waste can be to have the printer 'poop' blobs of filament on the bed, have the nozzle
cleaned with a brush/wipe, and then return the toolhead near the blob and kick off the bed using kinematics into a
bucket or other waste evacuation system. This works similar to the method used
by [Blobifier](https://github.com/Dendrowen/Blobifier), but does not require servo actuation. Enabling poop/kick/wipe
macros can result in more reliable filament changes and color separation, at the cost of additional time during each
filament swap. Each option can be enabled or disabled during the AFC installation process, or by editing `AFC/AFC.cfg`
and restarting Klipper.

#### Prime towers
Filament prime towers can be used standalone, or in combination with the above to minimize the chance that blobs or
oozes from the filament change process make it onto your final print. However, if you are using pooping as described
above, you likely need far less purging than the default slicer settings suggest. You can adjust the 'flushing volumes'
in most slicers, as well as the size of your prime/purge tower. A decent starting setting for these might be a flushing
volume multiplier of 0.1-0.2 and a tower width size of 20mm.

If you want to rely solely on the poop purge, a static size wipe tower can be set by disabling the 'Purge into Prime
Tower' setting in your slicer software's "Printer Settings".

Different filament color transitions will require different purge volumes, and thus tuning this is highly dependent on
your filaments and prints you are performing.

## Enclosure
The enclosure option for BoxTurtle has been moved to its [own repository](https://github.com/ArmoredTurtle/BoxTurtle-Enclosure). The enclosure is
available at [LDO resellers](./vendors.md) worldwide and additional information can be found in its GitHub repository.
Also consider [joining the ArmoredTurtle Discord](https://discord.gg/eT8zc3bvPR) to get progress updates as development progresses.

## Errata
Identified known issues and potential workarounds are documented in the [errata section](./errata.md).

## Manual
ArmoredTurtle's project manuals are hosted [here](https://armoredturtle.xyz). There is no PDF, this is intended to make
the build experience as easy as possible.
[BoxTurtle Assembly Manual](https://armoredturtle.xyz/manual-sections.html?manual=boxturtle)

## PTFE sizes/lengths
!!! note
      PTFE lengths from extruder to hub are suggested starting lengths - you may need to trim a little bit off in
      order for it to have a smooth bend. The filament path should be a smooth curve without tight corners.

| Component                     | Outer Diameter | Inner Diameter | Length          | Notes                                                                   |
|-------------------------------|----------------|----------------|-----------------|-------------------------------------------------------------------------|
| Feeders to Trigger            | 4mm            | 3mm            | 50mm            | Quantity 4 ; use PTFE cutter jig                                        |
| Trigger to Extruder           | 4mm            | 3mm            | 80mm            | Quantity 4 ; use PTFE cutter jig                                        |
| Extruder to Hub (Outer lanes) | 4mm            | 2mm            | 171mm           | Quantity 2; 3mm ID will blow out. A chamfer is recommended on each end. |
| Extruder to Hub (Inner lanes) | 4mm            | 2mm            | 101mm           | Quantity 2; 3mm ID will blow out. A chamfer is recommended on each end. |
| Hub to Turtleneck             | 4mm            | 2mm            | build dependent | A chamfer is recommended on each end.                                   |
| Turtleneck to toolhead        | 4mm            | 2-3mm          | build dependent | Existing bowden tube is fine, but tighter ID can also be used.          |

## Initial startup guide
Please refer to the [Initial Startup Guide](../initial-startup/01-overview.md) after completing the
assembly of your
BoxTurtle for next
steps.

## Optional Accessories
Some optional printed accessories for your BoxTurtle build can be found on
the [AFC-Accessories](https://github.com/ArmoredTurtle/AFC-Accessories) repository.

## Merch
- BoxTurtle T-Shirt (sweatshirt/hoodie variants also available) ($3 from each sale goes to support ArmoredTurtle)
  via [Cotton Bureau](https://cottonbureau.com/p/QKF5XC/shirt/colored-box-turtle#/26921844/tee-men-premium-lightweight-vintage-black-tri-blend-s)

- BoxTurtle Sticker via [Dr. Mursey](https://drmursey.myshopify.com/products/box-turtle)