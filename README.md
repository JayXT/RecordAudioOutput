# RecordAudioOutput
A custom bash script to record audio output in GNU/Linux.

The script is aimed at GNU/Linux distributions using **PulseAudio** as an audio system with **parec** (pulse audio recorder) command-line utility installed.
Before using it, it's necessary to identify your output source using the following command:

`pacmd list-sinks | grep -e 'name:' -e 'index' -e 'Speakers'`

Here's a sample of its output:

![image](https://user-images.githubusercontent.com/8045344/202847775-7b07fb32-623c-45ef-ba61-5ff13fa3896d.png)

After the source in use is identified, replace the value of deviceMonitor variable in the script with your device name + ".monitor" suffix.
In this particular example it looks as follows:

`deviceMonitor="alsa_output.usb-Audioengine_Ltd._Audioengine_2__ABCDEFB1180003-00.analog-stereo.monitor"`

The script with modified deviceMonitor variable should be placed in any convenient location.
Then user can assign a shortcut to execute it in the system settings:

![image](https://user-images.githubusercontent.com/8045344/202848531-43ae65c7-8a83-4bb1-935e-cdce79231c11.png)

The script has been tested in Debian 11 Xfce. 

Execution algorithm:
1. Check whether the parec process is already running.
2. If it's running, kill it and thus finish execution/recording, if it's not, proceed with the step 3.
3. Set deviceMonitor variable to the appropriate value.
4. Generate a random audio name starting with "out_" to be stored in user's Downloads folder.
5. Record the audio output from deviceMonitor to the output file using parec.
6. Copy the audio file to the clipboard (happens when the parec process is killed and then step 5 terminates).

So to sum it up, record_audio_output allows to start recording output audio, and finish it with one shortcut (e.g. `Super + A` to start and `Super + A` to finish).
Then the file will be ready in the clipboard to paste into any application like Anki, Telegram, etc.

This is especially useful for immersion-based language learning and Anki cards creation, because with a couple key presses its possible to record original high-quality audio from any movie, TV-series or dictionary.
