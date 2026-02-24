The AFC-Klipper-Add-On supports the use of multiple units. They can be any combination of supported unit types to include:

- BoxTurtle
- HTLF
- NightOwl
- QuattroBox
- ViViD
- etc.

The `install-afc.sh` script has an option to install additional units. When running the script, you can select `A`
in order to access the additional unit menu. From there, you can select which additional units you would like to install. 

!!! note

    You must have at least one unit installed in order to install an additional unit.

When installing additional units, the majority of the configuration files will be placed properly, however you *will*
need to perform some manual configuration in order to get the additional units working properly. This is because of
the myriad of different configurations that are possible.

Areas that will require manual configuration include:

- Updating lane numbering in the additional unit configuration file.
- Update the AFC_Indicator configuration to include the additional unit.

For example, if you were to install a BTT ViViD when a 4 lane boxturtle already existed, you would need to:

- Manually change the lane numbering from 1-4 to 5-8, and 
- Update all `AFC_Indicator_1` to `AFC_Indicator_3` and `AFC_Indicator_2` to `AFC_Indicator_4`
