# WAXAL ASR Challenge (Zindi)

Team repo for the [Google WAXAL ASR Challenge](https://zindi.africa/competitions/google-waxal-asr-challenge).
Task: ASR (WER/CER, 50/50) on **Lingala, Shona, Luganda**. Max 5 submissions/day, 200 total.

## Workflow: Kaggle-first

We train on **Kaggle** (30 free GPU hrs/week, 73GB disk — needed for the audio) instead of local machines.
GitHub is for code only. No audio, no checkpoints, no massive CSVs in this repo — see `.gitignore`.

## Repo structure

```
configs/            # one yaml per experiment/language/model
data/
  raw/              # Train.csv, Test.csv, SampleSubmission.csv (small, tracked)
  processed/        # generated locally/on Kaggle, gitignored
notebooks/          # exploration + Kaggle starter notebook, nothing load-bearing
scripts/
  setup_data.py     # pulls + preps audio from Hugging Face
  make_submission.py
src/
  data/             # loading, resampling, augmentation
  models/           # model wrappers (Whisper, wav2vec2/XLS-R, MMS)
  train.py
  infer.py
  metrics.py        # local WER/CER (jiwer) — matches leaderboard scoring
submissions/        # generated CSVs, gitignored
experiments.md       # log: date, model, config, local score, leaderboard score
requirements.txt
```

## Roles (current sprint)

**[Name 1] — Kaggle Pilot & Baseline**

- Import `Waxal_Challenge_Starter_Code.ipynb` into Kaggle (Code → New Notebook → Import).
- Enable Internet + GPU (T4x2).
- Run top to bottom (needs a Hugging Face token for audio download).
- Submit output CSV to Zindi, log score in `experiments.md`.

**[Name 2] — Data & Pipeline Wrangler**

- Extract the Hugging Face download code from the starter notebook into `scripts/setup_data.py` (clean, reusable, not notebook-bound).
- Profile the dataset: hours of audio per language (Lingala/Shona/Luganda), class balance. Share findings in group chat.

**[Name 3] — Metrics & Research**

- Build `src/metrics.py` using `jiwer` for WER + CER (matches Zindi's 50/50 scoring) so we can score locally before burning submissions.
- Find 1–2 solid Kaggle notebooks on fine-tuning Whisper for ASR; bookmark as templates.

## Golden rules

1. Log **every** Zindi submission in `experiments.md`: date, model, config, local score, leaderboard score.
2. Never commit audio, checkpoints, or large CSVs — `data/` and `submissions/` are gitignored except `.keep` / small raw CSVs.
3. Score locally with `src/metrics.py` before submitting — don't waste the 5/day quota.
4. One branch per person, PR into `main`; `main` should always produce a working submission.
5. Help each other unblock Kaggle/Hugging Face token issues — don't sit stuck.

## Setup

```bash
pip install -r requirements.txt
python scripts/setup_data.py    # pulls dataset (Kaggle or local, once written)
python src/metrics.py           # sanity check scoring
```
