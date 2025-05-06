## Setting up hotend specific values

Some of the values with AFC are extremely dependent on your hotend.

Suggested starting values for hotend types (more to be added later) are below.  `tool_stn` and `tool_stn_unload` 
are in `AFC/AFC_Hardware.cfg`, `variable_retract_length` and `variable_pushback_length` are in
`AFC/AFC_Macro_Vars.cfg`. For `tool_stn`, if you have `pin_tool_end` defined, use the second value; otherwise, use
the first value. You may need to increase this value if you are using a ram buffer as the toolhead sensor.

Below is an example diagram of a Revo Voron hotend on FilamATrix/Clockwork 2:

![revo-voron-filamatrix-cw2-diagram](../../assets/images/example-cw2-revo.png)

### Hotend specific values

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

=== "Revo Voron + G2E"

    - `tool_stn`: 72
    - `tool_stn_unload`: 62
    - `tool_sensor_after_extruder`: 0 (if running a single sensor)
    - `tool_unload_speed`: 25
    - `tool_load_speed`: 25

