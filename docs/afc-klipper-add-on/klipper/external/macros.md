## External Macros

These macros are available in the Mainsail/Fluidd interface and generally serve as a wrapper around the internal 
commands.

In a default installation, these macros are available in the `~/printer_data/config/AFC/macros/AFC_macros.cfg` file.

-----
### [BT_TOOL_UNLOAD]

Unloads the currently loaded lane.

!!! Usage
    ```plaintext
    BT_TOOL_UNLOAD
    ```

-----
### [BT_CHANGE_TOOL]

Switch to a new lane by ejecting the previously loaded one and then load the lane specified by LANE parameter. 
Lane parameter is an integer and defaults to 1. 

!!! Usage
    ```plaintext
    BT_CHANGE_TOOL LANE=<int>
    ```

-----
### [BT_LANE_EJECT]

Fully eject the filament from the lane so spool can be removed. 
Lane parameter is an integer and defaults to 1. ex. 

!!! Usage
    ```plaintext
    BT_LANE_EJECT LANE=2
    ```

-----
### [BT_LANE_MOVE]
Move the specified lane the specified amount. 
Lane parameter is an integer and defaults to 1. 
Distance parameter is and integer and defaults to 20. This value can also be negative.
Distance over 200 uses long load speeds. 

!!! Usage 
    ```
    BT_LANE_MOVE LANE=2 DISTANCE=100
    ```

-----
### [BT_RESUME]
Resume the print after an error.
Using the normal resume will also call AFC_RESUME.

!!! Usage
    ```plaintext
    BT_RESUME
    ```

-----
### [BT_PREP]
Run the AFC PREP sequence.

!!! Usage
    ```plaintext
    BT_PREP
    ```