# RC522 RFID Module

**Specifications:**

* Chip: Mifare RC522 (MFRC522)
* Operating frequency: 13.56MHz
* Power supply voltage: 3.3V
* Communication interface: SPI

<br />

Raspberry Pi has 3 types of serial interfaces on its GPIO header, namely UART (Universal Asynchronous Receiver/Transmitter), SPI  (Serial Peripheral Interface) and I2C (Inter-Integrated Cricuit). These serial interfaces allow data exchange/communication between Raspberry Pi and other microcontrollers or peripherals (such as an RFID reader).

We will be setting up the RFID RC522 module with the SPI interface, as it is the only interface supported by the module.

<br />

The installation and setup process consists of the following parts:

1. Configuring your Raspberry Pi
2. Installing RC522 RFID module
3. Verifying installation

<br />

You will need:

* Raspberry Pi with Raspberry Pi OS already installed
* Display monitor
* Keyboard

<br />

## Installation

### 1. Configure Raspberry Pi

1. Enable the SPI interface on your Raspberry Pi.

    1. Run rasp-config.

        ```bash
        sudo raspi-config
        ```

    1. Using the Up/Down key, select **Interface Options** and Press the **Enter** key.

    1. Using the Up/Down key, select **SPI** and press the **Enter** key.

    1. Using the Left/Right key (or the Tab key), select **\<Yes>** to enable the SPI interface and press the **Enter** key.

    1. With **\<Ok>** selected, press the **Enter** key.

    1. Using the Tab key, select **\<Finish>** and press the Enter key.

1. Check and confirm SPI has been successfully enabled. 

    ```bash
    lsmod | grep spi
    ```

    The output of the command should include `spi_bcm2835` like this:

    ```bash
    spidev                 20480  0
    spi_bcm2835            20480  0
    ```

### 2. Install RC522 RFID module on Raspberry Pi

1. Shut down your Raspberry Pi, and disconnect from the power source.

    ```bash
    sudo shutdown -h now
    ```

1. Connect the RFID RC522 and your Raspberry Pi with jumper wires, according to the [Board Connections guide](https://github.com/MiczFlor/RPi-Jukebox-RFID/blob/future3/main/documentation/developers/rfid/mfrc522_spi.md) and the wiring/fritzing schematic shown in [Wiring_for_RC522_card_reader](https://github.com/MiczFlor/RPi-Jukebox-RFID/wiki/Wiring_for_RC522_card_reader).

    :warning: Do this part with your Raspberry Pi shut down and powered off, i.e. with the power supply not plugged in, just in case you make a mistake when you are wiring the RFID RC522.

1. Check and double-check the wiring. Shifting the wiring by one pin or down a full row of pins could cause damage to both the RFID RC522 and your Raspberry Pi.

1. Re-connect your Raspberry Pi to a power source and boot it up.

### 3. Verify installation

1. Make sure you are in your home directory.

    ```bash
    cd
    ```

1. Make sure your package list is up-to-date.

    ```bash
    sudo apt update
    ```

1. Install dependencies.

    - git

      Check if it is already installed:

      ```bash
      apt list --installed | grep git
      ```

      If it is already installed, the command will output like the following (usually after a few seconds' pause):

      ```bash
      git/stable,now 1:2.39.2-1.1 armhf [installed]
      ```

      Otherwise, install the package:

      ```bash
      sudo apt install git -y
      ```

    - spidev

      Check if it is already installed:

      ```bash
      apt list --installed | grep python3-spidev
      ```

      If it is already installed, the command will output like the following (usually after a few seconds' pause):

      ```bash
      python3-spidev/stable,now 20200602~200721-1+bookworm armhf [installed]
      ```

      Otherwise, install the package:

      ```bash
      sudo apt install python3-spidev -y
      ```

    - RPi.GPIO

      Check if it is already installed:
      
      ```bash
      apt list --installed | grep python3-rpi.gpio
      ```

      If it is already installed, the command will output like the following:
      
      ```bash
      python3-rpi.gpio/stable,now 0.7.1~a4-1+b2 armhf [installed]
      ```

      Otherwise, install the package:
      
      ```bash
      sudo apt install python3-rpi.gpio -y
      ```
      
    - setuptools

      Check if it is already installed:
      
      ```bash
      apt list --installed | grep python3-setuptools
      ```

      If it is already installed, the command will output like the following:
      
      ```bash
      python3-setuptools/stable,now6.1.1-1 all [installed]
      ```

      Otherwise, install the package:
      
      ```bash
      sudo apt install python3-setuptools -y
      ```
      
      

1. Create a new directory named `downloads` and change your working directory to it.

    ```bash
    mkdir downloads && cd downloads
    ```

1. Install [pi-rc522](https://github.com/ondryaso/pi-rc522), the Python library used by Phoniebox to interface with RFID RF522 module.

    Clone the Python library repository:

    ```bash
    git clone https://github.com/ondryaso/pi-rc522.git
    ```

    Move inside the cloned repository:

    ```bash
    cd pi-rc522
    ```

    Install the Python library:

    ```bash
    sudo python3 setup.py install
    ```

1. Run the example code.

    ```bash
    python3 examples/ReadUid.py
    ```

    The code will initially output nothing, but when an NFC/RFID fob or sticker or card is placed over the area marked `(((•)))`, its ID number should start appearing in your terminal window:

    ```bash
    UID: 835D84A6
    UID: 835D84A6
    UID: 835D84A6
    UID: 835D84A6
    UID: 835D84A6
    ...
    ```

1. Press the **Ctrl+C** keys to quit running the code.

<br /><br />

\---

Michael Donnay and Kunika Kono, [Digital Humanities Research Hub (DHRH)](https://www.sas.ac.uk/digital-humanities), School of Advanced Study (SAS), University of London.  

:octocat: Find us on GitHub at https://github.com/SAS-DHRH
