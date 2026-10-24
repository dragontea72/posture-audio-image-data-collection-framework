# EEG-Based Prosthetic Arm Motor-Imagery Data Collection Paradigm

A Python/OpenCV stimulus-presentation paradigm for collecting EEG/EMG data during open-hand / close-hand motor (or motor-imagery) trials, built to drive a prosthetic-arm control research pipeline. The script cues a participant through a fixed visual and audio sequence per trial, logs stimulus onset times, and renders a Gantt-style timeline of each trial for alignment with the recorded physiological signal.

## Overview

Each session runs **5 trials**. The first trial follows an extended pre-paradigm/countdown sequence (plus-sign fixation → 3-2-1 countdown → start cue → open → close → post-paradigm rest), while trials 2–5 follow a shorter open → relax → close → relax cycle. For every trial, the script:

1. Signals the start of data acquisition (`collect_data()` — a placeholder hook to synchronize with external EEG/EMG recording hardware).
2. Displays each stimulus image fullscreen for its scheduled duration, playing a matching audio cue where applicable (`show_image()`).
3. Logs the real elapsed time at which each stimulus appeared.
4. Plots a timeline of the trial's stimulus sequence with the corresponding image thumbnailed onto each segment (`plot_gantt_with_images()`), useful for sanity-checking stimulus timing against the recorded signal offline.
5. Rests 5 seconds before the next trial.

At the end of the session, a closing "thank you" image and audio cue are shown.

## Repository structure

```
.
├── EEG_Prosthetic_Arm_Paradigm.ipynb   # Main notebook (recommended entry point)
├── paradigm.py                         # Equivalent standalone script
├── assets/                             # Stimulus images & audio (not tracked — see below)
│   ├── plus_sign.png                   # Fixation cross
│   ├── openhand.jpg                    # Open-hand cue
│   ├── closehand.jpg                   # Close-hand cue
│   ├── open.mp3                        # Audio cue for "open"
│   ├── close.mp3                       # Audio cue for "close"
│   ├── thanks.png                      # End-of-session image
│   └── thanks.mp3                      # End-of-session audio
└── README.md
```


## Requirements

- Python 3.8+
- A display and audio output device (the script opens fullscreen OpenCV windows and plays audio — it will not run in a headless/remote environment)

Install dependencies:

```bash
pip install opencv-python pygame matplotlib pandas
```

(if using the notebook) also:

```bash
pip install notebook
```

## Usage

1. Place your stimulus images and audio files in `assets/` (or update the paths in the code to match your own layout).
2. Run the notebook:

   ```bash
   jupyter notebook EEG_Prosthetic_Arm_Paradigm.ipynb
   ```

   or the equivalent script:

   ```bash
   python paradigm.py
   ```
3. Start your EEG/EMG acquisition software so it is recording when the session begins (the `collect_data()` function is a placeholder — wire it to your acquisition SDK, an LSL marker stream, or a hardware trigger as needed for your setup).
4. Follow the on-screen cues. Press through the fullscreen image windows via `cv2.waitKey` as configured.

## Notes & limitations

- **Hardware trigger**: `collect_data()` currently just prints a message and sleeps — it's a stand-in for whatever synchronizes this script with your actual EEG/EMG recording system.
- **Display-only timing log**: the Gantt plots are built from software-level timestamps (`time.time()`), intended as a sanity check of stimulus delivery, not a substitute for hardware-level event markers in the recorded signal.
- **Platform**: `cv2.WND_PROP_FULLSCREEN` fullscreen behavior can vary slightly across OSes; tested primarily on Windows.

## Author

**Riadul Islam Rabbi**

## License

Add a license of your choice (e.g. MIT) before publishing if you intend this to be reused by others.
