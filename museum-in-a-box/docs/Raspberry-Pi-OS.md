# Raspberry Pi OS

The installation and setup process consists of:

1. Preparing your MicroSD card.
2. Installing Raspberry Pi OS on your Raspberry Pi.
3. Updating Raspberry Pi OS on your Raspberry Pi. This is recommended even for newly installed OS.

<br />

You will need:

- Raspberry Pi
- MicroSD card
- Power supply for Raspberry Pi
- Display monitor
- USB Keyboard

For preparing a MicroSD card, you will also need:

- Linux, Mac or Windows computer
- MicroSD card reader/writer

<br />

## Installation

### 1. Prepare MicroSD card

1. Download and install [Raspberry Pi Imager](https://www.raspberrypi.com/software/) on your computer (not your Raspberry Pi).

1. Insert your MicroSD card into your computer, using a MicroSD card reader.

1. Start up Raspberry Pi Imager, and configure it as follows and click **NEXT**.

   **Raspberry Pi Device**: Select `NO FILTERING`.

   **Operating System**: Select `Raspberry Pi OS (other)` > `Raspberry Pi OS Lite (32-bit)` (A port of Debian Bookworm with no desktop environment).

   **Storage**: Select your MicroSD card.

1. In the **Use OS customisation?** prompt window, select **EDIT SETTINGS**.

1. In the **OS Customisation** window, configure the following settings, and when you are done, click **SAVE** to save your customisation settings and close the window.

   **GENERAL**

   - **Set hostname**: Enter a name for your Raspberry Pi. The name can be anything but should only contain alphabet, number and dash characters e.g. `museum-in-a-box`.

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

1. On returning to the **Use OS customisation?** prompt window, select **YES** to applying your OS customisation settings.

1. Select **YES** to erasing all existing data on your MicroSD card. If Raspberry Pi Imager asks you for a password, enter the password you use to log into your computer (not the password you set for your Raspberry Pi).

1. Once Raspberry Pi Imager has finished flashing Raspberry Pi OS to your MicroSD card, eject the card from your computer. Raspberry Pi Imager is by default set to eject media automatically - if you don't see your MicroSD card mounted on your Desktop or in Finder/File Explorer, you can simply remove it from the MicroSD card reader.

### 2. Install Raspberry Pi OS on Raspberry Pi

The examples in the following steps will use Raspberry Pi OS configured with:

- **Hostname**: museum-in-a-box
- **Username**: pi
- **Password**: letmein!23

Remember to use the username and password you set for your Raspberry Pi instead!

1. Insert your MicroSD card into your Raspberry Pi.

1. Connect your Raspberry Pi to a display monitor, keyboard and mouse.

1. Connect your Raspberry Pi to a power source and boot it up.

1. Once your Raspberry Pi has completed booting up (this may take 3-5 minutes the first time around), you should see a login prompt like the following:

   ```bash
   Raspibian GNU/Linux 12 museum-in-a-box tty1
   
   museum-in-a-box login:
   ```

1. Log into your Raspberry Pi.

   Type your username and press the Enter key, e.g.

   ```bash
   museum-in-a-box login: pi
   ```

   You will then be asked for a password. Type your password and press the Enter key, e.g.

   ```bash
   Password: letmein!23
   ```

1. You should now see a command prompt like this:

   ```bash
   pi@museum-in-a-box:~$
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

<br /><br />

\---

Michael Donnay and Kunika Kono, [Digital Humanities Research Hub (DHRH)](https://www.sas.ac.uk/digital-humanities), School of Advanced Study (SAS), University of London.  

:octocat: Find us on GitHub at https://github.com/SAS-DHRH
