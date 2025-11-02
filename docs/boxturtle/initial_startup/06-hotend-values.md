## Setting up hotend specific values

Some of the values with AFC are extremely dependent on your hotend.

Suggested starting values for hotend types (more to be added later) are below.  `tool_stn` and `tool_stn_unload` 
are in `AFC/AFC_Hardware.cfg`, `variable_retract_length` and `variable_pushback_length` are in
`AFC/AFC_Macro_Vars.cfg`. For `tool_stn`, if you have `pin_tool_end` defined, use the second value; otherwise, use
the first value. You may need to increase this value if you are using a ram buffer as the toolhead sensor.

Additional instructions on tuning some of these values can be [here](../../afc-klipper-add-on/toolhead/calculation.md).  

Below is an example diagram of a Revo Voron hotend on FilamATrix/Clockwork 2:

![type:video](../../assets/videos/AFC_CUT_Explainer.mp4)
!!!warning

    These values are derived from community based feedback and are not guaranteed to work for your specific setup.
    You may need to adjust them based on your specific hotend and extruder setup. Always test with caution.

### Hotend specific values

#### Stealthburner & CW2
=== "Revo Voron"

    - `tool_stn`: 52 (if `pin_tool_end` is NOT defined) / 29 (if `pin_tool_end` is defined)
    - `tool_stn_unload`: 62
    - `variable_retract_length`: 22
    - `variable_pushback_length`: 20

=== "Rapido HF"

    - `tool_stn`: 64.1 (if `pin_tool_end` is NOT defined) / 41.1 (if `pin_tool_end` is defined)
    - `tool_stn_unload`: 105.9
    - `variable_retract_length`: 10
    - `variable_pushback_length`: 10

=== "Rapido V2 HF"
    
    - `tool_stn`: 74 (if `pin_tool_end` is NOT defined) / 52 (if `pin_tool_end` is defined)
    - `tool_stn_unload`: 71
    - `variable_retract_length`: 20
    - `variable_pushback_length`: 10

=== "Dragon ACE with UHF <br>mini holder / 3mm spacer"

    - `tool_stn`: 60
    - `tool_stn_unload`: 106
    - `variable_retract_length`: 22
    - `variable_pushback_length`: 20

=== "Bambu X1 Hotend"

    - `tool_stn`: 55
    - `tool_stn_unload`: 62
    - `variable_retract_length`: 25
    - `variable_pushback_length`: 23
------

#### Stealthburner / Other extruders

=== "G2E and Dragon HF"

    - `tool_stn`: 79
    - `tool_stn_unload`: 100
    - `variable_retract_length`: 30
    - `variable_pushback_length`: 25

=== "G2E and Mosquito Magnum"

    - `tool_stn`: 72
    - `tool_stn_unload`: 68
    - `variable_retract_length`: 33.4
    - `variable_pushback_length`: 33

=== "G2E and Revo"

    - `tool_stn`: 71 (if `pin_tool_end` is NOT defined) / 41 (if `pin_tool_end` is defined)
    - `tool_stn_unload`: 105.5
    - `variable_retract_length`: 21
    - `variable_pushback_length`: 22.5

=== "G2SA and Revo"

    - `tool_stn`: 78
    - `tool_stn_unload`: 62
    - `variable_retract_length`: 35
    - `variable_pushback_length`: 30

------

#### Xol-Metrix 
=== "WWG2 and Rapido HF"

    - `tool_stn`: 75 (without post extruder sensor)
    - `tool_stn_unload`: 30
    - `variable_retract_length`: 32
    - `variable_pushback_length`: 27

=== "LXG Lite and Rapido V2 HF"

    - `tool_stn`: 72 
    - `tool_stn_unload`: 54
    - `variable_retract_length`: 2
    - `variable_pushback_length`: 0.4

=== "Orbiter and Dragon HF"

    - `tool_stn`: 16
    - `tool_stn_unload`: 70
    - `variable_retract_length`: 17
    - `variable_pushback_length`: 15

=== "Orbiter 2 and Revo"
    
    - `tool_stn`: 70
    - `tool_stn_unload`: 110
    - `variable_retract_length`: 20
    - `variable_pushback_length`: 0

=== "Orbiter 2.5 and Rapido V2 HF"

    - `tool_stn`: 72
    - `tool_stn_unload`: 100
    - `variable_retract_length`: 20
    - `variable_pushback_length`: 25

=== "Sherpa Mini and Rapido HF"

    - `tool_stn`: 40
    - `tool_stn_unload`: 100
    - `variable_retract_length`: 5
    - `variable_pushback_length`: 3

=== "Sherpa Mini and Rapido V2 HF"

    - `tool_stn`: 26
    - `tool_stn_unload`: 100
    - `variable_retract_length`: 25
    - `variable_pushback_length`: 5

=== "WWG2 and Revo"

    - `tool_stn`: 30
    - `tool_stn_unload`: 62
    - `variable_retract_length`: 22
    - `variable_pushback_length`: 20

-------

#### Dragonburner
=== "Orbiter with Phaetus Next G"

    - `tool_stn`: 75 
    - `tool_stn_unload`: 75
    - `variable_retract_length`: 15
    - `variable_pushback_length`: 15

=== "Orbiter with Dragon HF"

    - `tool_stn`: 55 
    - `tool_stn_unload`: 65
    - `variable_retract_length`: 20
    - `variable_pushback_length`: 25

=== "Orbiter 2 with Phaetus Conch"

    - `tool_stn`: 55 
    - `tool_stn_unload`: 100
    - `variable_retract_length`: 20
    - `variable_pushback_length`: 15

------
#### VZBot Goliath

=== "Vz-Hextrudort with Goliath"
    
    - `tool_stn`: 40
    - `tool_stn_unload`: 60
    - `variable_retract_length`: 12
    - `variable_pushback_length`: 5
------

#### Jabberwocky
=== "Phaetus Conch"

    - `tool_stn`: 27.23
    - `tool_stn_unload`: 96.8
    - `variable_retract_length`: 21
    - `variable_pushback_length`: 12.45

------

#### A4T
=== "WWBMG (dual sensor), CrossBow Cutter, Rapido 2HF"

    - `tool_stn`: 40
    - `tool_stn_unload`: 65
    - `variable_retract_length`: 32
    - `variable_pushback_length`: 27

=== "WWBMG (dual sensor), CrossBow Cutter, Chube compact"

    - `tool_stn`: 28
    - `tool_stn_unload`: 60
    - `variable_retract_length`: 25
    - `variable_pushback_length`: 23

------
#### Custom / Other

=== "Hemera XS with Revo"
    
    - `tool_stn`: 22
    - `tool_stn_unload`: 0
    - `variable_retract_length`: 20
    - `variable_pushback_length`: 0
