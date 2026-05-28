---
title: "ML: Vessel Object Detection with YOLOv5"
date: 2023-07-02
status: "Completed"
tools:
  - Python
  - YOLOv5 (Ultralytics)
  - PyTorch
  - Pandas
  - Scikit-learn
  - Matplotlib
github:
demo:
kaggle: "https://www.kaggle.com/code/ma12492002/ml-object-detection-vessels-using-yolov5"
excerpt: "Training and evaluating a YOLOv5s model to detect 10 classes of maritime vessels and safety equipment in real-world waterway images — achieving 0.861 mAP@0.5 overall, with a structured post-processing pipeline to evaluate predictions against ground truth labels."
---

## The Short Version

A real-world object detection project that fine-tunes YOLOv5s on a maritime vessel dataset to detect 10 target classes — including Cabin Cruisers, Canoes/Kayaks, Commercial vessels, PWCs, Humans, and lifejackets (PFDs). Beyond training, the notebook builds a complete prediction evaluation pipeline: running inference on a new dataset, parsing YOLO's text output into a structured DataFrame, deduplicating by confidence score, and computing accuracy, precision, and recall against ground truth labels extracted from filename conventions.

## Problem

Can a single-stage object detector reliably identify vessel types and safety equipment from waterway surveillance images? And once the model is trained, how do you rigorously evaluate its per-class performance on a brand-new dataset without pre-existing annotation files?

## Approach

**Training** — YOLOv5 was cloned from the Ultralytics repository and dependencies installed. `yolov5s.pt` (the small pre-trained backbone) was fine-tuned with:
- Image size: 640×640
- Batch size: 32
- Epochs: 100
- `--nosave` (intermediate checkpoints suppressed) with `--cache` for faster data loading.

**Old dataset validation** — `val.py` was run against the trained weights on the held-out validation set, producing per-class mAP@0.5 and mAP@0.5:0.95 metrics alongside precision and recall. A confusion matrix and PR curve were loaded from saved image files and displayed for visual analysis.

**New dataset inference** — A second dataset (`New_DataSet`) was processed by looping through all images, calling `detect.py` via `os.popen()` with `conf=0.1`, and inserting a 6-second `time.sleep()` between calls to allow subprocess completion. A second inference pass on a richer image set used `detect_txt.py` with `--save-txt --save-conf` to write per-image label files.

**Post-processing pipeline** — The saved `.txt` label files were parsed into a structured DataFrame:
- Each line was split into class ID, bounding box coordinates, and confidence score.
- Class IDs were filtered against a `class_dict` mapping to retain only the target classes.
- Real class names were extracted from image filenames (using a naming convention where the class precedes a `-` separator), with speed suffixes (`under5kn`, `over5kn`) stripped.
- Duplicate detections per image were removed; for each image, only the highest-confidence prediction was retained via `groupby('image_name')['confidence_score'].idxmax()`.
- `HalfCab` was remapped to `Open` during ground truth normalization.

**Evaluation** — `accuracy_score`, `precision_score` (macro), and `recall_score` (macro) were computed against the ground truth labels. A full confusion matrix was plotted.

## Result

**Old dataset validation metrics (mAP@0.5):**

| Class | mAP@0.5 |
|---|---|
| Cabin Cruiser | 99.5% |
| Canoe/Kayak | 97.8% |
| Commercial | 99.5% |
| Half Cab | 99.1% |
| Human | 90.8% |
| Open | 96.4% |
| PFD (lifejacket) | 82.7% |
| PWC | 94.7% |
| Registration Number | 89.5% |
| Structure | 89.6% |
| **Overall** | **mAP@0.5: 0.861 / mAP@0.5:0.95: 0.625** |

Overall precision: 0.939, recall: 0.832.

**New dataset evaluation** — The confusion matrix revealed that on the new dataset, Cabin Cruisers and Human-Powered vessels were frequently misclassified as HalfCab/Open — likely a labeling consistency issue rather than a model failure. Commercial and Open class predictions were accurate. The notebook concludes with actionable diagnosis: remove underrepresented classes (Dinghy, House Boat, Windsurfer etc. that had no training examples), remove the `Other` class (100% misclassification rate), and add more Human and PFD training examples to address the two weakest per-class scores.

## What I Learned

The post-processing pipeline is the technically demanding part of this project — YOLOv5's inference output is a folder of plain text files, and converting that into a structured, deduplicated, class-matched DataFrame ready for `sklearn` evaluation metrics requires non-trivial parsing logic. The filename-as-ground-truth convention (extracting the real label from the image name) is a pragmatic engineering choice that avoids needing separate annotation files for the new dataset, but it's brittle — the speed suffix stripping and class remapping steps were both required to handle naming inconsistencies in the data. The confusion matrix on the new dataset also illustrates an important model evaluation principle: poor performance on a held-out set isn't always a model problem. When Cabin Cruisers are predicted as HalfCabs on entirely new data, the first question is whether the training labels were consistent — not whether the architecture is wrong.

## Links

{% if page.kaggle %}- [Kaggle]({{ page.kaggle }}){% endif %}
