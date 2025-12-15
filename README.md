# Traffic Sign Recognition

A web application for classifying traffic signs using deep learning models trained on the German Traffic Sign Recognition Benchmark (GTSRB) dataset.

## Overview

This project provides a Flask-based web interface for real-time traffic sign classification. Users can upload images of traffic signs and get predictions from two different neural network models:

- **Simple CNN**: A lightweight convolutional neural network trained from scratch (32×32 input)
- **MobileNetV2**: A transfer learning model using pre-trained MobileNetV2 (96×96 input, higher accuracy)

The system recognizes 43 different traffic sign classes including speed limits, warnings, and regulatory signs.

## Features

- Drag-and-drop image upload
- Model selection (Simple CNN vs MobileNetV2)
- Top-3 predictions with confidence scores
- Modern, responsive UI
- RESTful API endpoints

## Project Structure

```
├── app.py                       # Flask backend server
├── TrafficSignRecognition.ipynb # Model training notebook
├── simple_cnn_final.h5          # Trained Simple CNN model
├── mobilenetv2_final.h5         # Trained MobileNetV2 model
├── templates/
│   └── index.html               # Web interface
├── GTSRB/                       # Dataset folder
│   ├── train.p                  # Training data (pickle)
│   ├── valid.p                  # Validation data (pickle)
│   ├── test.p                   # Test data (pickle)
│   └── signname.csv             # Class ID to sign name mapping
├── requirements.txt             # Python dependencies
└── README.md
```

## Requirements

- Python 3.8+
- TensorFlow 2.10+
- Flask 2.0+
- NumPy
- Pillow

## Installation

1. Clone the repository:
```bash
git clone <repository-url>
cd traffic-sign-recognition
```

2. Install dependencies:
```bash
pip install -r requirements.txt
```

3. Ensure model files are present:
   - `simple_cnn_final.h5`
   - `mobilenetv2_final.h5`

## Dataset

The GTSRB dataset is included in the `GTSRB/` folder as pickle files:

| File | Description |
|------|-------------|
| `train.p` | Training images and labels (34,799 samples) |
| `valid.p` | Validation images and labels (4,410 samples) |
| `test.p` | Test images and labels (12,630 samples) |
| `signname.csv` | Mapping of class IDs to sign names |

Each pickle file contains a dictionary with:
- `features`: Image arrays (32×32×3)
- `labels`: Class labels (0-42)

## Usage

1. Start the server:
```bash
python app.py
```

2. Open your browser and navigate to:
```
http://localhost:5000
```

3. Upload a traffic sign image (JPG, JPEG, or PNG)

4. Select a model and click "Recognize Sign"

5. View the prediction results with confidence scores

## API Endpoints

| Endpoint | Method | Description |
|----------|--------|-------------|
| `/` | GET | Web interface |
| `/predict` | POST | Submit image for classification |
| `/models` | GET | List available models |

### Prediction Request

```bash
curl -X POST -F "image=@traffic_sign.jpg" -F "model=simple_cnn" http://localhost:5000/predict
```

### Response Format

```json
{
  "success": true,
  "model_used": "Simple CNN",
  "predictions": [
    {"class_id": 14, "class_name": "Stop", "confidence": 98.5},
    {"class_id": 17, "class_name": "No entry", "confidence": 0.8},
    {"class_id": 13, "class_name": "Yield", "confidence": 0.3}
  ]
}
```

## Supported Traffic Sign Classes

The model recognizes 43 classes from the GTSRB dataset:

| ID | Sign Type |
|----|-----------|
| 0-8 | Speed limits (20-120 km/h) |
| 9-10 | No passing signs |
| 11-12 | Right-of-way, Priority road |
| 13-14 | Yield, Stop |
| 15-17 | No vehicles, Weight limit, No entry |
| 18-31 | Warning signs (curves, bumps, etc.) |
| 32-42 | Mandatory signs (directions, roundabout) |

## Model Training

The `TrafficSignRecognition.ipynb` notebook contains the complete training pipeline:

1. Data loading from GTSRB pickle files
2. Preprocessing and normalization
3. Data augmentation
4. Simple CNN architecture definition
5. MobileNetV2 transfer learning setup
6. Training with callbacks (EarlyStopping, ReduceLROnPlateau)
7. Model evaluation and visualization

## License

MIT License
