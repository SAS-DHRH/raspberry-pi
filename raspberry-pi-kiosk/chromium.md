# Raspberry Pi kiosk using Chromium

The following method sets up a Raspberry Pi as kiosk display, using [Chromium](https://www.chromium.org/Home/) web browser.

<br />

## Hardware

* Raspberry Pi 3B/3B+/4B or Raspberry Pi Zero W/WH/2W
* Power supply for Raspberry Pi
* MicroSD card
* Heatsink
* Display screen/monitor
* Power supply for the display screen/monitor
* HDMI (Raspberry Pi 3) or Micro HDMI (Raspberry Pi 4) or Mini HDMI (Raspberry Pi Zero) cable

For installation, setup and system administration, you will also need a keyboard.

<br />

## Installation and setup

### 1. Install Raspberry Pi OS

1. Using a computer (not your Raspberry Pi), install **Raspberry Pi OS Lite** (Bullseye or Buster) on your microSD card with [Raspberry Pi Imager](https://www.raspberrypi.com/software/) using the following configurations:

   * **Set hostname** to a name of your choice (note this down!)
   * **Enable SSH** and select **Use password authentication**
   * **Set username and password**. The username must be something other than `pi` for Bullseye (see [And update to Raspberry Pi OS Bullseye](https://www.raspberrypi.com/news/raspberry-pi-bullseye-update-april-2022/)). This is the username and password you will be using to log in to your Raspberry Pi (make sure to remember them!).
   * **Configure wireless LAN**. Enter the **SSID** (the name of your WiFi network) and if required, **password** to connect to the WiFi network. For **Wireless LAN country**, select `GB` .

1. Eject the microSD card from your computer and insert it into your Raspberry Pi.

1. Connect your Raspberry Pi to a display screen and keyboard, and then to a power source to boot up.

1. Once your Raspberry Pi has finished booting up (this may take 3-5 minutes the first time around, wait until the screen output stops completely), you should see a login prompt like this, with the `<hostname>` part showing the hostname you have chosen for your Raspberry Pi in step 1 above:
   
   ```bash
   <hostname> login: 
   ```

   Note: During the boot up, if you see a similar prompt `raspberrypi login:` in the terminal window, ignore it and wait for the prompt with your hostname.

   Log in to your Raspberry Pi using your username and password you set in step 1 above.

1. Update package list and apply full-upgrade to the operating system.

   ```bash
   sudo apt update
   sudo apt full-upgrade
   ```

### 2. Configure Raspberry Pi OS

1. Start the raspi-config Tool .

   ```bash
   sudo raspi-config
   ```

1. Enable console autologin. Select **System Options** > **Boot / Auto Login** > **Console Autologin**.

1. Disable overscan. Select **Display Options** > **Underscan**, then:

   **Would you like to enable overscan compensation for HDMI-1?** (Bullseye) or **Would you like to enable compensation for displays with overscan?** (Buster):  **No**

   **Display overscan compensation for HDMI-1 is disabled** (Bullseye) or **Display overscan compensation is disabled** (Buster): **Ok**

1. Once back in the main menu, select **Finish** then:

   **Would you like to reboot now?**: **Yes**

### 3. Install Graphical User Interface (GUI) components

   We will be intalling the bare minimum GUI components needed to display Chromium web browser on a screen/monitor.

   The graphical environment for GNU/Linux usally consists of four parts: X server (or X Window System display server), window manager, desktop environment, and login manager. However, since we are running a single application (i.e. Chromium) in full screen/kiosk mode and as given, we've no need for a desktop environment or window manager. We have the Raspberry Pi OS already configured to allow autologin in the previous step, so we don't need a login manager either. All we therefore need is X server here.
   
   If you are planning on customizing the Chromium window, keyboard shortcuts/bindings or mouse behaviour, you will need to install a window manager. If you do need a window manager, add one (e.g. [openbox](http://openbox.org/wiki/Main_Page) or [matchbox-window-manager](https://www.yoctoproject.org/software-item/matchbox/)) to the following command, or install it separately later.

   ```bash
   sudo apt install --no-install-recommends xserver-xorg x11-xserver-utils xinit
   ```

   What is being installed:

   * **xserver-xorg** is the [X.Org Foundation](https://x.org/wiki/)'s implementation of X server, and provides an interface to devices like displays, keyboards, mice and video cards.
   * **x11-xserver-utils** is an X client program that provides interface with an X server.
   * **xinit** is a program that allows us to manually start an X server. We will be using its frontend script called `startx` to start the X server and run Chromium web browser.

   The `--no-install-recommends` flag prevents installation of recommended packages that are not strictly required by the packages we are installing.
   
### 4. Install Chromium web browser

   ```bash
   sudo apt install --no-install-recommends chromium-browser
   ```

   As we did in the previous step, we are using the `--no-install-recommends` flag again to install only the absoluely necessary.

### 5. Configure kiosk display

   1. Make sure you are in your home directory:

      ```bash
      cd ~
      ```
   
   1. Create a new file and name it `.xinitrc` (note that the name starts with a dot):

      ```bash
      nano ~/.xinitrc
      ```

      The `.xinitrc` file is automatically used and executed by `startx` (and `xinit`) when it is present in a user's home directory (note: it will be ignored if it is located elsewhere!). We'll leverage this and put the kiosk display configurations and the command to launch Chromium in the file.

   1. Add display settings:

      ```bash
      # Display settings
      xset -dpms      # turn off display power management
      xset s noblank  # turn off screen blanking
      xset s off      # turn off screen saver
      ```
   1. Add X server settings:

      ```bash
      # X server settings
      setxkbmap -option terminate:ctrl_alt_bksp # allow termination with Ctrl+Alt+Backspace key combo
      ```

   1. Add Chromium settings:

      ```bash
      # Chromium settings
      # Prevent Chromium from throwing flags or pop-up warning bars upon error             
      sed -i 's/"exited_cleanly":false/"exited_cleanly":true/' ~/.config/chromium/Default/Preferences
      sed -i 's/"exit_type":"Crashed"/"exit_type":"Normal"/' ~/.config/chromium/Default/Preferences
      ```

   1. Add the command to launch Chromium in kiosk mode:

      ```bash
      # Launch Chromium
      chromium-browser $KIOSK_URL \
         --kiosk \
         --incognito \
         --noerrdialogs \
         --no-first-run \
         --disable-infobars \
         --disable-pinch \
         --overscroll-history-navigation=0 \
         --disk-cache-dir=/dev/null \
         --check-for-update-interval=31536000
      ```
      
      `$KIOSK_URL` should be typed as it is i.e. as an environmental variable name. Do not replace it with the actual URL to display. We'll be setting a value for the variable in a more appropriate place shortly.
      
      Adjust Chromium arguments as required, see [Run Chromium with command-line switches](https://www.chromium.org/developers/how-tos/run-chromium-with-flags/) and [List of Chromium command line switches](https://peter.sh/experiments/chromium-command-line-switches/).

   1. Put all together, the content of the `.xinitrc` file should now look like this:

      ```bash      
      # Display settings
      xset -dpms      # turn off display power management
      xset s noblank  # turn off screen blanking
      xset s off      # turn off screen saver

      # X server settings
      setxkbmap -option terminate:ctrl_alt_bksp # allow termination with the Ctrl+Alt+Backspace key combo
      
      # Chromium settings
      # Prevent Chromium from throwing flags or pop-up warning bars upon error                   
      sed -i 's/"exited_cleanly":false/"exited_cleanly":true/' ~/.config/chromium/Default/Preferences
      sed -i 's/"exit_type":"Crashed"/"exit_type":"Normal"/' ~/.config/chromium/Default/Preferences
      
      # Launch Chromium
      chromium-browser $KIOSK_URL \
         --kiosk \
         --incognito \
         --noerrdialogs \
         --no-first-run \
         --disable-infobars \
         --disable-pinch \
         --overscroll-history-navigation=0 \
         --disk-cache-dir=/dev/null \
         --check-for-update-interval=31536000
      ```

   1. Press the **Ctrlx**+**x** keys, type **Y** to save the changes, and press the **Enter** key to confirm.

   1. Set a value for the envrionmental variable KIOSK_URL

      1. Open the `.bash_profile` file (note that the name starts with a dot):

         ```bash
         nano ~/.bash_profile
         ```
         
         Note: Don't worry if the file doesn't exist yet, the above command will still work and create a new file named `.bash_profile` to be saved in your home directory.

         The `.bash_profile` file is a bash script file which is automatically executed upon login when it is present in a user's home directory (note: it will be ignored if it is located elsewhere!). It is mainly used to set user-specific custom configurations for a terminal session, such as the environment variable for the user or define scripts to perform the automated tasks at the startup.

      1. Add the following line, replacing the `<KIOSK_URL>` placeholder with the URL to open in Chromium:

         ```bash
         # Set environmental variable
         export KIOSK_URL=<KIOSK_URL>
         ```

         Example:

         ```bash
         # Set KIOSK_URL
         export KIOSK_URL=https://www.london.ac.uk
         ```

      1. [Optional] Press the **Ctrlx**+**x** keys, type **Y** to save the changes, and press the **Enter** key to confirm.

         If you are following the installation step-by-step, then you can keep the `.bash_prfoile` open as we'll be adding another line to it in the next step.

### 6. Configure autostart

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
      # Set environmental variable
      export KIOSK_URL=https://www.london.ac.uk

      # Start X server on boot, without the cursor
      [[ -z $DISPLAY && $XDG_VTNR -eq 1 ]] && startx -- -nocursor
      ``` 

   1. Press the **Ctrlx**+**x** keys, type **Y** to save the changes, and press the **Enter** key to confirm.

### 7. Finally...

Reboot your Raspberry Pi.

```bash
sudo reboot
```

<br />

## Usage

### Safe reboot

Press the **[Fn]**+**esc** keys.

Note: Raspberry Pi can be rebooted by unplugging from the power source and plugging back again, but this does not allow Raspberry Pi to shut down safely and can result in a corrupt micorSD card or file system. It is strongly recommended that you shut down safely using the above method (or at least try it first).

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

### Start a terminal session (without killing the X server and Chromium)

1. Press the **Ctrl**+**Alt**+**[Fn]**+**F2** keys to switch to terminal.
2. Log in with your username and password.
3. To switch back to kiosk display, make sure to logout first by typing `logout` into the terminal window and pressing the **Enter** key, and then press the **Ctrl**+**Alt**+**[Fn]**+**F1** keys.

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

1. Press the **Ctrlx**+**x** keys, type **Y** to save the changes, and press the **Enter** key to confirm.

1. Reboot your Raspberry Pi.

   ```bash
   sudo reboot
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
* Write-protecting the boot paritition.

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

### Web pages

On the web pages displayed, set the html lang (e.g. to 'en') to prevent Chromium from triggering page translation prompts.

<br /><br />

\---

Michael Donnay and Kunika Kono, [Digital Humanities Research Hub (DHRH)](https://www.sas.ac.uk/digital-humanities), School of Advanced Study (SAS), University of London.  

:octocat: Find us on GitHub at https://github.com/SAS-DHRH