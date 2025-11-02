# Updating the AFC-Klipper-Add-On

Updating the AFC-Klipper-Add-On is a simple process that can be done through the `install-afc.sh` script.

!!!warning

    Do **NOT** use the Moonraker update functionality to update the `AFC-Klipper-Add-On` software. It will
    not run the necessary update scripts and may cause issues with your installation.


!!!note 

    If your AFC-Klipper-Add-On is < 1.0.20 (Any version prior to 15 Jun 2025), you should run a `git pull` in the 
    `~/AFC-Klipper-Add-On` directory before running the `install-afc.sh` script. This is NOT necessary if your 
    AFC-Klipper-Add-On is >= 1.0.20. 

1. Connect to your printer via SSH and navigate to the AFC-Klipper-Add-On directory:  
   
```bash
cd ~/AFC-Klipper-Add-On
```
   
2. Run the `install-afc.sh` script:  

```bash
./install-afc.sh
```
   
3. Select the `Update AFC Klipper Add-on` option from the menu that appears. This option will only
   appear if the system detects that the AFC-Klipper-Add-On is already installed.  

4. Once in the `Update AFC Klipper Add-on` menu, select the `Update AFC-Klipper-Add-On` option. This will
   update the add-on to the latest version available in the repository.
 
    You will be prompted if you would like the update the AFC provided macros during the update process.
    If you select `Yes`, the macros will be updated to the latest version available. If you select `No`, the macros will not be updated, and
    you will need to manually update them if you want to use the latest versions.

5.  After the update is complete, Moonraker may still show an old version. Use the refresh button in the Mainsail/Fluidd
    interface to update the version information.

!!!warning

    If you have made any changes to the AFC provided macros directory, those changes will be overwritten by the 
    update process. It is recommended to back up any custom macros before proceeding with the update.