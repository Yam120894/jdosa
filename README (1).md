# Simple Next Slide Detector

A lightweight Streamlit web-app that listens to your voice and detects the spoken command **"Next Slide"**. It can be trained from scratch on your own voice recordings and then used live (or during a presentation) to increment a counter whenever the phrase is detected.

---

## Features

- Interactive Streamlit UI.
- One-click recording inside the browser (thanks to the _streamlit-audiorecorder_ component).
- Generates mel-spectrograms (128 × 128) from audio and trains a small CNN with TensorFlow/Keras.
- Simple data-augmentation pipeline (pitch-shift, time-stretch, noise, etc.).
- Persists the model to `simple_next_slide_model.h5` so you can reload it later.

---

## Project Structure

```
yam/
├── new_detector.py          # main Streamlit application
├── true/                    # place .wav files that contain the phrase "next slide" here
├── false/                   # place .wav files with any other speech / noise here
└── simple_next_slide_model.h5   # saved model will appear here after training
```

---

## Requirements

- **Python 3.9 or newer**
- macOS / Linux / Windows (tested on macOS with Apple Silicon – see the tensorflow note below)

### Python packages

The quickest way is to install everything from `requirements.txt`:

```bash
python -m venv venv          # optional but recommended
source venv/bin/activate     # Windows: venv\Scripts\activate

pip install -r requirements.txt
```

`requirements.txt` should contain roughly:

```
streamlit
streamlit-audiorecorder
numpy
librosa
matplotlib
opencv-python-headless  # or opencv-python
scikit-learn
scipy
tensorflow              # On Apple Silicon use  tensorflow-macos  +  tensorflow-metal
```

> **Apple Silicon:** If you are on an M1/M2 chip, install the Apple variant:
>
> ```bash
> pip install tensorflow-macos tensorflow-metal
> ```

---

## Usage

1. **Prepare training data**

   - Put a few short `.wav` recordings of you saying _"next slide"_ in the `true/` directory.
   - Put any other speech / background noise clips in the `false/` directory.
   - Tip: 10-15 recordings for each class are enough for a demo; the augmentation pipeline will expand them.

2. **Run the Streamlit app**

   ```bash
   streamlit run yam/new_detector.py
   ```

3. **Train**

   - In the browser tab that opens, click **🚀 Train New Model**.
   - Training progress, metrics, and validation curves are displayed live.

4. **Test**

   - After training finishes (≈ 1–2 minutes on CPU), use the built-in recorder to capture fresh audio and watch the prediction probability and counter update.

5. **Persist / Reload**
   - The trained model is saved automatically as `simple_next_slide_model.h5`.
   - On the next launch the app will pick it up and you can skip retraining.

---

## Troubleshooting

- **Audio too quiet** – speak closer to the microphone; the app warns if RMS energy is below a threshold.
- **Model not detecting** – collect more (and cleaner) examples, especially negatives.
- **`ModuleNotFoundError: audiorecorder`** – make sure you installed `streamlit-audiorecorder` (note the _streamlit-_ prefix on PyPI).
- **GPU not used with TensorFlow** – this is normal on CPUs; on Macs with Metal the `tensorflow-metal` plug-in provides acceleration.

---

## License

MIT © 2024 – Feel free to use, modify, and share.
