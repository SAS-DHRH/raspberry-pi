# FullPageOS

[FullPageOS](https://www.otot.tv/fullpageos/) is a Raspberry Pi distribution that automatically boots a Raspberry Pi into a full page web kiosk mode. This is a Chromium-based solution and subject to [some considerations](./Chromium-X11.md#notes), but it is quick and easy to install and run.

<br />

## Hardware

* Raspberry Pi 3B/3B+/4B
* Power supply for Raspberry Pi
* MicroSD card  (16GB or larger, U1/Class 10 recommended)
* Optionally, heatsink and/or fan for Raspberry Pi
* Display monitor
* Power supply for the display monitor
* HDMI cable for connecting Raspberry Pi to the display monitor

**Additionally for installation:**

* Another computer (Windows, Mac or Linux)
* MicroSD card reader
* USB Keyboard
* USB Mouse

**Additionally for system administration and troubleshooting/debugging:**

* USB Keyboard
* USB Mouse

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

   During OS installation, a user account will be created on your Raspberry Pi based on the information you provide here, and you will be using the username and password to log in to that user account on your Raspberry Pi.s
   
   You don't need to configure wireless LAN (WiFi) here, as it will be configured separately in a later step.
   
   **SERVICES**
   
      * **Enable SSH**: Select this option.
   
      * **Use password authentication**: Select this option.
   
   **OPTIONS**
   
   - **Eject media when finished**: Unselect this option.
   
1. On returning to the **Use OS customisation?** prompt window, select **YES** to applying your OS customisation settings.

1. Select **YES** to erasing all existing data on your MicroSD card. If Raspberry Pi Imager asks you for a password, enter the password you use to log into your computer (not the password you set for your Raspberry Pi).

1. Once Raspberry Pi Imager has finished flashing FullPageOS to your MicroSD card, double-click on your MicroSD card and open it.

      Your MicroSD card should still be mounted on your computer and you should see a disk named **boot** on your Desktop or Finder/File Explorer window. If it has been ejected, re-insert your MicroSD card into your computer again.

      Inside your MicroSD card, you should see lots of files and folders, amongst which the following files:

      - fullpageos-wpa-supplicant.txt
      - fullpageos.txt
      - Fullpagedashboard.txt

      *You will be editing these file next. Please make sure to use a plain text editor, or a text editor in plain text mode. Recommended text editors: Notepad++, SublimeText, VSCodium, VSCode.*

1. Open **fullpageos-wpa-supplicant.txt** and configure your WiFi setting.

   If you are using a secure WiFi, uncomment the network setting under `##WPA/WPA2 secured` and replace the placeholder text with your own WiFi SSID (the name of your WiFi network e.g. 'Cats Cats Cats') and psk (the password for your WiFi network e.g. 'ilovecats') e.g.

   ```bash
   ## WPA/WPA2 secured
   network={
     ssid="Cats Cats Cats"
     psk="ilovecats"
   }
   ```

   If you are using an open/unsecure WiFi, uncomment the network setting under `##Open/unsecured` and replace the placeholder text with your own WiFi SSID (the name of your WiFi network e.g. 'UoL Conferences') e.g.

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
      https://www.london.ac.uk/about/services/senate-house-library/exhibitions/shakespeares-first-folios-400-year-journey
      ```

1. Safely eject your MicroSD card from your computer.

### 2. Install FullPageOS on Raspberry Pi

1. Insert your MicroSD card into your Raspberry Pi.

2. Connect your Raspberry Pi to a display monitor, keyboard and mouse.

3. Connect your Raspberry Pi to a power source and boot it up.

4. Once your Raspberry Pi has completed booting up (this may take 3-5 minutes the first time around), you should see your webpage displayed in kiosk mode on your display monitor screen.

   If you have configured your Raspberry Pi to use the open/unsecure WiFi, you may find that the kiosk display is blank and only showing a pointer cursor. See [Troubleshooting](#troubleshooting) for how to fix this.

<br />

## Usage

### Start a terminal session

1. Connect a keyboard to your Raspberry Pi.
2. Press the **Ctrl**+**Alt**+**F2** keys. Some keyboards may also require the **Fn** key i.e. **Ctrl**+**Alt**+**Fn**+**F2**.
3. Log into your Raspberry Pi with your username (e.g. `pi`) and password (e.g. `letmein!23`).

To end the terminal session and switch back to kiosk display:

1. Make sure to logout first by typing `logout` into the terminal window and pressing the **Enter** key.
2. Press the **Ctrl**+**Alt**+**F7** keys. Some keyboards may also require the **Fn** key i.e. **Ctrl**+**Alt**+**Fn**+**F7**.

### Safe shutdown

1. Connect a keyboard to your Raspberry Pi.
2. Press the **Fn**+**esc** keys.

Raspberry Pi can be shutdown by unplugging from the power source, but this does not allow Raspberry Pi to shut down safely and can result in a corrupt MicroSD card or file system. It is strongly recommended that you shut down safely using the above method.

### Change the URL of the webpage to display

1. [Start a terminal session](#start-a-terminal-session), or [remote access your Raspberry Pi via SSH](#remote-access-your-raspberry-pi-via-ssh).

2. Open the file **/boot/fullpageos.txt** using a command-line text editor e.g. `nano`:

   ```bash
   sudo nano /boot/fullpageos.txt
   ```

   Replace the existing URL with the new one, and press the **Ctrl**+**X** keys, type **Y** to save the changes, and press the **Enter** key to confirm the file name to write and close the file.

3. Open the file **/boot/fullpagedashboard.txt** using a command-line text editor e.g. `nano`:

   ```bash
   sudo nano /boot/fullpagedashboard.txt
   ```

   Replace the existing URL with the new one, and press the **Ctrl**+**X** keys, type **Y** to save the changes, and press the **Enter** key to confirm the file name to write and close the file.

4. Reboot your Raspberry Pi.

   ```bash
   sudo reboot
   ```

Alternatively, you can:

1. [Safe shutdown](#safe-shutdown) your Raspberry Pi and remove the MicroSD card.
2. Insert the MicroSD card into another computer (not your Raspberry Pi) , and once mounted, double-click on it and open it.
3. Open **fullpageos.txt** and **fullpagedashboard.txt** and replace the existing URL with the new one in both files, save and close the files. Make sure you use a plain text editor, or a text editor in plain text mode, for this.
4. Eject the MicroSD card from your computer and re-insert it into your Raspberry Pi.
5. Boot up your Raspberry Pi.

### Change the WiFi network

1. [Start a terminal session](#start-a-terminal-session), or [remote access your Raspberry Pi via SSH](#remote-access-your-raspberry-pi-via-ssh).

2. Open the file **/boot/fullpageos-wpa-supplicant.txt** using a command-line text editor e.g. `nano`:

   ```bash
   sudo nano /boot/fullpageos-wpa-supplicant.txt
   ```

   Edit your WiFi settings, and press the **Ctrl**+**X** keys, type **Y** to save the changes, and press the **Enter** key to confirm the file name to write and close the file.

3. Reboot your Raspberry Pi.

   ```bash
   sudo reboot
   ```

Alternatively, you can:

1. [Safe shutdown](#safe-shutdown) your Raspberry Pi and remove the MicroSD card.
2. Insert the MicroSD card into another computer (not your Raspberry Pi) , and once mounted, double-click on it and open it.
3. Open the file **fullpageos-wpa-supplicant.txt** and edit your WiFi settings, save and close the file. Make sure you use a plain text editor, or a text editor in plain text mode, for this.
4. Eject the MicroSD card from your computer and re-insert it into your Raspberry Pi.
5. Boot up your Raspberry Pi.

### Remote access your Raspberry Pi via SSH

See [Raspberry Pi documentation on remote access](https://www.raspberrypi.com/documentation/computers/remote-access.html), and how-to for [Windows](https://www.raspberrypi.com/documentation/computers/remote-access.html#secure-shell-from-windows-10) or [Linux/Mac](https://www.raspberrypi.com/documentation/computers/remote-access.html#secure-shell-from-linux-or-mac-os).

<br />

## Troubleshooting

### Blank screen after FullPageOS installation

1. Make sure to wait 3-5 minutes to give enough time for FullPageOS to complete installation. Proceed with the next steps if you are still seeing blank screen with only a pointer cursor showing in your display monitor.

2. [Start a terminal session](#start-a-terminal-session).

3. Open **fullpageos-wpa-supplicant.txt**

   ```bash
   sudo nano /boot/fullpageos-wpa-supplicant.txt
   ```

   If the content includes the lines like this:

   ```bash
   network={
     ssid="UoL Conferences"
     psk=
   }
   ```

   Edit and replace the `psk=` line with `key_mgmt=NONE` like this:

   ```bash
   network={
     ssid="UoL Conferences"
     key_mgmt=NONE
   }
   ```

4. Press the **Ctrl**+**X** keys, type **Y** to save the changes, and press the **Enter** key to confirm the file name to write (i.e. `/boot/fullpageos-wpa-supplicant.txt`)  and close the file.

5. Reboot your Raspberry Pi.

   ```bash
   sudo reboot
   ```

<br /><br />

\---

Michael Donnay and Kunika Kono, [Digital Humanities Research Hub (DHRH)](https://www.sas.ac.uk/digital-humanities), School of Advanced Study (SAS), University of London.  

:octocat: Find us on GitHub at https://github.com/SAS-DHRH
