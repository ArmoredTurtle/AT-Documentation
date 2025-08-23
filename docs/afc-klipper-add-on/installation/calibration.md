# Armored Turtle Automated Filament Control (AFC) Calibration

## Guided Calibration

`AFC_CALIBRATION` starts the process of a guided calibration process. Prompts will show in the GUI that will walk you
through the calibration steps.

## Manual Calibration

The function `CALIBRATE_AFC` can be called in the console to calibrate distances.  

!!! note 
    Distances will be calibrated to have ~1 short move after the move distance

!!! warning
    If using Turtleneck buffer please hold shut until filament reaches toolhead, once buffer start expanding
    slowly release. Doing this will keep the calibration from falsely triggering before fully reaching toolhead. Also pay
    attention and make sure the neck is not fully extended and triggering the advance sensor.

### Definitions

- `dist_hub` for each lane is the distance from the load switch at the extruder to the hub.
- `afc_bowden_length` is the distance from the hub to the toolhead sensor.

### Usage

`CALIBRATE_AFC LANE=<lane> DISTANCE=<distance> TOLERANCE=<tolerance> BOWDEN=<lane> UNIT=<unit>`  

!!! note
    `DISTANCE` and `TOLERANCE` are optional. default distance 25mm, default tolerance 5mm. 
    `UNIT` can be used to calibrate all lanes in one unit

To calibrate all lanes and the bowden length all at once:
  
- `CALIBRATE_AFC LANE=all BOWDEN=<lane>` input which lane to be used to check `afc_bowden_length`

To calibrate individual lanes:
    
- `CALIBRATE_AFC LANE=<lane>` input the lane you would like to calibrate

To calibrate just the bowden length:
 
- `CALIBRATE_AFC BOWDEN=<lane>` input which lane to be used to check `afc_bowden_length`

## Lane Testing

Once all other calibration is complete, an automated lane test can be run to verify all lanes are calibrated correctly.

Simple run `AFC_TEST_LANES` in the console to start the test. This will prompt you to select either a single lane,
or all lanes to be tested. The test will run through the selected lane(s) with a configurable amount of iterations.

For example, if you select all lanes with 3 iterations, it will randomly choose lanes and load / unload each lane 3 times.