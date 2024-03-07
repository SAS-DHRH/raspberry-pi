# FullPageOS

[FullPageOS](https://www.otot.tv/fullpageos/) is a Raspberry Pi distribution that automatically boots a Raspberry Pi into a full page web kiosk mode. This is a Chromium-based solution and subject to [some considerations](./Chromium-X11.md#notes), but it is quick and easy to install and run.

<br />

## Hardware

* Raspberry Pi 3B/3B+/4B or Raspberry Pi Zero W/WH/2W
* Power supply for Raspberry Pi
* MicroSD card
* MicroSD card reader
* Display screen/monitor
* Power supply for the display screen/monitor
* HDMI (Raspberry Pi 3) or Micro HDMI (Raspberry Pi 4) or Mini HDMI (Raspberry Pi Zero) cable
* Optionally, heatsink and/or fan for Raspberry Pi

For installation, setup and system administration, you will also need another computer.

<br />

## Installation

### 1. Prepare MicroSD card

1. Download and install [Raspberry Pi Imager](https://www.raspberrypi.com/software/) on your computer (not your Raspberry Pi).

1. Insert your MicroSD card into your computer (not your Raspberry Pi), using a MicroSD card reader.

1. Start up Raspberry Pi Imager, and configure it as follows and when you are done, click **NEXT**.

   **Raspberry Pi Device**: Select `NO FILTERING`.

   **Operating System**: Select `Other specific-purpose OS` > `FullPageOS` > `FullPageOS (Stable)`

   **Storage**: Select your MicroSD card.

1. In the **Use OS customisation?** prompt window, select **EDIT SETTINGS**.

1. In the **OS Customisation** window, configure the following settings, and when you are done, click **SAVE** to save your customisation settings and close the window.

   **GENERAL**

   * **Set username and password**

     * **Username**: Enter your choice of username e.g. `pi`.
     * **Password**: Enter your choice of password e.g. `letmein!23`.

   > **Notes**:
   >
   > - During OS installation, a user account will be created on your Raspberry Pi based on the information you provide here, and you will be using the username and password to log in to that user account on your Raspberry Pi.s
   > - You don't need to configure wireless LAN (WiFi) here, as it will be configured separately in a later step.

   **SERVICES**

      * **Enable SSH**: Select this option.

      * **Use password authentication**: Select this option.

   **OPTIONS**

   - **Eject media when finished**: Unselect this option.

1. On returning to the **Use OS customisation?** prompt window, select **YES** to applying your OS customisation settings.

1. Select **YES** to erasing all existing data on your MicroSD card. If Raspberry Pi Imager asks you for a password, enter the password you use to log into your computer (not the password you set for your Raspberry Pi).

1. Once Raspberry Pi Imager has finished flashing FullPageOS to your MicroSD card, double-click on your MicroSD card and open it.

      > **Note**: Your MicroSD card should still be mounted on your computer (you should see a disk named **boot** on your Desktop or Finder/File Explorer window), but if it has been ejected, re-insert it into your computer again.

      You should see lots of files and folders, amongst which the following files:

      - fullpageos-wpa-supplicant.txt
      - fullpageos.txt
      - Fullpagedashboard.txt

1. Open **fullpageos-wpa-supplicant.txt** and configure your WiFi setting. Make sure you use a plain text editor, or a text editor in plain text mode, for this. Recommended text editors: Notepad++, SublimeText, VSCode, Atom.

   If you are using a secure WiFi, uncomment the network setting under `##WPA/WPA2 secured` and replace the placeholder text with your own WiFi SSID (the name of your WiFi network e.g. 'Cats Cats Cats') and psk (the password for your WiFi network e.g. 'ilovecats') e.g.

   ```bash
   ## WPA/WPA2 secured
   network={
     ssid="Cats Cats Cats"
     psk="ilovecats"
   }
   ```

   If you are using an unsecure/public WiFi, uncomment the network setting under `##Open/unsecured` and replace the placeholder text with your own WiFi SSID (the name of your WiFi network e.g. 'UoL Conferences') e.g.

   ```bash
   ## Open/unsecured
   network={
     ssid="UoL Conferences"
     key_mgmt=NONE
   }
   ```

1. Open **fullpageos.txt** using a plain text editor and replace the existing line `http://localhost/FullPageDashboard` with the URL of the webpage you wish to display e.g.

      ```bash
      https://www.london.ac.uk/about/services/senate-house-library/exhibitions/shakespeares-first-folios-400-year-journey
      ```

1. Open **fullpagedashboard.txt** using a plain text editor and replace the existing line `http://localhost/welcome` with the URL of the webpage you wish to display e.g.

      ```bash
      https://www.london.ac.uk/about/services/senate-house-library/exhibitions/shakespeares-first-folios-400-year-journeys
      ```

1. Safely eject your MicroSD card from your computer.

### 2. Install FullPageOS on Raspberry Pi

1. Insert your MicroSD card into your Raspberry Pi.
2. Connect your Raspberry Pi to a display monitor, keyboard and mouse.
3. Connect your Raspberry Pi to a power source and boot it up.
4. Once your Raspberry Pi has completed booting up (this may take 3-5 minutes the first time around), you should see your webpage displayed in kiosk mode on your display monitor screen.

<br />

## Usage

### Start a terminal session

1. Press the **Ctrl**+**Alt**+**[Fn]**+**F2** keys.
2. Log in with your username and password.

To end the terminal session and switch back to kiosk display:

1. Make sure to logout first by typing `logout` into the terminal window and pressing the **Enter** key.
2. Press the **Ctrl**+**Alt**+**[Fn]**+**F7** keys.

### Safe shutdown

Press the **[Fn]**+**esc** keys.

Note: Raspberry Pi can be shutdown by unplugging from the power source, but this does not allow Raspberry Pi to shut down safely and can result in a corrupt MicroSD card or file system. It is strongly recommended that you shut down safely using the above method.

### Change FullPageOS configurations

Note: Please refer to the steps #9-11 above for the instruction on the configuration settings.

To change the WiFi settings or the URL of the webpage to display:

1. Connect a keyboard to your Raspberry Pi.

2. Start a terminal session (see [Start a terminal session](#start-a-terminal-session)).

3. Log in with your username and password.

4. To change the WiFi settings, edit **fullpageos-wpa-supplicant.txt**.

   Open the file using a command-line text editor `nano`:

   ```bash
   nano /boot/fullpageos-wpa-supplicant.txt
   ```

   Edit as appropriate, and press the **Ctrlx**+**x** keys, type **Y** to save the changes, and press the **Enter** key to confirm.

5. To change the URL of the webpage to display, edit **fullpageos.txt** and **fullpagedashboard.txt**.

   Open each file using a command-line text editor `nano`:

   ```bash
   nano /boot/fullpageos.txt
   ```

   Edit as appropriate, and press the **Ctrlx**+**x** keys, type **Y** to save the changes, and press the **Enter** key to confirm. Do the same with **fullpagedashboard.txt**.

6. Reboot your Raspberry Pi.

   ```bash
   sudo reboot
   ```

Alternatively, you can:

1. Shutdown your Raspberry Pi and remove the MicroSD card.
2. Insert the MicroSD card into another computer (not your Raspberry Pi), and once mounted, double-click on it and open it.
3. Edit **fullpageos-wpa-supplicant.txt** to change the WiFi settings, or edit **fullpageos.txt** and **fullpagedashboard.txt** to change the URL of the webpage to display.
4. Eject the MicroSD card from your computer and re-insert into your Raspberry Pi.
5. Reboot your Raspberry Pi.

### Remote access your Raspberry Pi via SSH

See [Raspberry Pi documentation on remote access](https://www.raspberrypi.com/documentation/computers/remote-access.html), and how-to for [Windows](https://www.raspberrypi.com/documentation/computers/remote-access.html#secure-shell-from-windows-10) or [Linux/Mac](https://www.raspberrypi.com/documentation/computers/remote-access.html#secure-shell-from-linux-or-mac-os).

<br /><br />

\---

Michael Donnay and Kunika Kono, [Digital Humanities Research Hub (DHRH)](https://www.sas.ac.uk/digital-humanities), School of Advanced Study (SAS), University of London.  

:octocat: Find us on GitHub at https://github.com/SAS-DHRH
