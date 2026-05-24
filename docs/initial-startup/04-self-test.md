## First self-test

AFC will run a self-test automatically on startup, this is called the PREP routine. Select Unit type below to see how your unit runs during PREP

=== "BoxTurtle"

    The self-test will run through and activate the respoolers on each lane from lane1 to lane4 and will update the lightbox LED indicating status for each lane.

    After the self-test completes, you hopefully will see a picture of a happy turtle in the console log (NOTE: an upright
    turtle is a happy turtle)! If you get a picture of an error turtle (upside down), you may have a misconfigured AFC
    setting, broken pin or some other issue that needs attention.

=== "HTLF"

    The self-test will first home to home sensor and then run through selecting each lane from lane1 to lane4 and will update the LED indicating status for each lane.
    
    After the self-test completes, you hopefully will see a `HTLF Ready` print out in the console log. If you get `HTLF Not Ready`, you may have a misconfigured AFC
    setting, broken pin or some other issue that needs attention.

=== "ViViD"

    The self-test will run through each lane from lane1 to lane2 selecting them and if filament is loaded into a lane AFC will try and feed to the load sensor to verify that the filament is loaded. AFC will also update the LED indicating status for each lane.

    After the self-test completes, you hopefully will see a `ViViD Ready` print out in the console log. If you get `ViViD Not Ready`, you may have a misconfigured AFC
    setting, broken pin or some other issue that needs attention.

=== "EMU"

    The self-test runs through all lanes configured, and updates LED indicating status for each lane.

    After the self-test completes, you hopefully will see a `EMU Ready` print out in the console log. If you get `EMU Not Ready`, you may have a misconfigured AFC
    setting, broken pin or some other issue that needs attention.

The default status indicators are:

- Red: No filament loaded/detected at extruder sensor
- Green: Filament loaded to lane extruder sensor
- Blue: Filament loaded to toolhead
- White: Filament in process of being loaded

If the colors are different between LEDs, or if loading one changes the colors of others, your neopixel
`color_order` (found in your unit-specific config file, e.g., `AFC/AFC_Turtle_1.cfg` for BoxTurtle) is incorrect. Try
changing from the default of `GRBW` to `GRB` to start, those are the two most common options.

You can display the status of sensors in Mainsail/Fluidd as regular filament switches by setting
`enable_sensors_in_gui: True` either globally in `AFC/AFC.cfg` or in the individual component section (e.g.
extruders in `AFC/Turtle_1.cfg`).

If you are unable to resolve the error after visiting
the [troubleshooting guide](../troubleshooting/troubleshooting.md),
you can get support from the community on the Armored Turtle discord by opening a help thread (run the Discord command
``/help``) to learn how).