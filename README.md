# AI-Powered-Cloud-Anomaly-Detection
A hybrid system for detecting anomalies in cloud security logs. It combines rule-based detectors with machine learning models (Isolation Forest &amp; Random Forest) to identify both known and unknown threats. Includes a unified preprocessing pipeline, model training, prediction scripts, and automated reports with dashboards and summaries.
# 1) Create & activate virtualenv (Windows PowerShell)
python -m venv .venv
.venv\Scripts\Activate.ps1

# macOS/Linux
python3 -m venv .venv
source .venv/bin/activate

# 2) Install deps
pip install -r requirements.txt

# 3) Preprocess (creates events_labeled.parquet in temp)
python -m src.unified_preprocess --input datasets/raw/clue.json \
  --out-events datasets/processed/events_labeled.parquet \
  --out-hourly datasets/processed/hourly_uploads.parquet \
  --max-lines 1000000 --size-mb 100 --min-conds 2 \
  --exts ".exe,.dll,.js,.jar,.apk,.zip,.7z,.bat,.sh"

# 4) Train (standard or fast)
python -m src.train_ai
# or
python -m src.train_ai_fast

# 5) Predict on new logs (writes predictions.csv)
python -m src.predict --input datasets/raw/clue.json --out reports/predictions.csv

# 6) Detect against an already-processed parquet
python -m src.detect
