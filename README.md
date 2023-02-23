# Wave-files-to-Midi-composition
This script loads an audio file, filters out high frequencies, and converts each window of audio to a MIDI note based on the volume of the window. The MIDI notes are added to a MIDI track, and the resulting MIDI file is saved.

Here are the main steps of the script:

    Load the audio file using the PyDub library.
    Set the tempo and window size for the MIDI file, and set the sample rate for the audio data.
    Convert the audio data to mono and filter out high frequencies using a low-pass filter.
    Create a MIDI file and track, and set the time signature and tempo.
    Convert each window of audio to a MIDI note based on the volume of the window.
    Add the MIDI note to the MIDI track, and set the instrument to use for the track.
    Save the resulting MIDI file.

The script also includes some code to print progress and to adjust the velocity of the MIDI notes based on their volume.
