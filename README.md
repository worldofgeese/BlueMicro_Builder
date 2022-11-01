# BlueMicro_Builder
Initializes and builds your own BlueMicro_BLE firmware with a *Dvorak* keyboard layout for the Blue Wizard module on *Linux*. The Blue Wizard is a module built around the Arduino Feather 52832 intended to upgrade Kinesis Advantage and Kinesis Advantage2 keyboards to be wireless Bluetooth-capable. 

The Dvorak layout is very close to stock with only the following keys changed:
```txt
[caps]>[rctrl]
[lalt]>[rwin]
[lctrl]>[lalt]
[rctrl]>[rwin]
[rwin]>[rctrl]
```

If you wish to have stock thumb clusters, copy the keymap under `\\ left thumb` and `\\ right thumb` from [this file](https://github.com/jpconstantineau/BlueMicro_BLE/blob/master/firmware/keyboards/blue_wizard/keymaps/default/keymap.cpp) to the `keymap.cpp` file under ./firmware/keyboards/blue_wizard/keymaps/dvorak/ in https://github.com/worldofgeese/BlueMicro_BLE.

## First Time: Initializating the firmware

- Go to "Actions" at the top of your repo
- Select the workflow `blue_wizard`, you should see a "Run Workflow" menu to the right
- Trigger Workflow. This will run a GitHub Action to copy the necessary files from the BlueMicro_BLE firmware and copy over the keyboard and configuration files you have selected above.
- Once the action has completed, go back to the top of your repository, you should now see a folder that contains all the files you need to compile your own firmware.
- You will need to download the firmware to your computer, compile and flash it using the Arduino IDE. To do so, you can clone the repository or download it in a zip file.
- First, install `adafruit-nrfutil` needed to flash your firmware with `nix-env -iA python310.adafruit-nrfutil` if using Nix. If you don't have Nix, first install Nix using [this](https://github.com/worldofgeese/provision-ubuntu-on-wsl2) Ansible playbook.
- Now [install](https://www.arduino.cc/en/software) the Arduino IDE. Grab the AppImage. I use [`appimaged`](https://github.com/probonopd/go-appimage/blob/master/src/appimaged/README.md) to have downloaded AppImages immediately available on my `$PATH`
- Discover the group owner of the `/dev/ttyUSB0` and add your `$USER` to the group then reboot:
    ```
    stat -c '%G' /dev/ttyUSB0
    sudo usermod -a -G uucp $USER
    sudo systemctl reboot
    ```
- Finally, insert a USB type-C data cable to the type-C port on your Blue Wizard board and plug the other end into your computer then follow the instructions given in [this video](https://youtu.be/hKw3TPNu-BQ?t=418) to flash your Blue Wizard with the Dvorak keyboard layout.

## Next Time: Re-build your firmware

- Go to "Actions" at the top of your repo
- Select the workflow you want to run and build, you should see a "Run Workflow" menu to the right.  
- Trigger Workflow.  This will run a GitHub Action. If the firmware folder already exist, the workflow will skip the initialization steps and will go directly to compiling and building your firmware.
