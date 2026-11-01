# Human Activity Recognition with a Hidden Markov Model (Formative 2)

Four activities (standing, still/standstill, walking, jumping) recorded with Sensor Logger and decoded by a Gaussian HMM
implemented from scratch in NumPy (forward-backward, Baum-Welch with log-likelihood convergence check, Viterbi).

## Repository layout
```
data/raw/<activity>/<activity>_NN/   original Sensor Logger exports (Accelerometer, Gyroscope, TotalAcceleration, Metadata)
data/processed/<activity>_NN.csv     200 cleaned files: 100 Hz grid, trimmed, labelled, timestamp_ns + time_s columns
data/test/test_session_{1,2}.csv     the two unseen test files (held-out recordings, stitched)
data/inventory.csv                   per-recording duration / measured sampling rate / device
data/features_all_windows.csv        11 features (+ tilt) for every 1 s window
data/cv_results.csv, results.json    cross-validation and test results used in the report
notebooks/hmm_activity_recognition.ipynb   full pipeline (executed, outputs included)
figures/                             figures used in the report
report/Formative2_HMM_Report.docx    4-5 page report
```

## Run
```
pip install -r requirements.txt
cd notebooks && jupyter notebook hmm_activity_recognition.ipynb     # Run All (~1-2 min)
```
All random choices are seeded (`SEED = 42`), so numbers are reproducible.

## Data
200 recordings (50 per activity), one phone: Samsung SM-G973F, Android, Sensor Logger 1.68, requested 100 Hz (measured 99.84 Hz).
To add recordings from another phone, put the Sensor Logger folders under `data/raw/<activity>/` and re-run; every stream is
resampled to the common 100 Hz grid by `harmonise()`.

## Key results (2 unseen test files, 325 windows)
| Model | Test accuracy |
|---|---|
| Supervised init + Baum-Welch (primary) | 94.15 % |
| Label-only (supervised) | 96.00 % |
| Per-window Gaussian, no transitions | 96.00 % |
| Random init + Baum-Welch | 94.15 % |

Test recordings come from the same phone and person as the training data: this measures generalisation to new recordings,
not to new people or devices.
