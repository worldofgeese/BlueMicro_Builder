# BlueMicro_Builder
Initializes and builds your own BlueMicro_BLE firmware with a *Dvorak* keyboard layout for the Blue Wizard keyboard on *Linux*. 

## First Time: Initializating the firmware

- Create your own repository using the "Use this template" button above.
- Make sure that github actions is enabled
  - Go to settings (of your fork), 
  - Go down to the "Actions" section,
  - Select "Allow all actions"
- Edit one of the workflow files (located in .github\workflows) and edit the following entries (you can do this in the GitHub file editor):
  - keyboard: ['blue_wizard']
  - keymap: ['dvorak']
  - keyboard_config: ['single']
  - hardware_config: ['feather52832']
  - compile_with: ['feather52832']
- Go to "Actions" at the top of your repo
- Select the workflow you just edited, you should see a "Run Workflow" menu to the right
- Trigger Workflow.  This will run a GitHub Action to copy the necessary files from the BlueMicro_BLE firmware and copy over the keyboard and configuration files you have selected above.
- Once the action has completed, go back to the top of your repository, you should now see a folder that contains all the files you need to compile your own firmware.
- You will need to download the firmware to your computer, compile and flash it using the Arduino IDE. To do so, you can clone the repository or download it in a zip file.
- First, install `adafruit-nrfutil` needed to flash your firmware with `nix-env -iA python310.adafruit-nrfutil` if using Nix. If you don't have Nix install using [this](https://github.com/worldofgeese/provision-ubuntu-on-wsl2) Ansible playbook.
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
- For nRF52840 boards, an artifact was created with the action which will contain the UF2 file you can upload to your nRF52840 board.  Download the artifact zip file, uncompress and upload the UF2 file to yourboard.
