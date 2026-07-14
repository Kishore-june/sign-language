# Sign Language Detection & Translation

Real-time hand gesture recognition system that detects and classifies American Sign Language (ASL) alphabet signs from a live webcam feed. Built with OpenCV, cvzone, and a Keras/TensorFlow image classifier using MobileNet-based transfer learning, the model achieves ~90% classification accuracy on a custom-collected sign language dataset.

## Features

- 🖐️ Real-time hand detection and tracking via webcam
- ✋ Classifies hand signs into the 26 letters of the alphabet (A–Z)
- 📸 Built-in data collection script for building your own training dataset
- 🧠 Transfer-learning model (MobileNet) trained with Keras/TensorFlow
- 🎯 ~90% classification accuracy on the custom dataset

## Project Structure

```
sign-language/
├── MODEL/                 # Trained Keras model and class labels
│   ├── keras_model.h5
│   └── labels.txt
├── datacollection.py       # Captures and saves cropped hand images for training data
├── TEST.py                 # Runs real-time hand sign detection and classification
└── README.md
```

> **Note:** `datacollection.py` and `TEST.py` expect a `Data/` folder (for saving collected images) and a `Model/` folder (containing `keras_model.h5` and `labels.txt`) in the project root. Create/rename these folders as needed to match the paths referenced in the scripts.

## How It Works

1. **Data Collection** (`datacollection.py`) — Opens the webcam, detects a hand using `cvzone`'s `HandTrackingModule`, crops the hand region, resizes/pads it onto a fixed 300×300 white canvas (to normalize aspect ratio), and saves the processed image to disk when the `s` key is pressed.
2. **Model Training** — The collected images (organized into per-letter folders) are used to train an image classification model via transfer learning on MobileNet (trained externally, e.g. with [Teachable Machine](https://teachablemachine.withgoogle.com/) or Keras, and exported as `keras_model.h5` + `labels.txt`).
3. **Real-Time Testing** (`TEST.py`) — Captures live video, detects the hand, applies the same crop/resize/pad preprocessing, feeds the image to the trained classifier, and overlays the predicted letter on the video feed.

## Requirements

- Python 3.x
- [OpenCV](https://pypi.org/project/opencv-python/) (`opencv-python`)
- [cvzone](https://pypi.org/project/cvzone/)
- [NumPy](https://pypi.org/project/numpy/)
- TensorFlow / Keras (for running the classification model)

Install dependencies:

```bash
pip install opencv-python cvzone numpy tensorflow
```

## Usage

### 1. Collect Training Data (optional — to build your own dataset)

Update the `folder` variable in `datacollection.py` to point to the letter/class you want to collect images for, then run:

```bash
python datacollection.py
```

- Position your hand in front of the webcam.
- Press **`s`** to save the current cropped/processed frame as an image.
- Press **`q`** or close the window to stop.

Repeat for each letter (A–Z), saving images into separate folders (e.g. `Data/A`, `Data/B`, ...).

### 2. Train the Model

Train an image classifier (e.g. using MobileNet transfer learning) on the collected dataset. Export the trained model as `keras_model.h5` and its corresponding `labels.txt`, and place them inside the `Model/` folder.

### 3. Run Real-Time Detection

```bash
python TEST.py
```

This opens your webcam, detects your hand sign, and displays the predicted letter live on screen.

## Model

The `MODEL/` directory contains the pre-trained classifier used for inference:
- `keras_model.h5` — trained Keras model (MobileNet-based transfer learning)
- `labels.txt` — class labels corresponding to the model's output indices

## Tech Stack

- **Python**
- **OpenCV** — video capture and image processing
- **cvzone** — hand detection/tracking wrapper around MediaPipe
- **TensorFlow / Keras** — model training and inference
- **MobileNet** — transfer learning backbone

## License

No license Needed.

## Acknowledgements

Built using the `cvzone` hand-tracking and classification modules on top of OpenCV and TensorFlow/Keras.
