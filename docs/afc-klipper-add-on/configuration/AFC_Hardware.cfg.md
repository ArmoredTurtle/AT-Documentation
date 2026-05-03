# [AFC_Hardware.cfg] Configuration Overview

The `AFC_Hardware.cfg` file is used to typically define options such as the AFC extruder configuration, filament 
switch bypass sensors, and buffer configurations.

This file is typically located in the `~/printer_data/config/AFC` directory and is created during the installation 
of the AFC-Klipper-Add-On.

## [AFC_extruder extruder] Section

The following options are available in the `[AFC_extruder extruder]` section of the `AFC_Hardware.cfg` file. These options 
control the configuration of the AFC system when interfacing with the extruder / toolhead.

!!! note

    These options will most likely require the most amount of configuration and tuning.

``` cfg
[AFC_extruder extruder]
pin_tool_start: mcu:pin
#    MCU defined pin for filament sensor located before (pre) the
#    extruder gears. This is used to detect the presence of filament
#    before the extruder gears. 
pin_tool_end: mcu:pin
#    MCU defined pin for filament sensor located after (post) the
#    extruder gears. This is used to detect the presence of filament
#    after the extruder gears.
tool_stn: 72
#    Default: 72
#    See documentation for details on how to calculate this value. 
#    https://armoredturtle.xyz/docs/afc-klipper-add-on/toolhead/calculation.html
tool_stn_unload: 100
#    Default: 100      
#    See documentation for details on how to calculate this value.
#    https://armoredturtle.xyz/docs/afc-klipper-add-on/toolhead/calculation.html
tool_sensor_after_extruder: 0
#    Default: 0
#    Extra distance to move in mm once pre/post sensors are clear. 
#    Useful for when only using post sensor, so this distance can 
#    be the amount to move to clear extruder gears.
tool_unload_speed: 25
#    Default: 25      
#    Unload speed in mm/s when unloading toolhead.
tool_load_speed: 25             
#    Default: 25
#    Load speed in mm/s when unloading toolhead.
buffer: <buffer_name>
#    Buffer to use for extruder, this variable can be overridden 
#    per lane.
enable_sensors_in_gui: False
#    Default: False
#    Set to True toolhead sensors switches as filament sensors in 
#    Mainsail/Fluidd gui, overrides value set in AFC.cfg.
enable_tool_runout: True
#    Default: True
#    If enabled and toolhead sensor(s) detect filament not present while printing AFC
#    will pause printing. Inputting value here overrides global value in AFC.cfg file
debounce_delay: 0
#    Default: 0
#    A period of time in seconds to debounce switches prior to detecting
#    runout. If switches are pressed and released during this delay,
#    the entire switch event is ignored.
#
#    This value overrides value set in AFC config section
```

### Temperature Settings
 
``` cfg
[AFC_extruder extruder]
deadband: 2
#    Default: 2
#    Temperature deadband tolerance in degrees Celsius. AFC considers the
#    extruder "at temperature" when the current reading is within ±deadband
#    degrees of the target. Prevents excessive waiting caused by minor
#    sensor fluctuations. Also used as the D parameter in AFC's M109
#    override when waiting for temperature.
toolchange_temp_drop: 0
#    Default: 0
#    Degrees to drop this extruder's temperature without waiting after it
#    becomes the outgoing tool in a toolchange. Setting above 0 reduces
#    idle ooze on deselected toolheads while keeping them warm enough for
#    a quick reheat. Explicitly overrides the global toolchange_temp_drop
#    in AFC.cfg for this extruder only. Set to 0 to disable the drop.
```

### LED Settings
 
!!! note
 
    All LED index values are 1-based and refer to positions within the LED
    chain defined by `led_name`. Indices assigned to `status_led_idx` and
    `nozzle_led_idx` must not overlap — AFC will raise a configuration
    error at startup if they do.
 
``` cfg
[AFC_extruder extruder]
led_name: neopixel toolhead_leds
#    Name of the Klipper LED object (matching a [neopixel ...] or
#    [dotstar ...] config section) that AFC should control for this
#    toolhead. Used for both status indication and nozzle illumination.
#    AFC will raise an error at startup if the named object is not found.
#    Leave unset to disable LED control for this extruder.
status_led_idx: 1
#    Comma-separated LED index position(s) (1-based) within the led_name
#    chain reserved for AFC status indication. These LEDs reflect the
#    current lane/tool state (e.g. ready, loading, fault) and are excluded
#    from print lighting controlled by AFC_SET_EXTRUDER_LED.
#    Example: status_led_idx: 1        (single status LED)
#    Example: status_led_idx: 1,2      (two status LEDs)
#    Leave unset if no LEDs are dedicated to status.
nozzle_led_idx: 2,3
#    Comma-separated LED index position(s) (1-based) within the led_name
#    chain used for nozzle illumination. When set, AFC_SET_EXTRUDER_LED
#    toggles only these LEDs for print lighting instead of all non-status
#    LEDs in the chain. Leave unset to allow AFC_SET_EXTRUDER_LED to
#    toggle all LEDs not reserved by status_led_idx.
```

### Toolchanger Settings
 
!!! note
 
    The following options are only required for multi-toolhead toolchanger
    setups. Leave all of these unset for standard single-toolhead printers.
 
``` cfg
[AFC_extruder extruder]
toolchanger_unit: toolchanger
#    Name of the AFC_Toolchanger unit this extruder belongs to
#    (e.g. toolchanger references [AFC_Toolchanger toolchanger]).
#    When set, AFC creates a synthetic lane for this extruder, enables
#    shuttle detection via the tool's dock sensor, and registers this
#    extruder with the toolchanger unit at startup. AFC will raise an
#    error if the named AFC_Toolchanger object is not found in the config.
#    Leave unset for single-toolhead printers.
tool: tool T0
#    Full Klipper config section name of the klipper-toolchanger tool
#    object associated with this extruder (e.g. tool T0 references
#    [tool T0]). AFC uses this object to determine whether the tool is
#    physically docked or attached to the carriage via the tool's
#    detect_state attribute. If unset while toolchanger_unit is set,
#    AFC assumes the tool is always on the shuttle (on_shuttle returns
#    True unconditionally), which is appropriate when using
#    custom_tool_swap and custom_unselect macros without native
#    toolchanger detection.
map: T0
#    T-number identifier (e.g. T0, T1) that maps this extruder to a tool
#    position. Routes slicer temperature commands (M104/M109 T<n>),
#    AFC_SET_TOOLHEAD_LED, and Moonraker lane data to this extruder.
#    If left unset, AFC auto-assigns the next available T-number at
#    startup (T0, T1, T2...) while respecting any manually assigned values
#    on other lanes. Explicit assignment is strongly recommended for
#    multi-toolhead setups to guarantee predictable T-number ordering.
#    Runtime remapping is possible via SET_MAP and RESET_AFC_MAPPING.
custom_tool_swap: _MY_PICK_T0
#    Name of a G-code macro to execute instead of klipper-toolchanger default
#    SELECT_TOOL T=<n> macro when this extruder becomes the incoming tool in a
#    toolchange. The macro is responsible for the physical tool pickup motion
#    only — AFC automatically calls ACTIVATE_EXTRUDER and updates position offsets
#    after the macro returns. The tool index for the default SELECT_TOOL
#    path is derived from the extruder name (extruder=T0, extruder1=T1).
#    Leave unset to use AFC's default SELECT_TOOL behavior.
custom_unselect: _MY_DOCK_T0
#    Name of a G-code macro to execute instead of klipper-toolchanger default
#    UNSELECT_TOOL macro when this extruder is the outgoing tool in a toolchange
#    or when AFC_UNSELECT_TOOL is called. The macro is responsible for the
#    physical docking motion only — AFC handles LED state updates and
#    Spoolman spool deactivation after the macro returns. This macro is
#    triggered on every toolchange where this extruder is deselected.
#    Leave unset to use AFC's default UNSELECT_TOOL behavior.
```

## [AFC_buffer buffer_name] Section
The following options are available in the `[AFC_buffer buffer_name]` section of the `AFC_Hardware.cfg` file. These options
control the configuration of the AFC system when interfacing with the filament buffer.

``` cfg
[AFC_buffer buffer_name]
advance_pin: mcu:pin
#    MCU defined pin for advance sensor.
trailing_pin: mcu:pin
#    MCU defined pin for trailing sensor.
multiplier_high: 1.05
#    Default: 1.05
#    Factor to move more filament through the secondary extruder.
multiplier_low: 0.95
#    Default: 0.95
#    Factor to move less filament through the secondary extruder.
led_index: Buffer_Indicator:1
#    LED index for the buffer, used to control the buffer LED
#    (if present).
accel: 0
#    Default: 0 
#    Error if the buffer is not configured properly.
```

## [AFC_led Buffer_Indicator] Section

The following options are available in the `[AFC_led Buffer_Indicator]` section of the `AFC_Hardware.cfg` file. These options
control the configuration of the AFC system when interfacing with the buffer LED.

``` cfg
[AFC_led Buffer_Indicator]
pin: mcu:pin 
#    MCU defined pin for the LED.
chain_count: 1
#    Default: 1
#    Number of LEDs in the chain.
color_order: GRB
#    Default: GRB
#    Color order of the LED chain.
initial_RED: 0.0
#    Initial RED value of the LED.
initial_GREEN: 0.0
#    Initial GREEN value of the LED.
initial_BLUE: 0.0
#    Initial BLUE value of the LED.
initial_WHITE: 0.0
#    Initial WHITE value of the LED.
```