# Audio



## Setting up a Bluetooth speaker as the default system sound output

Phoniebox uses a sound server system called `PulseAudio` to pass sound data from Raspberry Pi to Bluetooth speaker, and `pactl` (PulseAudio Control) to configure and control the sound server.

1. Check available sound outputs.

   ```bash
   pactl list sinks short
   ```

   The command will output a list of audio devices like this:

   ```bash
   0	alsa_output.platform-3f902000.hdmi.hdmi-stereo	module-alsa-card.c	s16le 2ch 48000Hz	SUSPENDED
   3	bluez_sink.F4_4E_FD_8C_AA_E2.a2dp_sink	module-bluez5-device.c	s16le 2ch 44100Hz	SUSPENDED
   ```

2. Set the default sink.

   ```bash
   pactl set-default-sink bluez_sink.F4_4E_FD_8C_AA_E2.a2dp_sink
   ```

3. Check the default sink.

   ```bash
   pactl info
   ```

   The output of the command will look like this:

   ```bash
   Server String: /run/user/1000/pulse/native
   Library Protocol Version: 35
   Server Protocol Version: 35
   Is Local: yes
   Client Index: 12
   Tile Size: 65496
   User Name: pi
   Host Name: museum-in-a-box
   Server Name: pulseaudio
   Server Version: 16.1
   Default Sample Specification: s16le 2ch 44100Hz
   Default Channel Map: front-left,front-right
   Default Sink: bluez_sink.F4_4E_FD_8C_AA_E2.a2dp_sink
   Default Source: bluez_sink.F4_4E_FD_8C_AA_E2.a2dp_sink.monitor
   Cookie: c55d:4e15
   ```


<br />

## Testing system sound output

1. Check volume level.

   ```bash
   alsamixer
   ```

   Press the **ESC** key to exit.

2. Check Pulseaudio sound output.

   ```bash
   paplay /usr/share/sounds/alsa/Front_Center.wav
   ```

3. If you have your speaker connected via the sound jack port, also check ALSA sound output.

   ```bash
   aplay /usr/share/sounds/alsa/Front_Center.wav
   ```

<br /><br />

\---

Michael Donnay and Kunika Kono, [Digital Humanities Research Hub (DHRH)](https://www.sas.ac.uk/digital-humanities), School of Advanced Study (SAS), University of London.  

:octocat: Find us on GitHub at https://github.com/SAS-DHRH
