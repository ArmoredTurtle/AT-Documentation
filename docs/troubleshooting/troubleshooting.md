# Troubleshooting

## Known Issues

As with any project, there are some known issues that may arise. Known issues for the AFC-Klipper-Add-On project can 
be found [here](https://github.com/ArmoredTurtle/AFC-Klipper-Add-On/issues?q=is%3Aissue%20state%3Aopen%20label%3Abug).

We encourage anyone who experiences an issue to check the known issues list first, and if the issue is not listed, please
open a new issue on the GitHub repository. When opening a new issue, please provide as much detail as possible,
following the issue template provided. This will help us to quickly identify and resolve the issue.


## AFC Debug Script

A debug script is available to be run that can assist the Armored Turtle support team on their Discord server. To run 
this script, simply use the following commands on your SBC:

```bash
cd ~/AFC-Klipper-Add-On/troubleshooting
./afc-debug.sh
```

This script will collect some basic information about your system to include Klipper logs, device information, CAN 
network information, and your AFC configuration. This information will be uploaded to termbin.com, and the resulting 
link can be used to share this information with the Armored Turtle support team.

## Common Issues

### `TMC 'AFC_stepper lane1' reports error: GSTAT:      00000001 reset=1(Reset)`

This may be due to static building up in the system from the PTFE tubing in dry/low humidity environments. Many users
have found that running a ground wire from the stepper motor screws to a common GND pin on the AFC-Lite board, resolves
this issue. One example way to do this is below:

For each motor:

- Replace the M3x8 extruder motor mount screw with a M3x12 screw.
- Crimp a ring connector on a wire. Place a m3 washer over the now extended motor mount screw, followed by the ring
terminal, followed by an M3 hex or nyloc nut.

Join the wires from all the motors into a 5 port [WAGO 221-415](https://www.wago.com/us/wire-splicing-connectors/compact-splicing-connector/p/221-415). In the 5th port, run a wire from the WAGO to any spare
ground pin on the AFC-Lite board (e.g., the GND pin on RGB2).

### LEDs not displaying correct colors

If your leds are not displaying the correct color update the following value under your AFC_led section in 
`~/printer_data/config/AFC/AFC_Turtle_(n).cfg` file.

- color_order: change to match the color order for you leds. Different color orders are: RGB, RGBW, GRB, GRBW

### Filament pulling past extruder during unloads

During unloads if your filament retracts too much and goes past the lanes extruder then decrease your 
`afc_bowden_length` value in the `[AFC_hub <hub_name]` portion of the `~/printer_data/config/AFC/AFC_Turtle_(n).cfg` 
file. 

### Layer shift when using the cut macro

If you notice a layer shift occurs while using the cut macro, setting a higher stepper current while cutting has shown 
to help with this. Update and uncomment the following values in `~/printer_data/config/AFC/AFC_Macro_Vars.cfg` file.

- `variable_cut_current_stepper_x` - start with ~1.7-1.8A
- `variable_cut_current_stepper_y` - start with ~1.7-1.8A
- Only needed if cutting action is along the z - `variable_cut_current_stepper_z`

- Make sure your stepper names are updated for variables: `variable_cut_current_stepper_x`, 
- `variable_cut_current_stepper_y`, and `variable_cut_current_stepper_z`.

## General Unreliability

### Timer Too Close
AFC can require more resources than some SBCs are able to support with the stock settings. If you are encountering 
Timer Too Close (TTC) errors, the following may help resolve it:

- Operate the AFC-Lite in CAN bus mode instead of USB mode. USB bandwidth can be limited, especially when also running 
  cameras on the same bus.
- Reduce your `long_moves_speed` or `long_moves_accel` values in `AFC/AFC.cfg` from the defaults.
- Try disabling unnecessary services on the SBC, such as Crowsnest or KlipperScreen and see if that resolves the 
  problem.

### Timer too close (TTC) error

If you keep getting TTC errors start by adding the following to `~/printer_data/config/AFC/AFC.cfg` file under the 
[AFC] section.

`trsync_update: True`

### Lost communication with MCU
This usually indicates some sort of cabling or power delivery problem. First, double check that all crimps for the 
connectors for power and data are secure, even if it "looks" ok it may have a weak connection. Don't spend days 
troubleshooting problems when you can re-crimp the connectors and test in a half hour.

Powering the SBC via your MCU (e.g., on a Leviathan) may also not provide sufficient power to the device. Rule out
this as a cause by running separate power to the device.  If you are sharing multiple AC to DC PSUs, 
ensure that the V- wires are connected together for a common reference voltage.

## Flashing Issues or Errors

If you are having issues flashing the AFC-Lite, please ensure that you are using the correct configuration for your device.

### Error sending command: [SEND_BLOCK] to Device

The easiest way to resolve this issue is to revert to a known commit in Katapult, rebuild the katapult firmware, and then
reflash the katapult firmware to the AFC-Lite.

```shell
cd ~/katapult
git checkout e1716657
make clean
make menuconfig
make
```

Once the new katapult firmware is built, you can flash it to the AFC-Lite, and then proceed with flashing Klipper as normal.
