# 🍷 Neural Wine — AI-Powered Wine Classifier

A machine learning web application that classifies wine into three categories based on 13 chemical properties. Built with TensorFlow/Keras, Flask, and Docker.

## Features

- **Wine classification** — Predicts wine class (0, 1, or 2) from chemical composition
- **Web interface** — Modern UI with form and JSON input modes
- **REST API** — `POST /predict` endpoint for programmatic access
- **Docker support** — Single-image build or multi-service Compose setup

## Tech Stack

| Component | Technology |
|-----------|------------|
| ML Framework | TensorFlow 2.15, Keras |
| Dataset | scikit-learn Wine dataset |
| Web Framework | Flask |
| Containerization | Docker, Docker Compose |

## Project Structure

```
Docker Lab/
├── dockerfile           # Multi-stage build (train + serve)
├── docker-compose.yml   # Two-service setup with shared volume
├── requirements.txt
├── src/
│   ├── main.py          # Flask app & prediction API
│   ├── model_training.py # Model training script
│   ├── templates/
│   │   └── predict.html # Web UI
│   └── statics/         # Static assets (optional)
└── README.md
```

## Quick Start

### Option 1: Docker (single image)

Build and run in one container:

```bash
docker build -t neural-wine .
docker run -p 90:4060 neural-wine
```

### Option 2: Docker Compose (recommended for development)

Trains the model in one container, then serves it in another using a shared volume:

```bash
docker-compose up --build
```

- **Training** runs first and saves the model to a shared volume
- **Serving** starts after training completes and exposes the app on port **90**

## Usage

| Action | URL / Method |
|--------|--------------|
| Home | `http://localhost:90/` |
| Web form | `http://localhost:90/predict` (GET) |
| API prediction | `POST http://localhost:90/predict` |

### API Example

```bash
curl -X POST http://localhost:90/predict \
  -H "Content-Type: application/json" \
  -d '{
    "alcohol": 14.23,
    "malic_acid": 1.71,
    "ash": 2.43,
    "alcalinity_of_ash": 15.6,
    "magnesium": 127,
    "total_phenols": 2.8,
    "flavanoids": 3.06,
    "nonflavanoid_phenols": 0.28,
    "proanthocyanins": 2.29,
    "color_intensity": 5.64,
    "hue": 1.04,
    "od280_od315": 3.92,
    "proline": 1065
  }'
```

**Response:**

```json
{
  "predicted_class": "Class 2",
  "confidence": "98.45%"
}
```

## Input Features

The model expects these 13 features (from the Wine dataset):

| Feature | Description |
|---------|-------------|
| Alcohol | Alcohol content (%) |
| Malic Acid | Malic acid content |
| Ash | Ash content |
| Alcalinity of Ash | Alkalinity of ash |
| Magnesium | Magnesium content |
| Total Phenols | Total phenols |
| Flavanoids | Flavanoid content |
| Nonflavanoid Phenols | Non-flavanoid phenols |
| Proanthocyanins | Proanthocyan content |
| Color Intensity | Color intensity |
| Hue | Hue |
| OD280/OD315 | OD280/OD315 of diluted wines |
| Proline | Proline content |

## Model

- **Architecture:** Dense neural network (128 → 64 → 32 → 16 → 8 → 3)
- **Activations:** ELU (hidden), Softmax (output)
- **Regularization:** Dropout (0.3, 0.3, 0.2)
- **Training:** 120 epochs with Adam optimizer

## Requirements

- Docker 20.10+
- Docker Compose v2+ (for `docker-compose`)

## License

MIT
