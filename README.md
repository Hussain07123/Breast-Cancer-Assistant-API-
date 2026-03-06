# AI/ML Portfolio Project: Breast Cancer Risk Classifier

This project is a complete machine learning pipeline you can showcase on GitHub:
- Data loading from a real dataset (`sklearn` breast cancer dataset)
- Model selection (`LogisticRegression` vs `RandomForest` with cross-validation)
- Evaluation + saved metrics
- Reusable inference script
- Production-style FastAPI prediction endpoint
- Basic unit test

## Tech Stack

- Python
- scikit-learn
- pandas / numpy
- FastAPI + Uvicorn
- pytest

## Project Structure

```text
ml_profile_ai_project/
  src/
    train.py
    infer.py
    api.py
    model_utils.py
  models/           # generated after training
  reports/          # generated after training
  examples/         # generated sample input
  tests/
  requirements.txt
```

## Quick Start

```bash
cd ml_profile_ai_project
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
```

### 1) Train model

```bash
python -m src.train
```

This generates:
- `models/model.joblib`
- `models/metadata.json`
- `reports/metrics.json`
- `examples/sample_input.json`

### 2) Run inference (CLI)

```bash
python -m src.infer --input examples/sample_input.json
```

### 3) Run API

```bash
uvicorn src.api:app --reload
```

Open:
- `http://127.0.0.1:8000/docs` (interactive Swagger UI)

### API Flow (Simple)

In Swagger (`/docs`) use only these 3 endpoints:
- `GET /info` -> explains what the API does and whether model is ready
- `POST /check` -> fill simple details and get likely result
- `GET /prevention` -> prevention tips

For `POST /check`, you only fill:
- `age`
- `lump_size_mm`
- `texture_level` (`low` / `medium` / `high`)
- `border_irregular` (`true` / `false`)
- `family_history` (`true` / `false`)
- `skin_or_nipple_changes` (`true` / `false`)

Example body:

```json
{
  "age": 42,
  "lump_size_mm": 18,
  "texture_level": "medium",
  "border_irregular": false,
  "family_history": false,
  "skin_or_nipple_changes": false
}
```

The API converts this simple form into the full 30-feature model input automatically.

## Example API Request

```bash
curl -X POST "http://127.0.0.1:8000/predict" \
  -H "Content-Type: application/json" \
  -d @examples/api_request.json
```

You can generate `examples/api_request.json` after training with:

```bash
python - <<'PY'
import json
from pathlib import Path

sample = json.loads(Path("examples/sample_input.json").read_text())["sample"]
Path("examples/api_request.json").write_text(json.dumps({"features": sample}, indent=2))
print("Wrote examples/api_request.json")
PY
```

## Run Tests

```bash
pytest -q
```

## Portfolio Notes (Add this to your resume)

- Built and validated an end-to-end ML classification pipeline with model selection and ROC-AUC optimization.
- Packaged inference into both CLI and REST API interfaces.
- Added reproducible artifacts (model, metadata, metrics) for deployment and auditability.

## Important

This model is a technical demo for learning and portfolio purposes only, not a medical diagnostic tool.
