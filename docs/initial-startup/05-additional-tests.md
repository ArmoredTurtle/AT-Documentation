## Further tests

=== "BoxTurtle"
    
    ### Respoolers

    Run the ``TEST`` command against each lane (one at a time) to verify proper respooler operation:

    - `TEST LANE=lane1`
    - `TEST LANE=lane2`
    - `TEST LANE=lane3`
    - `TEST LANE=lane4`

    Verify that each respooler works properly by moving in reverse like respooling a spool before proceeding.

    If your respoolers are operating in reverse (typically seen in a Formbot kit), you can simply reverse the pin assignments
    for the respoolers in `AFC_Turtle_1.cfg` file.

    e.g.

    ```cfg
    afc_motor_rwd: Turtle_1:MOT1_RWD
    afc_motor_fwd: Turtle_1:MOT1_FWD
    ```

    to
    ```cfg
    afc_motor_rwd: Turtle_1:MOT1_FWD
    afc_motor_fwd: Turtle_1:MOT1_RWD
    ```

    ### Trigger switches
    
    Actuating the trigger switch should begin pulsing that lane's extruder motor to load filament. Verify that the switch
    being actuated is activating that same lane's extruder motor. Between activating each trigger please allow 1-2 seconds
    before activating the next trigger.
    
    Once all lanes are confirmed to be activating the correct extruder when the trigger switch is actuated, move on to
    the next step.
    
    ### Extruders
    
    Insert filament into the feeder tube (it helps to cut it at an angle) and press through until the extruder motor gears
    catch the filament and load it further. If you can press the filament through, but it feels like the extruder motor is
    pushing back on the filament instead of pulling it in, try reversing the `dir_pin` setting for that extruder motor in
    your unit-specific config file (e.g., `AFC/AFC_Turtle_1.cfg` for BoxTurtle).
    
    If you are able to load filament into all lanes and get a green LED indicator, and the console
    reports no errors, move on to the next step.
=== "HTLF"

    ### Lane selection

    Run ``AFC_SELECT_LANE`` command against each lane (once at a time) to verify proper lane selection operation:

    - `AFC_SELECT_LANE LANE=lane1`
    - `AFC_SELECT_LANE LANE=lane2`
    - `AFC_SELECT_LANE LANE=lane3`
    - `AFC_SELECT_LANE LANE=lane4`

    ### Homing Unit

    Run ``AFC_HOME_UNIT`` command to verify that unit moves selector back to home pin:

    - `AFC_HOME_UNIT UNIT=HTLF_1`

=== "ViViD"

    ### Lane selection

    Run ``AFC_SELECT_LANE`` command against each lane (once at a time) to verify proper lane selection operation:

    - `AFC_SELECT_LANE LANE=lane1`
    - `AFC_SELECT_LANE LANE=lane2`
    - `AFC_SELECT_LANE LANE=lane3`
    - `AFC_SELECT_LANE LANE=lane4`

### Buffer

Test that your buffer is configured correctly by extending the slide all the way out, then run
`QUERY_BUFFER BUFFER=Turtle_1`. This should return `Trailing (buffer is compressing)`. Collapse the slide all the way so it
triggers the
switch at the rear, then rerun the QUERY_BUFFER command. It should then report `Advancing (buffer is expanding)`.

To help understand the terminology:

- Once the buffer fully expands, your AFC unit will reduce the speed of its stepper and the buffer will begin to compress.
- Once the buffer fully compresses, your AFC unit will increase the speed of its stepper and the buffer will begin to expand.

!!! note
    The buffer name in the `QUERY_BUFFER` command corresponds to the name configured during installation.
    For BoxTurtle, this is typically `Turtle_1`. Check your unit-specific config file for the correct buffer name.

Confirm proper operation of your buffer before proceeding.