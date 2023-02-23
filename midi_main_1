from pydub import AudioSegment
import mido
from scipy import signal
import numpy as np

# Load the audio file
audio_file = AudioSegment.from_file("vela_1.wav")

# Set the tempo (in beats per minute) for the MIDI file
tempo = 500

# Set the window size for each syllable
window_size = 2

# Set the time (in seconds) between each note in the MIDI file
note_spacing = 0.1

# Convert the audio to mono and set the sample rate to 44.1 kHz
audio_data = audio_file.set_channels(1).set_frame_rate(44100)

# Get the raw audio data as a list of integers
samples = audio_data.get_array_of_samples()

# Filter the audio to remove higher frequencies
b, a = signal.butter(25, 5000, 'lowpass', fs=44100)
filtered_samples = signal.filtfilt(b, a, samples)

# Create a MIDI file with one track
midi_file = mido.MidiFile()
track = mido.MidiTrack()
midi_file.tracks.append(track)

# Set the time signature to 4/4
track.append(mido.MetaMessage('time_signature', numerator=4, denominator=4))

# Set the tempo
track.append(mido.Message('note_on', note=60, velocity=127, time=0))


# Convert each window of audio to a MIDI note and add it to the MIDI track
for i in range(0, len(filtered_samples), window_size):
    # Get the current window of audio as a list of integers
    window = filtered_samples[i:i + window_size]

    # Compute the root-mean-square (RMS) volume of the audio window
    rms_volume = np.sqrt(np.mean(np.square(window)))

    # Determine the MIDI note to play based on the volume of the audio window
    if rms_volume > 2000:
        note = max(0, min(int(round(12 * np.log2(rms_volume / 440) + 69)/1.5), 127))
        velocity = int(round(127 * rms_volume / 32768)*9)
        velocity = max(0, min(velocity, 127))
        print(f"Note: {note}, Velocity: {velocity}")

        # Add the note to the MIDI track
        if velocity >= 0 and velocity <= 127:
            print(f"Adding note_on message with note={note}, velocity={velocity}")
            track.append(mido.Message('note_on', note=note, velocity=velocity, time=0))
            print(f"Adding note_off message with note={note}, velocity={velocity}")
            track.append(mido.Message('note_off', note=note, velocity=velocity, time=int(note_spacing * 1000)))
            if velocity > 57:
                x=74
            if velocity <80:
                x=68 
            if velocity <100:
                x=61
            

            # Set the program (instrument) to use for the track
            track.append(mido.Message('program_change', program=x, time=0))

    # Print progress
    if i % (44100 * 2) == 0:
        print(f"{i / 44100} seconds processed")
        
# Save the MIDI file
midi_file.save('vela_1.mid')
