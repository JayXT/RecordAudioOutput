# RecordAudioOutput
A custom bash script to record audio output in GNU/Linux.

The solution is aimed at GNU/Linux distributions using either **PulseAudio** or **PipeWire** audio system. In the first case [pulseaudio-utils](https://packages.debian.org/sid/pulseaudio-utils) or alternative package  for your distribution containing **parec** utility should be installed. The script is available in two variations: **record_audio_output** (PulseAudio version) and **record_audio_output_pw** (PipeWire version).

Before using any of them, please ensure that **lame** and **xclip** (X11) or **wl-clipboard** (Wayland) dependencies are also installed. For a Wayland + PipeWire setup, the xclip line should be commented out, while the wl-copy line should be uncommented. Both script variations use the default sink (output audio device).

The respective script should be placed in any convenient location and a permission to execute it should be granted.
Then user can assign a shortcut to run it via the system settings:

![image](https://user-images.githubusercontent.com/8045344/202848531-43ae65c7-8a83-4bb1-935e-cdce79231c11.png)

The record_audio_output script has been tested in Debian 11 Xfce and Debian 12 Xfce with PulseAudio. The record_audio_output_pw has been tested in Fedora 39 GNOME with PipeWire.

The default execution algorithm:
1. Check whether parec/pw-record is already running.
2. If it's running, kill it and thus finish execution/recording, if it's not, proceed with the step 3.
3. Set a targetSink variable to the default output device.
4. Generate a random audio name starting with "out_" to be stored in user's Downloads folder.
5. Record the audio output from the targetSink to the output file using parec/pw-record + lame.
6. Copy the audio file to the clipboard with xclip/wl-copy (happens when the audio process is killed and then step 5 terminates).

So to sum it up, both script versions allow to start recording output audio, and finish it with one shortcut (e.g. `Super + A` to start and `Super + A` to finish).
Then the file will stay in the clipboard ready to be pasted into any application like Anki, Telegram, etc.

This is especially useful for immersion-based language learning and Anki cards creation, because with a couple of key presses it's possible to record the original high-quality audio from any movie, TV series, dictionary, or service.

## Additional Notes 

If for some reason there's a need to manually specify a sink to record the audio from, for the PulseAudio variant of the script it could be done as follows:


1. Identify your output device using the following command: 
`pacmd list-sinks | grep -e 'name:' -e 'index' -e 'Speakers'`
Here's a sample of its output:
![image](https://user-images.githubusercontent.com/8045344/202847775-7b07fb32-623c-45ef-ba61-5ff13fa3896d.png)
2. Replace the value of targetSink variable in the script with your device name + ".monitor" suffix for PulseAudio.
In this particular example it looks as follows: 
`targetSink="alsa_output.usb-Audioengine_Ltd._Audioengine_2__ABCDEFB1180003-00.analog-stereo.monitor"`
