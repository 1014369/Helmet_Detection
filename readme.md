
# YOLOv8 Helmet Detection

Real-time helmet detection using YOLOv8 for industrial safety applications. This project provides an end-to-end pipeline from dataset collection and annotation to model training, inference, REST API deployment, and Docker containerization.

## Project Overview

- **Problem Statement:** Ensure safety compliance by detecting helmets in industrial environments.
- **Goal:** Train a YOLOv8 model for accurate real-time helmet detection.
- **Pipeline:**
  1. Dataset collection and annotation.
  2. Model training using Jupyter Notebook and modular scripts.
  3. Inference on images and videos.
  4. REST API integration using FastAPI.
  5. Docker deployment for portability.
  6. Live testing and evaluation.

## Project Structure

```text
helmet-detection/
│
├── data/
│   ├── images/
│   │   ├── train/
│   │   └── val/
│   └── labels/
│       ├── train/
│       └── val/
│
├── notebooks/
│   └── helmet_detection_yolov8.ipynb
│
├── model/
│   └── yolov8/
│
├── src/
│   ├── train.py
│   ├── detect.py
│   └── utils.py
│
├── app/
│   └── app.py
│
├── README.md
├── requirements.txt
├── Dockerfile
├── .gitignore
└── .env
```

## Installation

### 1. Clone the Repository

```bash
git clone <repository-url>
cd helmet-detection
```

### 2. Install Dependencies

```bash
pip install -r requirements.txt
```

Ensure Ultralytics YOLOv8 is installed for model training and inference.

### 3. Dataset Preparation

- Collect helmet images from Roboflow, Kaggle, CCTV footage, or custom sources.
- Annotate images using LabelImg or Roboflow in YOLO `.txt` format.
- Organize the dataset:

```text
data/
├── images/
│   ├── train/
│   └── val/
└── labels/
    ├── train/
    └── val/
```

- Create and configure `data.yaml` specifying dataset paths and class names.

### 4. Model Training

**Using Jupyter Notebook:**

- Open `notebooks/helmet_detection_yolov8.ipynb`.
- Configure `data.yaml` paths.
- Train the YOLOv8 model using the notebook cells.

**Using Modular Script:**

```bash
python src/train.py --data data/data.yaml --epochs 50 --img-size 640 --batch-size 16
```

### 5. Inference

```python
from ultralytics import YOLO

model = YOLO("model/yolov8/best.pt")

results = model.predict(
    source="data/images/val/sample1.jpg",
    conf=0.5,
    save=True,
    save_txt=True
)
```

- Outputs are saved in the `runs/detect/` directory.

### 6. REST API with FastAPI

Start the API:

```bash
uvicorn app.app:app --reload
```

**Endpoint:** `/predict`

Accepts image uploads and returns JSON containing:

- Labels
- Confidence scores
- Bounding boxes

**Test using cURL:**

```bash
curl -X POST -F "file=@sample.jpg" http://localhost:8000/predict
```

### 7. Docker Deployment

**Build the Docker image:**

```bash
docker build -t helmet-detector .
```

**Run the container:**

```bash
docker run -p 8000:8000 helmet-detector
```

Access the API at:

```text
http://localhost:8000/predict
```

### 8. Live Testing and Improvements

- Webcam feed detection.
- Batch inference on multiple images.
- Analyze accuracy, false positives, and edge cases.

## Future Enhancements

- Helmet-type classification.
- Sound alerts or visual dashboard.
- Multi-camera integration.

