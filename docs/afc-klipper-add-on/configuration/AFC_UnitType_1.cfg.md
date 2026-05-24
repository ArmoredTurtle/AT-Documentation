# AFC_UnitType_1.cfg Configuration Overview

The `AFC_UnitType_1.cfg` file is used to define the configuration for the AFC system when using a specific unit type.

This file will typically be named in the format of `AFC_Turtle_1.cfg` for example, when a BoxTurtle is installed. If 
multiple unit types are installed, the file will be named in the format of `AFC_Turtle_2.cfg`, `AFC_Turtle_3.cfg`, etc.

!!! note

    The actual naming convention may vary depending on your specific setup. The unit name can be renamed by using the 
    `install-afc.sh` script.

## [mcu mcu_name] Section

This section will define the MCU used for the specific unit.

!!! warning

    Either `canbus_uuid` or `serial` should be defined, not both. Please ensure that you are using the correct
    configuration for your setup. The `canbus_uuid` is used for CAN bus devices, while the `serial` is used for
    USB devices. Whichever option is **NOT** being used should be commented out.

``` cfg
[include mcu/<mcu_name>.cfg]
#    This will be used for any MCU aliases or definition files.
[mcu mcu_name]
canbus_uuid: 1234567890a
#    UUID of the CAN bus device. This is used to identify the device 
#    on the CAN bus.
serial: /dev/serial/by-id/<device-name>
#    Serial address of the device. This is usually in the format of
#    `/dev/serial/by-id/<device-name>` 
```

## [temperature_sensor sensor_name] Section

If your MCU has a temperature sensor built in, you can define it here. 

``` cfg
[temperature_sensor sensor_name]
sensor_type: temperature_mcu
#    Type of temperature sensor. This is usually `temperature_mcu`
sensor_mcu: mcu_name
#    Name of the MCU that the temperature sensor is connected to.
```

## [AFC_lane lane_name] Section

The following options are available in the `[AFC_lane <lane_name>]` section of the `AFC_UnitType_1.cfg` file. These
options control the configuration of the AFC system when interfacing with the stepper motor for the specific unit type.
You will typically have one of these sections for each lane in the unit.  

``` cfg
[AFC_lane <lane_name>]

unit: MCU:<lane>
#    This is the unit name of the stepper motor. This would typically be 
#    formatted as `MCU:<lane>` where `<lane>` is the lane number. The
#    MCU being referenced is relative to the `[mcu <mcu_name>]`
#    in this file. For example: `unit: Turtle_1:1` would set this as
#    the first lane for the hardware connected to the `Turtle_1` MCU.
step_pin: mcu:pin
#    Step GPIO pin (triggered high). This parameter must be provided.
dir_pin: mcu:pin
#    Direction GPIO pin (high indicates positive direction). This
#    parameter must be provided.
enable_pin: mcu:pin
#    Enable pin (default is enable high; use ! to indicate enable
#    low). If this parameter is not provided then the stepper motor
#    driver must always be enabled.
microsteps: 16
#    The number of microsteps the stepper motor driver uses. This
#    parameter must be provided.
rotation_distance: 4.65
#    Default: 4.65
#    Distance (in mm) that the axis travels with one full rotation of
#    the stepper motor (or final gear if gear_ratio is specified).
#    This parameter must be provided. With a standard BoxTurtle based
#    on BOM components, this parameter should not need to be adjusted.
gear_ratio:
#    Default: <none>
#    For a standard BoxTurtle, this parameter should not need to be
#    set. This parameter is only used if a different gear set is used.
#    The gear ratio if the stepper motor is connected to the axis via a
#    gearbox. For example, one may specify "5:1" if a 5 to 1 gearbox is
#    in use. If the axis has multiple gearboxes one may specify a comma
#    separated list of gear ratios (for example, "57:11, 2:1"). If a
#    gear_ratio is specified then rotation_distance specifies the
#    distance the axis travels for one full rotation of the final gear.
#    The default is to not use a gear ratio.
map:
#    Default: None
#    Used for custom mapping toolchange macros. This can be set to anything
#    but a normal toolchange macro is normally T(n).
dist_hub: 60
#    Default: 60
#    Bowden distance between Box Turtle extruder and hub.
park_dist: 10
#    Default: 10
#    Currently unused.
led_index: AFC_Indicator:1
#    Default: None
#    LED index of lane in chain of lane LEDs. This should reference
#    a defined LED in the `[AFC_Indicator]` section.
#
#    Index can have multiple entries in a comma separated list and range values
#    also are allowed
#    eg. AFC_Indicator_4:1,2,3,4, 6-9, 11-14, 16-18
afc_motor_rwd: Turtle_1:MOT1_RWD
#    Reverse pin on MCU for spoolers (N20 motors).
afc_motor_fwd: Turtle_1:MOT1_FWD
#    Forward pin on MCU for spoolers (N20 motors).
afc_motor_enb: Turtle_1:MOT1_EN
#    Enable pin on MCU for spoolers (N20 motors).
rwd_speed_multiplier: 0.5
#    Default: 0.5
#    Multiplier to apply to rpm.
fwd_speed_multiplier: 0.5
#    Default: 0.5
#    Multiplier to apply to rpm.
pwm: True
#    Default: True
#    If True, the n20 motor will be controlled using PWM. 
prep: ^!Turtle_1:TRG1
#    MCU pin for prep trigger. This is the sensor attached to the
#    trigger that is used when filament is loaded.
load: ^!Turtle_1:EXT1
#    MCU pin for load trigger. This is the sensor attached to the
#    extruder body located AFTER the extruder motor.
led_fault: 1,0,0,0
#    Default: 1,0,0,0
#    LED color to set when faults occur in lane.
#    (R,G,B,W) 0 = off, 1 = full brightness. Setting value here 
#    overrides values set in unit(AFC_BoxTurtle/NightOwl/etc) section.
led_ready: 0,0.8,0,0
#    Default: 0,0.8,0,0
#    LED color to set when lane is ready.
#    (R,G,B,W) 0 = off, 1 = full brightness. Setting value here 
#    overrides values set in unit(AFC_BoxTurtle/NightOwl/etc) section.
led_not_ready: 1,0,0,0
#    Default: 1,0,0,0
#    LED color to set when lane is not ready.
#    (R,G,B,W) 0 = off, 1 = full brightness. Setting value here 
#    overrides values set in unit(AFC_BoxTurtle/NightOwl/etc) section.
led_loading:
#    Default: <none>
#    LED color to set when lane is loading.
#    (R,G,B,W) 0 = off, 1 = full brightness. Setting value here 
#    overrides values set in unit(AFC_BoxTurtle/NightOwl/etc) section.
led_unloading: 1,1,1,0
#    Default: 1,1,1,0
#    LED color to set when lane is unloading.
#    (R,G,B,W) 0 = off, 1 = full brightness. Setting value here 
#    overrides values set in unit(AFC_BoxTurtle/NightOwl/etc) section.
led_tool_loaded: 0,0,1,0
#    Default: 0,0,1,0
#    LED color to set when lane is loaded in toolhead.
#    (R,G,B,W) 0 = off, 1 = full brightness. Setting value here 
#    overrides values set in unit(AFC_BoxTurtle/NightOwl/etc) section.
led_spool_index:
#    Default: None
#    Led index for led in chain that illuminates spool, currently only
#    supports QuattroBox units. This should reference a defined LED in 
#    the [AFC_Indicator] section. 
#
#    Index can have multiple entries in a comma separated list and range 
#    values also are allowed.
#    eg. AFC_Indicator_4:1,2,3,4, 6-9, 11-14, 16-18
led_tool_loaded_idle: 0.4,0.4,0,0
#    Default: 0.4,0.4,0,0
#    LED color used when this lane is loaded into the toolhead and idle.
#    Format: (R,G,B,W) where 0 = off and 1 = full brightness.
#
#    Setting value here overrides values set in unit(AFC_BoxTurtle/NightOwl/etc) 
#    section.
led_tool_unloaded: 1,0,0,0
#    Default: 1,0,0,0
#    LED color used when this lane is not loaded in the toolhead.
#    Format: (R,G,B,W) where 0 = off and 1 = full brightness.
#
#    Setting value here overrides values set in unit(AFC_BoxTurtle/NightOwl/etc) 
#    section.
led_spool_illuminate: 1,1,1,0
#    Default: 1,1,1,0
#    Loading color to illuminate spool, currently only for QuattroBox units.
led_use_filament_color: False
#    Default: False
#    When True, lane LED colors will use the filament color from the spool color
#    field (set manually or synced from Spoolman) instead of the configured LED 
#    state colors.
#
#    Setting value here overrides values set in unit(AFC_BoxTurtle/NightOwl/etc) 
#    section.
long_moves_speed: 150
#    Default: 150
#    Speed in mm/s to move filament when doing long moves. 
#    Setting value here overrides values set in 
#    unit(AFC_BoxTurtle/NightOwl/etc) section
rev_long_moves_speed_factor: 1.0
#    Default: 1.0
#    Range: 0.5 to 1.2
#    Scalar factor multiplied to long_moves_speed when rewinding filament.
#    Useful when reversing is done using non-assisted systems, e.g. filamentalist
long_moves_accel: 250
#    Default: 250
#    Acceleration in mm/s squared when doing long moves. 
#    Setting value here overrides values set in 
#    unit(AFC_BoxTurtle/NightOwl/etc) section
quiet_moves_speed: 50
#    Default: 50           
#    Speed in mm/s. This is the speed cap during quiet moves.
short_moves_speed: 50
#    Default: 50
#    Speed in mm/s to move filament when doing short moves. 
#    Setting value here overrides values set in 
#    unit(AFC_BoxTurtle/NightOwl/etc) section
short_moves_accel: 300
#    Default: 300
#    Acceleration in mm/s squared when doing short moves. 
#    Setting value here overrides values set in 
#    unit(AFC_BoxTurtle/NightOwl/etc) section
short_move_dis: 10
#    Default: 10
#    Move distance in mm for failsafe moves. Setting value 
#    here overrides values set in unit(AFC_BoxTurtle/NightOwl/etc) 
#    section
max_move_dis: 99999
#    Default: 99999
#    Maximum distance to move filament. AFC breaks filament moves over 
#    this number into multiple moves. Useful to lower this number if 
#    running into timer too close errors when doing long filament moves. 
#    Setting value here overrides values set in 
#    unit(AFC_BoxTurtle/NightOwl/etc) section
n20_break_delay_time: 0.200
#    Default: 0.200
#    Time to wait between braking N20 motors(nSleep/FWD/RWD all 1) 
#    and then releasing the brake to allow coasting. Setting value 
#    here overrides values set in unit (AFC_BoxTurtle/NightOwl/etc) section.
enable_assist: True
#    Default: True
#    Enables espooler print assist. Setting value 
#    here overrides values set in unit (AFC_BoxTurtle/NightOwl/etc) section.
enable_assist_weight: 500
#    Default: 500
#    Number in grams to activate espooler print assist once spool weight is 
#    less than this number.
#    Setting value here overrides values set in unit (AFC_BoxTurtle/NightOwl/etc)
#    section.
timer_delay: 5
#    Default: 5
#    Affects espooler assist, number of seconds to wait before checking 
#    filament movement for espooler print assist. Setting value here 
#    overrides values set in unit (AFC_BoxTurtle/NightOwl/etc) section.
enable_kick_start: True
#    Default: True
#    Enables full speed espoolers for `kick_start_time` amount to
#    help spools to start moving. Setting value here overrides values
#    set in unit (AFC_BoxTurtle/NightOwl/etc) section.
kick_start_time: 0.070
#    Default: 0.070
#    Time in seconds to enable spooler at full speed to help with
#    getting the spool to spin. Setting value here overrides values set 
#    in unit (AFC_BoxTurtle/NightOwl/etc) section.
delta_movement: 150
#    Default: 150
#    Affects espooler assist, delta amount in mm to move from last assist
#    to trigger another assist move. Setting value here overrides values 
#    set in unit (AFC_BoxTurtle/NightOwl/etc) section.
spoolrate: 1.0
#    Default: 1.0
#    Scaling factor for the following variables: 
#      kick_start_time, spool_outer_diameter, delta_movement
#    Setting value here overrides values set in unit(AFC_BoxTurtle/NightOwl/etc).
#    section.
espool_rot_dist: 132.9
#    Default: 132.9
#    Rotation distance estimation of espooler wheels, setting value
#    here will override value in AFC.cfg. Setting value here
#    overrides values set in unit(AFC_BoxTurtle/NightOwl/etc).
spool_ratio: 2
#    Default: 2
#    Gear ratio for printed gearbox between N20 and spooler wheels, setting value here
#    overrides values set in unit(AFC_BoxTurtle/NightOwl/etc).
full_weight: 1000
#    Default: 1000
#    Full starting weight of filament spool (not including spool), setting value here
#    overrides values set in unit(AFC_BoxTurtle/NightOwl/etc).
load_to_hub: True
#    Default: True
#    Fast loads filament to hub when inserted, set to False to disable. This value is not
#    used in HTLF units.
#    Setting here overrides global setting in AFC.cfg.
enable_sensors_in_gui: False
#    Default: False
#    Set to True to show prep and load sensors switches as filament sensors 
#    in Mainsail/Fluidd gui, overrides value set in AFC.cfg.
sensor_to_show:
#    Default: <none>
#    Set to prep to only show prep sensor, set to load to only show load 
#    sensor. Do not add if you want both prep and load sensors to show in 
#    web GUI.
assisted_unload: False
#    Default: False
#    If True, the unload retract is assisted to prevent loose windings, 
#    especially on full spools. This can prevent loops from slipping off the 
#    spool. Setting value here overrides values set in 
#    unit(AFC_BoxTurtle/NightOwl/etc) section.
filament_diameter: 1.75
#    Default: 1.75
#    Diameter of the filament in mm.
filament_density: 1.24
#    Default: 1.24
#    Density of the filament in g/cm^3.
spool_inner_diameter: 100
#    Default: 100
#    Inner diameter of the spool in mm.
spool_outer_diameter: 200
#    Default: 200 
#    Outer diameter of the spool in mm. Affects espooler assist logic
empty_spool_weight: 190
#    Default: 190
#    Weight of the empty spool in grams. This value is pulled automatically if
#    using spoolman.
spool_weight: 1000
#    Default: 1000
#    Remaining filament weight in grams on spools.  This value is pulled automatically
#    if using spoolman.
max_motor_rpm: 465
#    Default: 465
#    Maximum motor RPM for the assist motor (N20).
hub: 
#    Default: <none>
#    Hub that this lane belongs to. Set to the name of an AFC_hub
#    section to override the hub assigned by the
#    unit (AFC_BoxTurtle/NightOwl/etc).
#
#    Two special values are also accepted:
#
#    direct       - Use when this lane has no hub and connects directly
#                   to the toolhead.
#    direct_load  - Same as direct, but AFC will automatically load
#                   filament to the toolhead when filament is inserted
#                   into the lane.
buffer:
#    Default: <none>
#    Buffer name(AFC_buffer) that belongs to this stepper, overrides 
#    buffer that is set in extruder(AFC_extruder) or 
#    unit(AFC_BoxTurtle/NightOwl/etc) sections.
extruder:
#    Default: <none>
#    Extruder name(AFC_extruder) that belongs to this stepper, 
#    overrides extruder that is set in unit(AFC_BoxTurtle/NightOwl/etc) 
#    section.
debounce_delay: 0
#    Default: 0
#    A period of time in seconds to debounce switches prior to detecting
#    runout. If switches are pressed and released during this delay,
#    the entire switch event is ignored.
#
#    This value overrides value set in AFC config section
capture_td1_when_loaded: False
#    Default: False
#    When set to True and TD-1 device is correctly configured in moonraker, 
#    when filament is loaded into a lane AFC will move filament to TD-1 device
#    to capture TD and color.
#
#    Setting value here overrides values set in unit config(AFC_BoxTurtle/NightOwl/etc).
td1_device_id: None
#    Default: None
#    Set this value to TD-1 device ID to use for a lane, this is only needed if
#    using multiple TD-1 devices.
td1_bowden_length:
#    Default: value from AFC_hub config section (if set)
#    Length in mm from the hub to the TD-1 device for this lane. AFC moves
#    filament exactly this distance when capturing TD-1 color and type data.
#
#    If not set, AFC falls back to the td1_bowden_length
#    value defined in the associated AFC_hub section. This value is not
#    used for direct hub setups.
#
#    Setting value here overrides td1_bowden_length set in AFC_hub section.
#
#    Run AFC_CALIBRATION to automatically calibrate this length,
#    option to calibrate will only show up if TD-1 has been setup
#    correctly in moonraker.
remember_spool: False
#    Default: False
#    If true, AFC will retain values (spoolID, weight, color, material) of the last spool
#    that was ejected from a lane and will reuse those values the next time the given
#    lane is loaded.
#    Overrides variable that is set in unit(AFC_BoxTurtle/NightOwl/etc) sections.
homing_overshoot: 50
#    Default: 50
#    Additional amount to add to all homing moves to guarantee the move will hit
#    endstops when homing.
#
#    Overrides variable that is set in unit(AFC_BoxTurtle/NightOwl/etc) sections.
homing_delta: 300
#    Default: 300
#    This value is only valid when homing is enabled and is used as another verifier
#    to make sure filament is within this range from commanded homing movement. If
#    homing move is not within this delta of commanded movement then AFC will try 
#    to home again to verify that filament is at correct position.
#
#    Overrides variable that is set in unit(AFC_BoxTurtle/NightOwl/etc) sections.
load_then_home: True
#    Default: True
#    When set to True and utilizing the buffer in ramming mode, AFC will first do a normal
#    move toward the toolhead where the total distance is:
#       - afc_bowden_length - load_undershoot, or
#       - dist_hub - load_undershoot (when using a direct hub).
#    After this initial move to the toolhead, AFC will then do a homing move the rest of
#    the way until the buffer advance sensor is triggered.
#
#    Overrides variable that is set in unit(AFC_BoxTurtle/NightOwl/etc) sections.
load_undershoot: 20
#    Default: 20
#    Amount to subtract from afc_bowden_length (or from dist_hub when using a direct hub)
#    when load_then_home is enabled.
#
#    Overrides variable that is set in unit(AFC_BoxTurtle/NightOwl/etc) sections.
extruder_clear_dis: 50
#    Default: 50
#    This variable affects additional move distance when ejecting filament and filament
#    has passed load sensor. AFC will move this additional amount to make sure filament
#    is no longer in extruder gears.
#    Overrides variable that is set in unit(AFC_BoxTurtle/NightOwl/etc) sections.
calibrated_lane: False
#    Default: False
#    Currently used in AFC_vivid units so AFC know if lane needs to calibrate dist_hub
#    distance when inserting filament for the first time.
selector_cal_distance: 0.0
#    Default: 0.0
#    Currently used only by AFC_vivid units, this value is the amount to move in mm once
#    selector is homed to specified lane. AFC will then move the selector by this amount
#    after the home to sensor has finished. By modifying this value, this could allow
#    the selector to have a better grip on the filament.
custom_load_cmd:
#    Default: None
#    Custom G-code macro to run instead of AFC's built-in load sequence
#    when loading this lane to the toolhead. Leave unset to use AFC's
#    default load sequence.
custom_unload_cmd:
#    Default: None
#    Custom G-code macro to run instead of AFC's built-in unload sequence
#    when unloading this lane from the toolhead. Leave unset to use AFC's
#    default unload sequence.
```

## [AFC_stepper lane_name] Section

The following options are available in the `[AFC_stepper <lane_name>]` section of the `AFC_UnitType_1.cfg` file. These
options control the configuration of the AFC system when interfacing with the stepper motor for the specific unit type.
You will typically have one of these sections for each lane in the unit.

AFC_stepper inherits configuration options from AFC_lane configuration section, below are additional configuration values
for the AFC_stepper sections.  

``` cfg

[AFC_stepper <lane_name>]

print_current: 0.6
#    Default: 0.6
#    Current to use while printing, set to a lower current to reduce stepper 
#    heat when printing. Defaults to global_print_current, if not specified 
#    current is not changed.
default_homing_endstop: load
#    Default: load
#    Endstop to default to when running AFC_STEPPER_HOME macro.
hub_endstop: <hub_endstop_pin>
#    Default: endstop defined in AFC_hub config
#    Override default AFC_hub endstop lookup with custom hub endstop pin
#    (endstop must be defined in the AFC_hub config).
extra_homing_pins: ""
#    Default: ""
#    Extra homing pins to add to the lookup; can be used as an
#    endstop selection for the `AFC_STEPPER_HOME` macro.
```


## [tmc2209 AFC_stepper <lane_name>] Section

The following options are available in the `[tmc2209 AFC_stepper <lane_name>]` section of the `AFC_UnitType_1.cfg` file. These
options control the configuration of the AFC system when interfacing with the TMC2209 stepper motor driver for the specific unit type.

!!!note

    There should be a matching `[tmc2209 AFC_stepper <lane_name>]` section for each `[AFC_stepper <lane_name>]` 
    section in the configuration file.

``` cfg
[tmc2209 AFC_stepper <lane_name>]
uart_pin: mcu:pin
#    Example: Turtle_1:M1_UART
#    The UART pin for the TMC2209 stepper motor driver. 
uart_address: 0
#    Address of the TMC2209 driver on the UART bus.
run_current: 0.8
#    The amount of current (in amps RMS) to configure the driver to use
#    during stepper movement. This parameter must be provided.
sense_resistor: 0.110
#    The resistance (in ohms) of the driver sense resistor. This parameter
#    must be provided. Common values are 0.110 ohms for most TMC2209 drivers
#    and 0.075 ohms for TMC5160 drivers. Check your stepper driver documentation
#    or board schematic to confirm the correct value.
```
!!!warning

    Ensure that the `sense_resistor` is set appropriately. Failure to set this appropriately may result in
    overheating of the stepper motor driver and/or stepper motor. This value is typically set to 0.110 for the TMC2209
    stepper motor driver present on the AFC-Lite board for the BoxTurtle. You *MUST* change this based on your
    board if using another system / configuration.

## [AFC_hub hub_name] Section

The following options are available in the `[AFC_hub hub_name]` section of the `AFC_UnitType_1.cfg` file. These 
options control the configuration of the AFC system when interfacing with the hub for the specific unit type. Multiple
hubs may be defined in the configuration file.

``` cfg
[AFC_hub hub_name]
switch_pin: mcu:pin
#    Default: <none> (required)
#    MCU pin for the hub switch.
#
#    Alternatively, set to `virtual` to use a virtual hub sensor instead
#    of a physical switch. When set to virtual, the hub state is
#    determined by the load sensors on each attached lane — the hub is
#    considered triggered if any lane's load sensor is active.
#
#    Requires all lanes attached to this hub to have a load sensor
#    configured. AFC will raise a configuration error at startup listing
#    any lanes that are missing a load sensor.
#
#    Example (physical): switch_pin: mcu:PA5
#    Example (virtual):  switch_pin: virtual
hub_clear_move_dis: 55
#    Default: 55
#    How far to move filament so that it doesn't block the hub exit.
afc_bowden_length:  900
#    Default: 900
#    Length of the bowden tube in mm from the hub to the toolhead 
#    sensor.
#    Run AFC_CALIBRATION to automatically calibrate this length.
afc_unload_bowden_length: 900
#    Default: <afc_bowden_length>
#    Length to unload when retracting back from toolhead to hub in 
#    mm. This defaults to the value of `afc_bowden_length` if not
#    specified. With normal installations, this value should not
#    need to be changed or specified explicitly.
td1_bowden_length: <afc_bowden_length> - 50
#    Default: <afc_bowden_length> - 50
#    Length of the bowden tube in mm from the hub to TD-1 device.
#
#    Run AFC_CALIBRATION to automatically calibrate this length,
#    option to calibrate will only show up if TD-1 has been setup
#    correctly in moonraker.
use_dist_hub: False
#    Default: False
#    Set to True to use dist_hub value for PTFE length to toolhead.
#    This is useful when using sensorless hubs like in EMU Units and
#    parking filament behind load switch instead of at the hub. This
#    is needed since PTFE path per lane can differ when loading to
#    toolhead.
#
#    Caution: This is not fully implemented everywhere and has only
#    been tested with EMU units.
assisted_retract: False
#    Default: False
#    If true, retracts are assisted to prevent loose windings on the
#    spool.
move_dis: 65
#    Default: 65
#    Distance to move the filament within the hub in mm.
cut: False
#    Default: False
#    Set to true if using a hub cutter. An example of this would
#    be the `Snappy` hub cutter. 
cut_cmd: AFC
#    Default: AFC
#    Macro command to run when the cut command is sent. This should
#    be set to `AFC` if using the built in functionality. This is 
#    only required when `cut: True` is set.
cut_servo_name: cut
#    Default: cut
#    Name of servo to use for the hub cutter. This is only required
#    when `cut: True` is set. This requires a `[servo cut]` section
#    to be defined in a configuration file.
cut_dist: 50
#    Default: 50
#    How much filament (in mm) to cut when the cut command is sent.
#    This is only required when `cut: True` is set.
cut_clear: 120
#    Default: 120
#    How far the filament should retract (in mm) back from the hub.
#    This is only required when `cut: True` is set.
cut_min_length: 200
#    Default: 200
#    Minimum length of filament (in mm) to cut. This is only
#    required when `cut: True` is set.
cut_servo_pass_angle: 0 
#    Default: 0
#    Servo angle to align the bowden tube with the hole for loading
#    the toolhead. This is only required when `cut: True` is set.
cut_servo_clip_angle: 160
#    Default: 160
#    Servo angle for a completed cut movement. This is only required
#    when `cut: True` is set.
cut_servo_prep_angle: 75
#    Default: 75
#    Servo angle to prepare the filament for cutting. This should be 
#    set so that the bowden tube is NOT aligned with the hub exit. 
#    It should be clear so that filament can be pushed out of the hub
#    and the cutting mechanism can be engaged. This is only required
#    when `cut: True` is set.
cut_confirm: False
#    Default: False
#    If true, the cut command will execute twice. This is only
#    required when `cut: True` is set.
enable_sensors_in_gui: False
#    Default: False
#    Set to True to show hub sensor switch as a filament sensor in 
#    Mainsail/Fluidd gui, overrides value set in AFC.cfg.
debounce_delay: 0
#    Default: 0
#    A period of time in seconds to debounce hub switch prior to detecting
#    runout. If hub switch is pressed and released during this delay,
#    the entire switch event is ignored.
#
#    This value overrides value set in AFC config section
enable_hub_runout: True
#    Default: True
#    If enabled and hub sensor(s) detect filament not present while printing AFC will
#    pause printing. Inputting value here overrides global value in AFC.cfg file
```

## [AFC_led led_name] Section

The following options are available in the `[AFC_led led_name]` section of the `AFC_UnitType_1.cfg` file. These
options control the configuration of the AFC system when interfacing with the LED for the specific unit type. Multiple
LEDs may be defined in the configuration file.

``` cfg
[AFC_led AFC_Indicator]
pin: mcu:pin
#    MCU pin definition for LED indicator.
chain_count: 1
#    Number of LEDs in the chain.
color_order: GRB
#    Default: GRB
#    Color order of the LEDs in the chain. This is typically set to
#    a value like `GRB` or `GRBW` depending on the LED type. However,
#    if you are experiencing LED issues, ensure you validate that the 
#    proper color order is set. An incorrect value may result in 
#    sporadic behavior of the LEDs.
```

## [AFC_button lane_name] Section

The following options are available in the `[AFC_button lane_name]` section of the `AFC_UnitType_1.cfg` file. These
options control the configuration of the AFC system when using optional hardware buttons to control various functionality
for each lane. Each lane may have its own button configuration, or you may have any combination of lanes configured.

``` cfg
[AFC_button lane1]
pin: 
#    Default: <none>
#    The pin that the switch is connected for the specified lane. 
#    This will typically look like `^!Turtle_1:SW6` or any valid
#    MCU/pin combination. Ensure that you include pull-up(^) and invert(!).
long_press_duration:
#    Default: 1.2
#    The duration in seconds that the button must be pressed to
#    trigger a long press event. This is used to differentiate between
#    a short press and a long press. 
```

More information on the configuration for this is available [here](../features.md).

## [AFC_BoxTurtle unit_name] Section

The following options are available in the `[AFC_BoxTurtle unit_name]` section of the `AFC_UnitType_1.cfg` file. These
options control the configuration of the AFC system when interfacing with the BoxTurtle unit type. This section is
typically used to define the unit name and other options that are specific to the BoxTurtle unit type.

``` cfg
[AFC_BoxTurtle Turtle_1]
hub: 
#    Default: <none>
#    Hub that this unit belongs to. Set to the name of an AFC_hub
#    section. This value can be overridden in the AFC_stepper/AFC_lane sections.
#
#    Two special values are also accepted:
#
#    direct       - Use when this lane has no hub and connects directly
#                   to the toolhead.
#    direct_load  - Same as direct, but AFC will automatically load
#                   filament to the toolhead when filament is inserted
#                   into the lane.
extruder:
#    Default: <none>
#    Extruder name(AFC_extruder) that belongs to this unit. can be
#    overridden in the [AFC_stepper] section.
buffer: 
#    Default: <none>
#    Buffer name(AFC_buffer) that belongs to this unit. can be
#    overridden in the [AFC_stepper] section.
enable_assist: True
#    Default: True
#    Enables espooler print assist, overrides setting in AFC.cfg file
#    Can be overridden in the [AFC_stepper] section.
enable_assist_weight: 500
#    Default: 500
#    Number in grams to activate espooler print assist once spool weight is 
#    less than this number.
#    Can be overridden in the [AFC_stepper] sections.
timer_delay: 5
#    Default: 5
#    Affects espooler assist, number of seconds to wait before 
#    checking filament movement for espooler print assist
#    Can be overridden in the [AFC_stepper] section.
enable_kick_start: True
#    Default: True
#    Enables full speed espoolers for `kick_start_time` amount to
#    help spools to start moving
#    Can be overridden in the [AFC_stepper] section.
kick_start_time: 0.070
#    Default: 0.070
#    Time in seconds to enable spooler at full speed to help with
#    getting the spool to spin
#    Can be overridden in the [AFC_stepper] section.
delta_movement: 150
#    Default: 150
#    Affects espooler assist, delta amount in mm to move from last 
#    assist to trigger another assist move
#    Can be overridden in the [AFC_stepper] section.
spoolrate: 1.0
#    Default: 1.0
#    Scaling factor for the following variables: 
#      kick_start_time, spool_outer_diameter, delta_movement
#    Can be overridden in the [AFC_stepper] section.
spool_ratio: 2
#    Default: 2
#    Gear ratio for printed gearbox between N20 and spooler wheels, setting value
#    here will override value in AFC.cfg. The value also can be overridden in
#    [AFC_stepper/AFC_lane] sections.
full_weight: 1000
#    Default: 1000
#    Full starting weight of filament spool (not including spool), setting value
#    here will override value in AFC.cfg. The value also can be overridden in
#    [AFC_stepper/AFC_lane] sections.
espool_rot_dist: 132.9
#    Default: 132.9
#    Rotation distance estimation of espooler wheels, setting value
#    here will override value in AFC.cfg. The value also can be overridden in
#    [AFC_stepper/AFC_lane] sections.
led_fault: 1,0,0,0
#    Default: 1,0,0,0
#    LED color to set when faults occur in lane        
#    (R,G,B,W) 0 = off, 1 = full brightness. Setting value here 
#    overrides values set in AFC.cfg file.
led_ready: 1,1,0,0
#    Default: 1,1,0,0
#    LED color to set when lane is ready
#    (R,G,B,W) 0 = off, 1 = full brightness. Setting value here
#    overrides values set in AFC.cfg file.
led_not_ready: 1,1,0,0
#    Default: 1,1,0,0
#    LED color to set when lane is not ready
#    (R,G,B,W) 0 = off, 1 = full brightness. Setting value here
#    overrides values set in AFC.cfg file.
led_loading: 1,0,0,0
#    Default: 1,0,0,0
#    LED color to set when lane is loading
#    (R,G,B,W) 0 = off, 1 = full brightness. Setting value here
#    overrides values set in AFC.cfg file.
led_unloading: 1,1,.5,0
#    Default: 1,1,.5,0
#    LED color to set when lane is unloading
#    (R,G,B,W) 0 = off, 1 = full brightness. Setting value here
#    overrides values set in AFC.cfg file.
led_tool_loaded: 1,1,0,0
#    Default: 1,1,0,0
#    LED color to set when lane is loaded in toolhead
#    (R,G,B,W) 0 = off, 1 = full brightness. Setting value here
#    overrides values set in AFC.cfg file.
led_tool_loaded_idle: 0.4,0.4,0,0
#    Default: 0.4,0.4,0,0
#    LED color used when a lane is loaded into the toolhead and idle.
#    Format: (R,G,B,W) where 0 = off and 1 = full brightness.
#
#    Setting value here overrides values set in AFC.cfg file.
led_tool_unloaded: 1,0,0,0
#    Default: 1,0,0,0
#    LED color used when a lane is not loaded in the toolhead.
#    Format: (R,G,B,W) where 0 = off and 1 = full brightness.
#
#    Setting value here overrides values set in AFC.cfg file.
led_use_filament_color: False
#    Default: False
#    When True, lane LED colors will use the filament color from the spool color
#    field (set manually or synced from Spoolman) instead of the configured LED 
#    state colors.
#
#    Setting value here overrides values set in AFC.cfg file.
long_moves_speed: 100
#    Default: 100
#    Speed in mm/s to move filament when doing long moves.
#    Setting value here overrides values set in AFC.cfg file.
rev_long_moves_speed_factor: 1.0
#    Default: 1.0
#    Range: 0.5 to 1.2
#    Scalar factor multiplied to long_moves_speed when rewinding filament.
#    Useful when reversing is done using non-assisted systems, e.g. filamentalist
long_moves_accel: 400
#    Default: 400
#    Acceleration in mm/s squared when doing long moves.
#    Setting value here overrides values set in AFC.cfg file.
short_moves_speed: 25
#    Default: 25
#    Speed in mm/s to move filament when doing short moves.
#    Setting value here overrides values set in AFC.cfg file.
short_moves_accel: 400
#    Default: 400
#    Acceleration in mm/s squared when doing short moves.
#    Setting value here overrides values set in AFC.cfg file.
short_move_dis: 400
#    Default: 400
#    Move distance in mm for failsafe moves. Setting value
#    here overrides values set in AFC.cfg file.
max_move_dis: 99999
#    Default: 99999
#    Maximum distance to move filament. AFC breaks filament moves over
#    this number into multiple moves. Useful to lower this number if
#    running into timer too close errors when doing long filament moves.
#    Setting value here overrides values set in AFC.cfg file.
n20_break_delay_time: 0.200
#    Default: 0.200
#    Time to wait between braking N20 motors(nSleep/FWD/RWD all 1)
#    and then releasing the brake to allow coasting. Setting value
#    here overrides values set in AFC.cfg file.
assisted_unload: False
#    Default: False
#    If True, the unload retract is assisted to prevent loose windings,
#    especially on full spools. This can prevent loops from slipping off the
#    spool. Setting value here overrides values set in AFC.cfg file.
unload_on_runout: False
#    Default: False
#    When True, AFC will unload lane and then pause when runout is 
#    triggered and spool to swap is not set (infinite spool). Setting
#    value here overrides values set in AFC.cfg file.
capture_td1_when_loaded: False
#    Default: False
#    When set to True and TD-1 device is correctly configured in moonraker, 
#    when filament is loaded into a lane AFC will move filament to TD-1 device
#    to capture TD and color.
#
#    Setting value here overrides values set in AFC.cfg file. This value can
#    also be overridden per AFC_lane/AFC_stepper.
td1_device_id: None
#    Default: None
#    Set this value to TD-1 device ID to use for a unit, this is only needed if
#    using multiple TD-1 devices.
#
#    If using separate TD-1 devices per lanes this value should be set in 
#    AFC_lane/AFC_stepper sections.
remember_spool: False
#    Default: False
#    If true, AFC will retain values (spoolID, weight, color, material) of the last
#    spool that was ejected from a lane and will reuse those values the next time
#    the given lane is loaded.
#
#    This variable can be overridden per AFC_lane/AFC_stepper config sections.
homing_overshoot: 50
#    Default: 50
#    Additional amount to add to all homing moves to guarantee the move will hit
#    endstops when homing.
#
#    This variable can be overridden per AFC_lane/AFC_stepper config sections.
homing_delta: 300
#    Default: 300
#    This value is only valid when homing is enabled and is used as another verifier
#    to make sure filament is within this range from commanded homing movement. If
#    homing move is not within this delta of commanded movement then AFC will try 
#    to home again to verify that filament is at correct position.
#
#    This variable can be overridden per AFC_lane/AFC_stepper config sections.
load_then_home: True
#    Default: True
#    When set to True and utilizing the buffer in ramming mode, AFC will first do a normal
#    move toward the toolhead where the total distance is:
#       - afc_bowden_length - load_undershoot, or
#       - dist_hub - load_undershoot (when using a direct hub).
#    After this initial move to the toolhead, AFC will then do a homing move the rest of
#    the way until the buffer advance sensor is triggered.
#
#    This variable can be overridden per AFC_lane/AFC_stepper config sections.
load_undershoot: 20
#    Default: 20
#    Amount to subtract from afc_bowden_length (or from dist_hub when using a direct hub)
#    when load_then_home is enabled.
#
#    This variable can be overridden per AFC_lane/AFC_stepper config sections.
extruder_clear_dis: 50
#    Default: 50
#    This variable affects additional move distance when ejecting filament and
#    filament has passed load sensor. AFC will move this additional amount to
#    make sure filament is not longer in extruder gears.
#    This variable can be overridden per AFC_lane/AFC_stepper config sections.
enable_buffer_tool_check: False
#    Default: False
#    When enabled AFC will verify that a lane is loaded to toolhead when using
#    buffer as a ramming sensor. AFC will try to expand out buffer to hit the 
#    advance sensor, if advance sensor is triggered then AFC know that lane
#    is loaded to toolhead. If this check fails AFC will notify that it could
#    not verify that lane is loaded and give instructions on how to fix.
#
#    Currently this is only intended to work with buffers that have springs
#    or rubber bands between PTFE tubes. Do not enable this if your buffer is
#    not setup this way.
```

## [AFC_NightOwl unit_name] Section

The following options are available in the `[AFC_NightOwl unit_name]` section of the `AFC_UnitType_1.cfg` file. These
options control the configuration of the AFC system when interfacing with the NightOwl unit type. This section is
typically used to define the unit name and other options that are specific to the NightOwl unit type.
 

AFC_NightOwl inherits configuration options from AFC_BoxTurtle configuration section, below are additional configuration values
for a NightOwl unit.  

``` cfg
[AFC_NightOwl NightOwl_1]
```

## [AFC_QuattroBox unit_name] Section

The following options are available in the `[AFC_QuattroBox unit_name]` section of the `AFC_UnitType_1.cfg` file. These
options control the configuration of the AFC system when interfacing with the AFC_QuattroBox unit type. This section is
typically used to define the unit name and other options that are specific to the AFC_QuattroBox unit type.  

AFC_QuattroBox inherits configuration options from AFC_BoxTurtle configuration section, below are additional configuration values
for a QuattroBox unit.  
``` cfg
[AFC_QuattroBox QuattroBox_1]
led_logo_index:
#    Default: None
#    Led index for led in chain that illuminates a logo. This should reference 
#    a defined LED in the [AFC_Indicator] section.
#
#    Index can have multiple entries in a comma separated list and range values
#    also are allowed
#    eg. AFC_Indicator_4:1,2,3,4, 6-9, 11-14, 16-18
led_logo_color: '0,0,0,0'
#    Default: '0,0,0,0'
#    Color to apply to logo. Logo also changes to led_logo_loading color when 
#    loading a lane and then once a lane is loaded the logo changes to the lanes 
#    spool color. When nothing is loaded this variable(led_logo_color) is 
#    applied to logo.
led_logo_loading: 1,0,0,0
#    Default: 1,0,0,0
#    Color to apply to logo when loading a lane, default values defaults to 
#    led_loading variable
led_spool_index:
#    Default: None
#    Led index for led in chain that illuminates spool. This should reference 
#    a defined LED in the [AFC_Indicator] section.
#    Can be overridden per lane in AFC_stepper section.
#
#    Index can have multiple entries in a comma separated list and range values
#    also are allowed
#    eg. AFC_Indicator_4:1,2,3,4, 6-9, 11-14, 16-18
led_spool_illuminate: 1,1,1,0
#    Default: 1,1,1,0
#    Loading color to illuminate spool, currently only for QuattroBox units and
#    can be overridden in AFC_stepper section.
```

## [AFC_HTLF unit_name] Section

The following options are available in the `[AFC_HTLF unit_name]` section of the `AFC_UnitType_1.cfg` file. These
options control the configuration of the AFC system when interfacing with the AFC_HTLF unit type. This section is
typically used to define the unit name and other options that are specific to the AFC_HTLF unit type. 

The following macros are specific to HTLF:  
- [AFC_HOME_UNIT](../klipper/internal/misc.md#AFC_HTLF.AFC_HTLF.cmd_AFC_HOME_UNIT)  
- [AFC_SELECT_LANE](../klipper/internal/lane.md#AFC_unit.afcUnit.cmd_AFC_SELECT_LANE)

AFC_HTLF inherits configuration options from AFC_BoxTurtle configuration section, below are additional configuration values
for a HTLF unit.  
``` cfg

[AFC_HTLF HTLF_1]
drive_stepper:
#    Name of AFC_stepper for drive motor.
selector_stepper:
#    Name of AFC_stepper for selector motor.
mm_move_per_rotation: 32
#    Default: 32
#    How many millimeters move the selector pulley one full rotation.
cam_angle:
#    Cam lobe angle thats being used on HTLF. Valid options: 30,45,60 (recommend using 60)
home_pin:
#    Pin for homing sensor.
MAX_ANGLE_MOVEMENT: 215
#    Default: 215
#    Max angle to move lobes, this is when lobe 1 is fully engaged with its lane.
enable_sensors_in_gui: True
#    Default: True
#    Set to True to show prep and load sensors switches as filament sensors 
#    in Mainsail/Fluidd gui, overrides value set in AFC.cfg.
selector_movement_speed: 50
#    Default: 50
#    Speed in mm/s to move filament when selecting lanes with selector motor.
selector_movement_accel: 50
#    Default: 50
#    Acceleration in mm/s squared when selecting lanes with selector motor.
```

## [AFC_vivid unit_name] Section

The following options are available in the `[AFC_vivid unit_name]` section of the `AFC_UnitType_1.cfg` file. These
options control the configuration of the AFC system when interfacing with the AFC_vivid unit type. This section is
typically used to define the unit name and other options that are specific to the AFC_vivid unit type. 

The following macros are specific to ViViD:  
- [AFC_SELECT_LANE](../klipper/internal/lane.md#AFC_unit.afcUnit.cmd_AFC_SELECT_LANE)  
- [AFC_RECOVER_LANE](../klipper/internal/lane.md#AFC_lane.AFCLane.cmd_AFC_RECOVER_LANE)  
- [AFC_UNSELECT_LANE](../klipper/internal/lane.md#AFC_unit.afcUnit.cmd_AFC_UNSELECT_LANE)

AFC_vivid inherits configuration options from AFC_BoxTurtle configuration section, below are additional configuration values
for a ViViD unit.  
``` cfg

[AFC_vivid Vivid_1]
drive_stepper:
#    Name of AFC_stepper for drive motor.
selector_stepper:
#    Name of AFC_stepper for selector motor.
enable_sensors_in_gui: True
#    Default: True
#    Set to True to show prep and load sensors switches as filament sensors 
#    in Mainsail/Fluidd gui, overrides value set in AFC.cfg.
selector_movement_speed: 150
#    Default: 150
#    Speed in mm/s to move filament when selecting lanes with selector motor.
selector_movement_accel: 150
#    Default: 150
#    Acceleration in mm/s squared when selecting lanes with selector motor.
max_selector_movement: 800
#    Default: 800
#    Max movement in mm to try to move selector to specified lane.
```

## [AFC_EMU unit_name] Section

The following options are available in the `[AFC_EMU unit_name]` section of the `AFC_UnitType_1.cfg` file. These
options control the configuration of the AFC system when interfacing with the EMU unit type. This section is
typically used to define the unit name and other options that are specific to the EMU unit type.
 

AFC_EMU inherits configuration options from AFC_BoxTurtle configuration section, below are additional configuration values
for a EMU unit.  

``` cfg
[AFC_EMU EMU_1]
```

### Notes about default EMU config setup
EMU default configuration is setup to run with a sensorless hub, what this means is that filament is parked behind the load sensor instead of at the hub.
Verify that your hub configuration section has `switch_pin: virtual` and `use_dist_hub: True` set. Setting switch pin as virtual means that all your load
sensors become the hub sensor, so once one load sensor is triggered then AFC will think that your hub has filament in the path to your toolhead. The `use_dist_hub`
variable is needed to tell AFC to use the `dist_hub` variable from each lane instead of `afc_bowden_length` since distance from load switch to toolhead can
differ per lane.

To switch to a hub that uses a sensor, remove `use_dist_hub: True` and set your hubs switch_pin correctly in your AFC_hub configuration section. Then make sure you
update `dist_hub` in each `AFC_stepper` section to be around what the PTFE length is between the load sensor and hub sensor. Make sure you run calibration as this
will then be calibrated to the correct distance.

## [servo tool_cut] Section

The following options are available for all units (BoxTurtle, HTLF, and NightOwl) and allow for the configuration and 
operation of a gantry mounted servo to assist in tool cutting. 

``` cfg
[servo tool_cut]
#--=================================================================================--#
######### Tip Cut Servo ###############################################################
#--=================================================================================--#
pin: YOUR_SERVO_PIN_HERE
#    This should be the MCU/pin combination for the servo pin.
minimum_pulse_width: 0.00053
#    Adjust this value until a '_CUTTER_SERVO POS=out' extends the pin 
#    fully without a buzzing sound.
maximum_pulse_width: 0.0023
#    Adjust this value until a '_CUTTER_SERVO POS=in' retracts the pin 
#    fully without a buzzing sound.
maximum_servo_angle: 180
#   Initial angle (in degrees) to set the servo to. The default is to
#   not send any signal at startup.

```
