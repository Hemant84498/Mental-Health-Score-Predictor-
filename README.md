# Mental Health Signal

**Student wellness, read like a forecast.**

A small ML-powered web app that estimates a student's mental health score (0–10) from their daily digital habits, study/sleep routine, and self-reported stress — trained on the "Student Social Media And Mental Health Impact" dataset.

![Mental Health Signal app screenshot](images/screenshot.png)

> ⚠️ This is a portfolio/learning project, not a diagnostic or clinical tool. The score is a model prediction, not medical advice.

---

## Overview

The app has two parts:

- **Backend** — a FastAPI service that loads a trained scikit-learn pipeline (`Mental_Health_Model.pkl`) and exposes a `/predict` endpoint.
- **Frontend** — a static HTML/CSS/JS form that collects a short profile (age, platform use, screen time, sleep, stress level, etc.), sends it to the backend, and animates the result on a gauge.

## Features

- Single-page form split into three sections — Profile, Academic & Digital Habits, Lifestyle & Stress
- Real-time validation on all numeric fields (age, hours, unlocks)
- Live gauge visualization with a loading, result, and error state
- FastAPI backend with Pydantic request/response validation and auto-generated docs at `/docs`
- CORS enabled for independent frontend/backend deployment

## Tech Stack

| Layer     | Tools |
|-----------|-------|
| Frontend  | HTML5, CSS3, vanilla JavaScript (fetch API, SVG gauge) |
| Backend   | FastAPI, Pydantic, Uvicorn |
| ML        | scikit-learn (`ColumnTransformer` + regression pipeline), pandas, joblib |

## Project Structure

```
mental-health-signal/
├── main.py                  # FastAPI backend + prediction endpoint
├── requirements.txt         # pinned backend dependencies
├── Mental_Health_Model.pkl  # trained scikit-learn pipeline
├── index.html               # frontend markup
├── style.css                # frontend styling
├── script.js                # form logic, API calls, gauge animation
├── assets/
│   └── screenshot.png
└── .gitignore
```

## Getting Started

### 1. Backend

```bash
pip install -r requirements.txt
uvicorn main:app --reload --port 2200
```

The API will be live at `http://127.0.0.1:2200`. Interactive docs are auto-generated at `http://127.0.0.1:2200/docs`.

### 2. Frontend

`script.js` calls the backend at a fixed `API_BASE` URL — make sure it matches wherever your backend is running (`http://127.0.0.1:2200` for local dev).

Serve the frontend with any static server (don't open `index.html` directly via `file://`, since `fetch` calls to a local backend can behave inconsistently that way):

```bash
python -m http.server 5500
```

Then open `http://127.0.0.1:5500`.

## API Reference

### `GET /`

Health check. Returns a welcome message.

### `POST /predict`

**Request body:**

```json
{
  "age": 21,
  "gender": "Male",
  "country": "India",
  "academic_level": "Undergraduate",
  "most_used_platform": "Instagram",
  "purpose_of_use": "Education",
  "avg_daily_usage_hours": 3.0,
  "daily_unlocks": 60,
  "study_hours": 4.0,
  "physical_activity_hours": 1.0,
  "sleep_hours_per_night": 7.0,
  "stress_level": "Medium"
}
```

| Field | Type | Constraints |
|---|---|---|
| `age` | int | 10–100 |
| `gender` | enum | `Male`, `Female` |
| `country` | string | free text |
| `academic_level` | enum | `Undergraduate`, `Graduate`, `High School` |
| `most_used_platform` | enum | `Facebook`, `LinkedIn`, `Instagram`, `Snapchat`, `Twitter`, `YouTube`, `TikTok`, `LINE`, `KakaoTalk`, `VKontakte`, `WhatsApp`, `WeChat` |
| `purpose_of_use` | enum | `Networking`, `Education`, `Entertainment`, `News` |
| `avg_daily_usage_hours` | float | 0–24 |
| `daily_unlocks` | int | ≥ 0 |
| `study_hours` | float | 0–24 |
| `physical_activity_hours` | float | 0–24 |
| `sleep_hours_per_night` | float | 0–24 |
| `stress_level` | enum | `Low`, `Medium`, `High`, `Very High` |

**Response:**

```json
{
  "predicted_mental_health_score": 6.43
}
```

## Deployment

- **Backend** → any Python host (e.g. Render): build with `pip install -r requirements.txt`, start with `uvicorn main:app --host 0.0.0.0 --port $PORT`.
- **Frontend** → any static host (e.g. Netlify, GitHub Pages). Update `API_BASE` in `script.js` to the deployed backend URL before deploying the frontend.

## Author

Built by Hemant, final-year Computer Engineering student.
