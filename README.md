# 🏁 F1 Podium Predictor — Streamlit App

An interactive Streamlit application that deploys a pre-trained **Gradient
Boosting Classifier** to predict the probability that a Formula 1 driver
finishes on the **podium (Top 3)** in a given race.

## Contents

| File | Description |
|---|---|
| `app.py` | The Streamlit application |
| `f1_podium_gbm_model.pkl` | Pre-trained scikit-learn `GradientBoostingClassifier` |
| `requirements.txt` | Python dependencies |
| `sample_race_grid.csv` | Example CSV (10 drivers, one race) to try the app immediately |

## Model details

- **Type:** `sklearn.ensemble.GradientBoostingClassifier` (100 estimators, max depth 5)
- **Target:** binary — podium finish (1) vs. no podium (0)
- **Features (in order):**
  1. `grid` — starting grid position
  2. `position_driverStanding` — driver's championship standing before the race
  3. `wins` — driver's season win count so far
  4. `position_constructorStanding` — constructor's championship standing before the race
  5. `wins_constructorStanding` — constructor's season win count so far
  6. `round` — round number of the race in the season

## Setup

```bash
# 1. Create a virtual environment (recommended)
python3 -m venv venv
source venv/bin/activate      # Windows: venv\Scripts\activate

# 2. Install dependencies
pip install -r requirements.txt

# 3. Run the app
streamlit run app.py
```

The app will open automatically in your browser at `http://localhost:8501`.

> **Note:** the model was trained with an older scikit-learn version. You may
> see an `InconsistentVersionWarning` on load — this is safe to ignore, but
> for best results install scikit-learn 1.6.x (`pip install scikit-learn==1.6.1`)
> to match the training environment exactly.

## Using the app

### Option 1 — Upload a CSV
1. Select **Upload CSV** in the sidebar.
2. Download the provided sample template (or use `sample_race_grid.csv`) to see the expected format.
3. Upload a file containing at minimum the six required feature columns. Optional columns such as `driver`, `constructor`, or `race` are kept for display only.
4. The app predicts a podium probability for every row instantly.

### Option 2 — Manual entry
1. Select **Manual entry** in the sidebar.
2. Use **➕ Add driver** / **➖ Remove last driver** to size your grid.
3. Fill in each driver's grid position, standings, and win counts.
4. Click **🔮 Predict**.

### Reading the results
- **Threshold slider** (sidebar): sets the probability cutoff for an individual "predicted podium" classification.
- **Top 3 ranking** (sidebar checkbox): ranks every driver in the file by predicted probability (grouped by `round` if present) and flags the top 3 as the predicted podium finishers for that race — this is usually the more useful view when you upload a full race grid.
- **Summary metrics**: number of drivers evaluated, count predicted podium, average and max probability.
- **🏆 Predicted Top 3 Finishers**: medal callout for the three highest-probability drivers.
- **Table**: full sortable results with a one-click CSV download.
- **Charts**:
  - Bar chart of podium probability per driver (with the threshold line)
  - Histogram of the probability distribution
  - Scatter plot of grid position vs. podium probability (bubble size = season wins)

## Taking screenshots for your presentation

1. Run the app locally (`streamlit run app.py`).
2. Load `sample_race_grid.csv` via **Upload CSV** to populate a full example race.
3. Suggested screenshots to capture:
   - Sidebar with input mode and threshold settings
   - Summary metric cards + Predicted Top 3 podium callout
   - Full prediction table
   - Bar chart and histogram
   - (Optional) Manual entry form for a single driver
4. Use your OS screenshot tool (`Cmd+Shift+4` on macOS, `Win+Shift+S` on Windows, or your browser dev tools' "Capture full size screenshot") and paste the images into your slides.

## Deployment

To share the app publicly without local setup, you can deploy it for free on
[Streamlit Community Cloud](https://streamlit.io/cloud):
1. Push this folder to a public GitHub repo (include the `.pkl` file).
2. Go to share.streamlit.io, connect the repo, and point it at `app.py`.
3. Streamlit installs `requirements.txt` automatically and gives you a shareable URL.
