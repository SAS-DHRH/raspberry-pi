# Chromium (X11 version)

Raspberry Pi OS configured to automatically boot into a full page web kiosk mode. This Chromium-based solution is built from scratch and thus is highly customisable (build as you like!).

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

For installation, setup and system administration, you will also need a keyboard.

<br />

## Installation

### 1. Prepare MicroSD card

1. Download and install [Raspberry Pi Imager](https://www.raspberrypi.com/software/) on your computer (not your Raspberry Pi).

1. Insert your MicroSD card into your computer (not your Raspberry Pi), using a MicroSD card reader.

1. Start up Raspberry Pi Imager, and configure it as follows and when you are done, click **NEXT**.

   **Raspberry Pi Device**: Select `NO FILTERING`.

   **Operating System**: Select `Raspberry Pi OS (other)` > `Raspberry Pi OS (Legacy, 32-bit) Lite` (A port of Debian Bullseye with security updates and no desktop environment)

   **Storage**: Select your MicroSD card.

1. In the **Use OS customisation?** prompt window, select **EDIT SETTINGS**.

1. In the **OS Customisation** window, configure the following settings, and when you are done, click **SAVE** to save your customisation settings and close the window.

   **GENERAL**

   - **Set hostname**: Enter a name for your Raspberry Pi. The name can be anything but should only contain alphabet, number and dash characters e.g. `kiosk-pi`.

   * **Set username and password**

     * **Username**: Enter your choice of username e.g. `pi`.
     * **Password**: Enter your choice of password e.g. `letmein!23`.

     > **Note**: During OS installation, a user account will be created on your Raspberry Pi based on the information you provide here, and you will be using the username and password to log in to that user account on your Raspberry Pi.

   * **Configure wireless LAN**

     * **SSID**: Enter the name of your WiFi network/connection e.g. `UoL Conferences`.
     * **Password**: Enter the password for your WiFi network/connection. Leave blank if no password is required.
     * **Wireless LAN country**: Select the country code for the country for which your WiFi router is configured e.g. `GB`.

   * **Set locale settings**

     * **Time zone**:  Select the timezone you would like your Raspberry Pi to use e.g. `Europe/London`.
     * **Keyboard layout**: Select the keyboard layout appropriate to the keyboard you are using with your Raspberry Pi e.g. `gb`. 

   **SERVICES**

      * **Enable SSH**: Tick/check this option.

      * **Use password authentication**: Select this option.

1. Upon returning to the **Use OS customisation?** prompt window, select **YES** to applying your OS customisation settings.

1. Select **YES** to erasing all existing data on your MicroSD card. If Raspberry Pi Imager asks you for a password, enter the password you use to log into your computer (not the password you set for your Raspberry Pi).

1. Once Raspberry Pi Imager has finished flashing Raspberry Pi OS to your MicroSD card, eject the card from your computer. Raspberry Pi Imager is by default set to eject media automatically - if you don't see your MicroSD card mounted on your Desktop or in Finder/File Explorer, you can simply remove it from the MicroSD card reader.

### 2. Install Raspberry Pi OS on Raspberry Pi

The examples in the following steps will use Raspberry Pi OS configured with:

- **Hostname**: kiosk-pi
- **Username**: pi
- **Password**: letmein!23

1. Insert your MicroSD card into your Raspberry Pi.

2. Connect your Raspberry Pi to a display monitor, keyboard and mouse.

3. Connect your Raspberry Pi to a power source and boot it up.

4. Once your Raspberry Pi has completed booting up (this may take 3-5 minutes the first time around), you should see a login prompt like the following:

   ```bash
   Raspibian GNU/Linux 12 kiosk-pi tty1
   
   kiosk-pi login:
   ```

5. Log into your Raspberry Pi.

   Type your username and press the **Enter** key, e.g.

   ```bash
   kiosk-pi login: pi
   ```

   You will then be asked for a password. Type your password and press the **Enter** key, e.g.

   ```bash
   Password: letmein!23
   ```

6. You should now see a command prompt like this:

   ```bash
   pi@kiosk-pi:~$
   ```

### 3. Update Raspberry Pi OS (optional, recommended)

1. Update the system package list.

   ```bash
   sudo apt update
   ```

1. Apply any available upgrades.

   ```bash
   sudo apt upgrade -y
   ```

1. Reboot your Raspberry Pi.

   ```bash
   sudo reboot
   ```

### 4. Configure Raspberry Pi OS

1. Start the raspi-config tool.

   ```bash
   sudo raspi-config
   ```

1. Enable console autologin.

   1. Using the Up/Down key, select **System Options** and press the **Enter** key.

   1. Using the Up/Down key, select **Boot / Auto Login** and press the **Enter** key.

   1. Using the Up/Down key, select **Console Autologin** and press the **Enter** key.

1. Disable overscan. 

   1. Using the Up/Down key, select **Display Options** and press the **Enter** key.

   1. Using the Up/Down key, select **Underscan** and press the **Enter** key.

   1. When asked **Would you like to enable overscan compensation for HDMI-1?**, using the Tab key, select **\<No>** and press the **Enter** key.

   1. With **\<Ok>** selected, press the Enter key to confirm **Display overscan compensation for HDMI-1 is disabled**.

1. Once back in the main menu, using the Tab key, select **\<Finish>** and press the **Enter** key.

1. When asked **Would you like to reboot now?**, using the Tab key, select **\<Yes>** and press the **Enter** key.

### 5. Install Graphical User Interface (GUI) components

We will be intalling the bare minimum GUI components needed to display Chromium web browser on a display monitor.

The graphical environment for GNU/Linux usally consists of four parts: X server (or X Window System display server), window manager, desktop environment, and login manager. However, since we are running a single application (i.e. Chromium) in full screen/kiosk mode and as given, we've no need for a desktop environment or window manager. We have the Raspberry Pi OS already configured to allow autologin in the previous step, so we don't need a login manager either. All we therefore need is X server here.

If you are planning on customizing the Chromium window, keyboard shortcuts/bindings or mouse behaviour, you will need to install a window manager. If you do need a window manager, add it (e.g. [openbox](http://openbox.org/wiki/Main_Page) or [matchbox-window-manager](https://www.yoctoproject.org/software-item/matchbox/)) to the following command, or install it separately later.

   ```bash
   sudo apt install --no-install-recommends xserver-xorg x11-xserver-utils xinit -y
   ```

   What is being installed:

- **xserver-xorg** is the [X.Org Foundation](https://x.org/wiki/)'s implementation of X server, and provides an interface to devices like displays, keyboards, mice and video cards.

   * **x11-xserver-utils** is an X client program that provides interface with an X server.
   * **xinit** is a program that allows us to manually start an X server. We will be using its frontend script called `startx` to start the X server and run Chromium web browser.

Options used:

- `--no-install-recommends` to prevent installation of recommended packages that are not strictly required by the packages we are installing.
- `-y` to instruct the command to assume "yes" as answer to all prompts and run non-interactively.

### 6. Install Chromium web browser

   ```bash
   sudo apt install --no-install-recommends chromium-browser -y
   ```

As we did in the previous step, we are using the `--no-install-recommends` option again to install only the absoluely necessary, and `-y` option to run the command non-interactively.

### 7. Set up kiosk mode

   1. Create a new file in your home directory and name it `.xinitrc` (note that the name starts with a dot):

      ```bash
      nano ~/.xinitrc
      ```

      The `.xinitrc` file is automatically used and executed by `startx` (and `xinit`) when it is present in a user's home directory (note: it will be ignored if it is located elsewhere!). We'll leverage this and put the kiosk display configurations and the command to launch Chromium in the file.

   1. Add display settings:

      ```bash
      ## Display settings
      xset -dpms      # turn off display power management
      xset s noblank  # turn off screen blanking
      xset s off      # turn off screen saver
      ```
   1. Add X server settings:

      ```bash
      ## X server settings
      setxkbmap -option terminate:ctrl_alt_bksp # allow termination with Ctrl+Alt+Backspace key combo
      ```

   1. Add Chromium settings:

      ```bash
      ## Chromium settings
      # Prevent Chromium from throwing flags or pop-up warning bars upon error             
      sed -i 's/"exited_cleanly":false/"exited_cleanly":true/' ~/.config/chromium/Default/Preferences
      sed -i 's/"exit_type":"Crashed"/"exit_type":"Normal"/' ~/.config/chromium/Default/Preferences
      ```

   1. Add the command to launch Chromium in kiosk mode:

      ```bash
      ## Launch Chromium
      # Get screen width and height
      GEOMETRY="$(fbset -s | awk '$1 == "geometry" { print $2":"$3 }')"
      WIDTH=$(echo "$GEOMETRY" | cut -d: -f1)
      HEIGHT=$(echo "$GEOMETRY" | cut -d: -f2)
      
      unclutter & 
      chromium-browser $KIOSK_URL \
         --kiosk \
         --incognito \
         --start-fullscreen \
         --start-maximized \
         --window-position=0,0 \
         --window-size=$WIDTH,$HEIGHT \
         --no-default-browser-check \
         --noerrdialogs \
         --no-first-run \
         --disable-infobars \
         --disable-pinch \
         --disable-features=TranslateUI \
         --overscroll-history-navigation=0 \
         --disk-cache-dir=/dev/null \
         --check-for-update-interval=31536000
      ```

      `$KIOSK_URL` should be typed as it is i.e. as an environmental variable name. Do not replace it with the actual URL to display. We'll be setting a value for the variable in a more appropriate place shortly.

      Adjust Chromium arguments as required, see [Run Chromium with command-line switches](https://www.chromium.org/developers/how-tos/run-chromium-with-flags/) and [List of Chromium command line switches](https://peter.sh/experiments/chromium-command-line-switches/).

   1. Put all together, the content of the `.xinitrc` file should now look like this:

      ```bash      
      ## Display settings
      xset -dpms      # turn off display power management
      xset s noblank  # turn off screen blanking
      xset s off      # turn off screen saver
      
      ## X server settings
      setxkbmap -option terminate:ctrl_alt_bksp # allow termination with the Ctrl+Alt+Backspace key combo
      
      ## Chromium settings
      # Prevent Chromium from throwing flags or pop-up warning bars upon error                   
      sed -i 's/"exited_cleanly":false/"exited_cleanly":true/' ~/.config/chromium/Default/Preferences
      sed -i 's/"exit_type":"Crashed"/"exit_type":"Normal"/' ~/.config/chromium/Default/Preferences
      
      ## Launch Chromium
      # Get screen width and height
      GEOMETRY="$(fbset -s | awk '$1 == "geometry" { print $2":"$3 }')"
      WIDTH=$(echo "$GEOMETRY" | cut -d: -f1)
      HEIGHT=$(echo "$GEOMETRY" | cut -d: -f2)
      
      unclutter & 
      chromium-browser $KIOSK_URL \
         --kiosk \
         --incognito \
         --start-fullscreen \
         --start-maximized \
         --window-position=0,0 \
         --window-size=$WIDTH,$HEIGHT \
         --no-default-browser-check \
         --noerrdialogs \
         --no-first-run \
         --disable-infobars \
         --disable-pinch \
         --disable-features=TranslateUI \
         --overscroll-history-navigation=0 \
         --disk-cache-dir=/dev/null \
         --check-for-update-interval=31536000
      ```

   1. Press the **Ctrl**+**X** keys, type **Y** to save the changes, and press the **Enter** key to confirm.

   1. Set a value for the envrionmental variable KIOSK_URL.

      1. Open the `.bash_profile` file (note that the name starts with a dot):

         ```bash
         nano ~/.bash_profile
         ```
         
         Note: Don't worry if the file doesn't exist yet, the above command will still work and create a new file named `.bash_profile` to be saved in your home directory.

         The `.bash_profile` file is a bash script file which is automatically executed upon login when it is present in a user's home directory (note: it will be ignored if it is located elsewhere!). It is mainly used to set user-specific custom configurations for a terminal session, such as the environment variable for the user or define scripts to perform the automated tasks at the startup.

      1. Add the following line, replacing the `<KIOSK_URL>` placeholder with the URL of a webpage to display in kiosk mode:

         ```bash
         # Set environmental variable
         export KIOSK_URL=<KIOSK_URL>
         ```

         Example:

         ```bash
         # Set KIOSK_URL
         export KIOSK_URL=https://www.london.ac.uk/about/services/senate-house-library/exhibitions/shakespeares-first-folios-400-year-journeys
         ```

      1. Press the **Ctrl**+**X** keys, type **Y** to save the changes, and press the **Enter** key to confirm.

         If you are following the installation step-by-step, then you can keep the `.bash_prfoile` open as we'll be adding another line to it in the next step.

### 8. Configure autostart

   1. Open the `.bash_profile` file:

      ```bash
      nano ~/.bash_profile
      ```

      Note: Don't worry if the file doesn't exist yet, the command will still work and create a new file named `.bash_profile`.

   1. Add the following *AFTER* the enviromental variable is set:

      ```bash
      # Start X server on boot, without the cursor
      [[ -z $DISPLAY && $XDG_VTNR -eq 1 ]] && startx -- -nocursor
      ```

   1. The content of the `.bash_profile` file should now look like this:

      ```bash
      ## Set environmental variable
      export KIOSK_URL=https://www.london.ac.uk/about/services/senate-house-library/exhibitions/shakespeares-first-folios-400-year-journey
      
      ## Start X server on boot, without the cursor
      # To start with the cursor, remove ` -- -nocursor` so that it is just `startx`
      [[ -z $DISPLAY && $XDG_VTNR -eq 1 ]] && startx -- -nocursor
      ```

   1. Press the **Ctrl**+**X** keys, type **Y** to save the changes, and press the **Enter** key to confirm.

### 9. Finally...

Reboot your Raspberry Pi.

```bash
sudo reboot
```

<br />

## Usage

### Start a terminal session

1. Press the **Ctrl**+**Alt**+**[Fn]**+**F2** keys to switch to terminal.
2. Log in with your username and password.

To end the terminal session and switch back to kiosk display:

1. Make sure to logout first by typing `logout` into the terminal window and pressing the **Enter** key.
2. Press the **Ctrl**+**Alt**+**[Fn]**+**F1** keys.

### Safe shutdown

Press the **[Fn]**+**esc** keys.

Note: Raspberry Pi can be rebooted by unplugging from the power source and plugging back again, but this does not allow Raspberry Pi to shut down safely and can result in a corrupt MicroSD card or file system. It is strongly recommended that you shut down safely using the above method.

### Change the kiosk's URL

1. [Start a terminal session](#start-a-terminal-session), or [remote access your Raspberry Pi via SSH](#remote-access-your-raspberry-pi-via-ssh).

1. Open the `.bash_profile` file:

   ```bash
   nano ~/.bash_profile
   ```

1. Look for the following line, and edit it, replacing the `<CURRENT_KIOSK_URL>` placeholder with the new URL:

   ```bash
   export KIOSK_URL=<CURRENT_KIOSK_URL>
   ```

1. Press the **Ctrl**+**x** keys, type **Y** to save the changes, and press the **Enter** key to confirm.

1. Reboot your Raspberry Pi.

   ```bash
   sudo reboot
   ```

### Reboot after Chromium or X server crashes

1. Kill the X server by pressing the **Ctrl**+**Alt**+**Backspace** keys. This gets you into the terminal session in which X server and Chromium are/were running.

2. Restart the X server.

   ```bash
   startx -- -nocursor
   ```

   Or to restart the X server with the cursor visible:

   ```bash
   startx
   ```

### Remote access your Raspberry Pi via SSH

See [Raspberry Pi documentation on remote access](https://www.raspberrypi.com/documentation/computers/remote-access.html), and how-to for [Windows](https://www.raspberrypi.com/documentation/computers/remote-access.html#secure-shell-from-windows-10) or [Linux/Mac](https://www.raspberrypi.com/documentation/computers/remote-access.html#secure-shell-from-linux-or-mac-os).

<br />

## Notes

### Security and energy-saving

To harden security and reduce unnecessary energy consumption, consider:

* Setting up SSH to use keys instead of passwords.
* Disabling USB and Ethernet ports, Bluetooth and on-board LEDs.
* Setting up auto-recovery in the event of crashing.
* Setting up a schedueled task to shutdown the Raspberry Pi, and power it down/up using a timer socket/plug. Be sure to leave a few minutes between shutdown and power down, to allow Raspberry Pi to safely shutdown.
* Write-protecting the boot partition.

### Keyboard and mouse

If/where the kiosk requires a keyboard (e.g. to allow users to fill in a form), use on-screen keyboard (customised with e.g. ctrl, alt, fn, function keys, etc removed) rather than an external/physical keyboard.

In addition to the hotkeys already covered in the [Usage](#usage), the following function keys are also active in Chromium in kiosk mode:
* **F2** forwards the Chromium window to Google Chrome Help page.
* **F3** opens find bar to search currect page, which can be exited with the **esc** key but this is not indicated nor immediately obvious.
* **F7** pops up a dialogue box for turning on/off caret navigation/browsing.

The function keys on external/physical keyboards can be re-mapped (e.g. to do nothing when pressed), but this could potentially result in losing the keyboard functionality needed for operational and maintenance tasks.

Similarly, the mouse functions are active, although the cursor is disabled and not visible on screen.

### Autorefresh

If the display content needs to be refreshed regularly e.g. to fetch and display realtime data:

* Timed autorefresh can be scheduled with a cron job and using `xdotool`, a command line tool with which keyboard input like the shift+ctrl+r or shift+F5 hotkeys can be programatically simulated.

* With timed autorefresh that depends on user activity/inactivity, such as at regular intervals but only refresh if the kiosk display is not being used (so as not to interrupt browsing) or to return the display to home page after a period of idleness, this is best achieved with javascript e.g. a Chrome exetension or built into the web site/app. But be very careful with Chrome extensions - only install from a trusted developer, or safer still, write your own extension.

### Webpage

* Avoid linking to external webpage.
* Disable cookie consent on the webpage when access from the Raspberry Pi for displaying in kiosk mode.
* Consider designing the website so that naviagation is possible with mouse/touch alone and without the keyboard.
* On the web pages displayed, set the html lang (e.g. to 'en') to prevent Chromium from triggering page translation prompts.

<br /><br />

\---

Michael Donnay and Kunika Kono, [Digital Humanities Research Hub (DHRH)](https://www.sas.ac.uk/digital-humanities), School of Advanced Study (SAS), University of London.  

:octocat: Find us on GitHub at https://github.com/SAS-DHRH