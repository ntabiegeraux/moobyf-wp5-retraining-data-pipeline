# moobyf-wp5-retraining-data-pipeline

Prepares the training data used to retrain the MOOBYF (WP5) video species detection model.

It takes the annotations already used for the previous model, adds the newly validated ones, and splits everything into a **training** and a **validation** set (about 80/20) ready for training.

## What it does

1. Reads the new annotation files and keeps only the validated lines.
2. Merges them with the previous annotations.
3. Splits the data **by video**: all annotations of a video go to the same set, so the model is never validated on videos it has seen. Videos are assigned so that each class is as close as possible to 80 % training / 20 % validation, using a greedy algorithm followed by an arrangement step on the videos of under-represented classes. Classes listed in `EXCLUDED` are left out.

## Input data

Data is not stored in this repository. Expected layout:

    data/
    ├── old_annotations/*.csv       previous annotations
    └── new_annotations/<batch>/*.csv         new annotation files, one folder per batch

## Output

Created in `output/<YYYYMMDD>/` (run date):

| File | Content |
|---|---|
| `new_annotation_<date>.csv` | validated new annotations |
| `new_and_old_annotation_<date>.csv` | old and new annotations merged |
| `splitted_annotation_<date>.csv` | same, with a `subset` column (training / validation) |
| `splitted_frame_<date>.csv` | subset of each frame |

## Setup

Requires [uv](https://docs.astral.sh/uv/).

    git clone https://github.com/ntabiegeraux/moobyf-wp5-retraining-data-pipeline.git
    cd moobyf-wp5-retraining-data-pipeline
    uv sync

Run `uv sync` again after any `git pull` that changes `pyproject.toml` or `uv.lock`.

## Run

1. Open `merge_and_split_annotation.ipynb` and select the `.venv` kernel.
2. Set the paths and parameters in the first cells.
3. Run **Kernel > Restart Kernel and Run All Cells**.
4. Check the printed average deviation from 80 % (lower is better) and the share of annotations in each set per specie.

## Contact

TABIE-GERAUX NAWFEL, IRD, MARBEC
Email: ntabiegeraux@proton.me
GitHub: [@ntabiegeraux](https://github.com/ntabiegeraux)
