<div align="center">

# ⬡ TrafficAI
### Detection of Severity in Traffic Accidents

![Status](https://img.shields.io/badge/status-active-3fb850?style=flat-square)
![Language](https://img.shields.io/badge/language-HTML%20%7C%20CSS%20%7C%20JS-3b82f6?style=flat-square)
![Model](https://img.shields.io/badge/model-Orange%20Data%20Mining-f97316?style=flat-square)
![License](https://img.shields.io/badge/license-MIT-7d8590?style=flat-square)
![Year](https://img.shields.io/badge/base%20year-2020-7d8590?style=flat-square)

**An AI-powered platform for real-time classification of traffic accident severity.**

[Live Demo](#demo) · [How It Works](#how-it-works) · [Dataset](#dataset) · [Model](#model) · [Team](#team)

---

</div>

## Overview

**TrafficAI** is a full-stack prototype that applies machine learning to classify the severity of traffic accidents into three levels **Light, Moderate, and Critical** enabling faster and more accurate emergency response dispatch.

Built on a real 2020 dataset with **22,643 records**, the system was trained using **Orange Data Mining** with algorithms including **Random Forest** and **XGBoost**. The front-end interface allows users to submit accident images or text descriptions and receive a severity classification with confidence score and suggested emergency resources.

> Road safety is a critical public health challenge. Every second between an accident and the right emergency team arriving can mean the difference between life and death.

---

## Table of Contents

- [Overview](#overview)
- [Demo](#demo)
- [How It Works](#how-it-works)
- [Dataset](#dataset)
- [Model](#model)
- [Project Structure](#project-structure)
- [Getting Started](#getting-started)
- [Key Findings](#key-findings)
- [Severity Classification](#severity-classification)
- [Roadmap](#roadmap)
- [Team](#team)

---

## Demo

The front-end platform (`index.html`) includes a fully interactive demo section where you can:

- **Upload an accident image** via drag-and-drop or file picker
- **Describe an accident in text** and receive an AI-based classification
- See the detected **severity level**, **confidence percentage**, **suggested emergency resource**, and **analysis time**

> ⚠️ The current demo uses simulated inference logic in JavaScript. Integration with the real trained model (`.pkcls`) via a backend API is the next development step.

**To run locally:**

```bash
# Clone the repository
git clone https://github.com/devmariagabriela/trafficai.git
cd register-an-incident

# Open in browser (no build step required)
open index.html
# or just double-click index.html
```

---

## How It Works

The system follows a four-stage automated pipeline:

```
┌─────────────────┐    ┌──────────────────┐    ┌─────────────────┐    ┌──────────────────┐
│  01 · Capture   │───▶│ 02 · Preprocess  │───▶│  03 · Inference │───▶│  04 · Alert      │
│                 │    │                  │    │                 │    │                  │
│ Traffic cameras │    │ Normalization    │    │ Trained model   │    │ Severity level   │
│ Drones / photos │    │ Metadata extract │    │ Random Forest / │    │ Emergency dispatch│
│ Text input      │    │ Null treatment   │    │ XGBoost         │    │ Resource routing │
└─────────────────┘    └──────────────────┘    └─────────────────┘    └──────────────────┘
```

**Stage 1 - Data Capture:** Images from traffic cameras, drones, smartphones, or text descriptions submitted by users.

**Stage 2 - Preprocessing:** The pipeline normalizes inputs, extracts contextual metadata (time, location, weather), encodes categorical variables, and treats null values.

**Stage 3 - AI Inference:** The trained classifier analyzes damage patterns, vehicle positioning, debris distribution, and contextual factors to predict severity.

**Stage 4 - Classification & Alert:** Output is mapped to one of three severity levels. Alerts are dispatched to the appropriate emergency services.

---

## Dataset

| Property | Value |
|---|---|
| **Total records** | 22,643 |
| **Base year** | 2020 |
| **Target variable** | Severity (Light / Moderate / Critical) |
| **Source** | Brazilian traffic accident registry |

### Key variables

| Variable | Relevance |
|---|---|
| `sex` | Male involvement ~76% of all cases |
| `seatbelt` | Single strongest predictor of injury severity |
| `hour` | Peak accident time: 6 PM |
| `road_type` | Contextual predictor |
| `severity` | **Target** - Light / Moderate / Critical |

### Severity distribution

```
Moderate  ████████████████████████████████░░░░  ~54%
Light     █████████████████████░░░░░░░░░░░░░░░  ~45%
Critical  █░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░  ~0.5%
```

> The significant class imbalance critical cases represent only 0.5% of records is the main modeling challenge and requires special handling (see [Model](#model)).

---

## Model

### Training environment

The model was trained using **[Orange Data Mining](https://orangedatamining.com/)**, a visual workflow tool for machine learning. The workflow file is available at:

```
Deteccao_Severidade_treino.ows
```

The trained model is exported as:

```
modelo_acidente_treinado.pkcls
```

This file can be loaded directly into Orange for inference or evaluation.

### Pipeline

```
Raw CSV
   │
   ▼
┌──────────────────────────────┐
│  Preprocessing               │
│  • Null value imputation     │
│  • Categorical encoding      │
│  • Feature normalization     │
└──────────────┬───────────────┘
               │
               ▼
┌──────────────────────────────┐
│  Data Balancing              │
│  • SMOTE (oversampling)      │
│    or Undersampling          │
│  (handles 0.5% critical      │
│   class imbalance)           │
└──────────────┬───────────────┘
               │
               ▼
┌──────────────────────────────┐
│  Algorithm Training          │
│  • Random Forest             │
│  • XGBoost                   │
└──────────────┬───────────────┘
               │
               ▼
┌──────────────────────────────┐
│  Evaluation                  │
│  • Confusion Matrix          │
│  • F1-Score                  │
│  • Recall (priority:         │
│    critical class)           │
└──────────────────────────────┘
```

### Why Recall?

In this context, **a false negative on a critical accident is far more dangerous than a false positive**. Missing a life-threatening case because the model underestimated severity could cost lives. Therefore, **Recall for the critical class** is the primary optimization target ahead of overall accuracy.

---

## Project Structure

```
trafficai/
│
├── index.html                          # Main web interface
├── style.css                           # Styles (dark theme, animations)
├── script.js                           # Interactive logic, demo simulation
│
├── Deteccao_Severidade_treino.ows      # Orange Data Mining workflow
├── modelo_acidente_treinado.pkcls      # Trained classifier (Orange format)
│
└── DETECTION_OF_SEVERITY_IN_
    TRAFFIC_ACCIDENTS.pdf               # Project presentation (slides)
```

---

## Getting Started

### Running the front-end

No build tools or dependencies required.

```bash
git clone https://github.com/YOUR_USERNAME/trafficai.git
cd trafficai
open index.html
```

### Opening the model in Orange

1. Download and install [Orange Data Mining](https://orangedatamining.com/download/)
2. Open Orange
3. Go to **File → Open** and select `Deteccao_Severidade_treino.ows`
4. The trained model can be loaded via **File → Load Model** → `modelo_acidente_treinado.pkcls`

### Future: API integration

To connect the front-end demo to the real model, a Python backend would expose the `.pkcls` model via a REST endpoint:

```python
# Example (pseudocode)
import pickle
import orange3

with open("modelo_acidente_treinado.pkcls", "rb") as f:
    model = pickle.load(f)

def predict(features):
    return model(features)
```

The `runDemo()` function in `script.js` is already structured to receive and render a real API response.

---

## Key Findings

From the exploratory analysis of 22,643 records:

**Gender disparity**
- 76% of those involved in accidents were male (17,351 men vs. 4,727 women)
- Suggests higher driving exposure and risk-taking behavior among men

**Seatbelt use**
- 86.5% of those involved wore a seatbelt (19,583 people)
- The 13.5% who did not (3,060 people) showed disproportionately higher severity outcomes
- Seatbelt use is the single strongest individual predictor of injury severity

**Peak hours**
- Accident frequency peaks at **6 PM**, coinciding with end-of-day traffic congestion
- Secondary peak at 8 AM (morning rush)
- Lowest risk period: 2–4 AM

**Class imbalance**
- Critical cases account for only 0.5% of records
- Without balancing, models default to predicting Light/Moderate and miss all Critical cases
- SMOTE or undersampling is required before training

---

## Severity Classification

| Level | Label | Criteria | Emergency Resource |
|---|---|---|---|
| 🟢 **Level 1** | Light | Minor damage, no apparent victims, vehicles on road | Traffic patrol unit |
| 🟡 **Level 2** | Moderate | Structural damage, airbag deployed, conscious victims with pain | SAMU + Municipal Guard |
| 🔴 **Level 3** | Critical | Severe destruction, fire risk, trapped/unconscious victims, road blocked | SAMU + Fire Brigade + Police |

---

## Roadmap

- [x] Exploratory data analysis (2020 dataset)
- [x] Model training in Orange Data Mining
- [x] Front-end prototype (HTML/CSS/JS)
- [x] Interactive demo with severity simulation
- [ ] Python backend API exposing the `.pkcls` model
- [ ] Real model inference connected to front-end
- [ ] Integration with live traffic camera feeds
- [ ] Expand dataset to multiple years and cities
- [ ] Mobile application for field reporting

---

## Tech Stack

| Layer | Technology |
|---|---|
| Front-end | HTML5, CSS3, Vanilla JavaScript |
| Fonts | Bebas Neue, DM Sans, Space Mono (Google Fonts) |
| ML Training | Orange Data Mining 3 |
| Algorithms | Random Forest, XGBoost |
| Model format | `.pkcls` (Orange pickle) |
| Balancing | SMOTE / Undersampling |
| Evaluation | Confusion Matrix, F1-Score, Recall |

---

## Team

| Name | Role |
|---|---|
| **Adriely Natasha Martins Andrade** | Data Analysis & Presentation |
| **Hilton Alves Nery** | Statistical Analysis |
| **Miguel Luiz Lins de Oliveira** | AI model training & Explanation |
| **Maria Gabriela da Silva Pereira** | Data Analysis & Presentation |
| **Matheus Leonardo Araujo de Mesquita Silva** | Front-End Development & Code Presentation |

**Instructors:** Jonathan Bandeira da Silva and Oscar Agra Gonçalves

---

## License

This project was developed for academic purposes as part of the Software Analysis and Development 4th Semester. Dataset and results are for educational use only.

---

<div align="center">

**⬡ TrafficAI** - Built to save lives through faster emergency response.

*Artificial intelligence applied to road safety.*

</div>
