# Types of Commands

There are two types of commands that are present in the AFC system. These can be divided to be either `internal` 
commands or `external` macros.

Internal commands are commands that are present within the python code of the AFC system. These commands usually will 
not show up in GUIs such as Mainsail or Fluidd, due to the way that these systems detect macros. To expose these 
commands to the end-user, they are commonly wrapped with an `external` macro.

An example of this is the `LANE_MOVE` command. An external macro called `BT_LANE_MOVE` is wrapped around this internal 
command in order to make it easier to use.

The `internal` commands are subdivided into categories based on their functionality. These categories are:

- **Spool Commands**: These commands are used to manage the spool and its properties.
- **Calibration Commands**: These commands are used to help calibrate the AFC system. Most of them will provide a guided 
  prompt to help you through the process.
- **Tip Forming Commands**: These commands are used to manage the tip forming process.
- **Buffer Commands**: These commands are used to manage the buffer and its properties.
- **Lane / Movement Commands**: These commands are used to manage the lane and its properties.
- **Miscellaneous Commands**: These commands are used for various purposes, including error handling, tool changes, and 
  AFC status updates.




