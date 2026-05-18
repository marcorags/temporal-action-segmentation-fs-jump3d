# Temporal Action Segmentation on FS-Jump3D

This repository contains a collaborative university project focused on **Temporal Action Segmentation (TAS)** using 3D pose sequences from the **FS-Jump3D** dataset.

We adapted a FACT-based Temporal Action Segmentation pipeline to a simplified figure skating scenario, where each sequence contains a single annotated action segment. The goal of the project is to evaluate how a temporal segmentation model originally designed for more complex action segmentation benchmarks behaves when applied to pose-only figure skating jump data.

---

## Project Context

This project was developed collaboratively by [**Marco Ragusa**](https://github.com/marcorags) and [**Edoardo Fiorentino**](https://github.com/edofiore) as part of the *Signal, Image & Video* course at the **University of Trento**.

The repository has been reorganized and documented to make the methodology, setup, and experimental choices easier to understand and reproduce.

---

## Overview

Temporal Action Segmentation aims to assign an action label to each frame of a temporal sequence. In standard TAS benchmarks, videos often contain multiple actions, transitions, and background segments.

In this project, we explored a more constrained setting based on figure skating jumps:

- the input data consists of 3D pose sequences;
- each sequence represents a figure skating jump trial;
- the segmentation setting is simplified to a single annotated action segment;
- the model is evaluated on pose-only temporal information, without RGB frames.

The project is based on the **Frame-Action Cross-Attention Temporal Modelling (FACT)** framework, adapted to work with the FS-Jump3D data format.

---

## Dataset

The project uses the [**FS-Jump3D**](https://github.com/ryota-skating/FS-Jump3D) dataset, which provides 3D pose sequences of figure skating jumps.

The dataset is organized around:

- athletes;
- jump types;
- individual jump trials;
- 3D skeleton keypoints over time.

Dataset files are **not included** in this repository. To reproduce the experiments, download the JSON version of FS-Jump3D from the official dataset source and place it according to the expected project structure.

Expected structure:

```text
temporal-action-segmentation-fs-jump3d/
├── CVPR2024-FACT/
│   └── data/
│       └── json/
│           └── ...
├── utils/
├── requirements.txt
└── README.md
```



## Methodology

The project follows a pipeline designed to adapt a Temporal Action Segmentation framework to 3D pose-based figure skating data.

First, we downloaded the FS-Jump3D dataset in JSON format. The dataset contains 3D pose sequences representing figure skating jump trials.

Then, we formatted the raw JSON files into the structure expected by the FACT framework. This step involved preparing the pose sequences, organizing the input files, and adapting the dataset representation to the temporal segmentation pipeline.

After preprocessing, we configured the FACT framework for the FS-Jump3D setting. Since the original framework was designed for standard Temporal Action Segmentation benchmarks, we adapted the experimental setup to a simplified scenario where each sequence contains a single annotated action segment.

Finally, we trained and evaluated the model on the formatted 3D pose sequences. The goal was to analyze whether a FACT-based temporal model could be applied to pose-only figure skating jump data, while also identifying the limitations of this simplified setup.

## What We Built

In this project, we adapted a Temporal Action Segmentation pipeline to work with 3D pose sequences from the FS-Jump3D dataset.

More specifically, we:

- integrated the FACT framework as the base Temporal Action Segmentation model;
- prepared utility scripts to format FS-Jump3D JSON pose data for the FACT pipeline;
- organized the input data structure required for training and evaluation;
- configured the project for a simplified pose-based segmentation setting;
- worked with 3D skeleton sequences instead of RGB video features;
- analyzed the applicability and limitations of FACT in this constrained figure skating scenario.

The final repository combines the original FACT framework with additional formatting and utility code needed to run experiments on FS-Jump3D.

## Repository Structure

```text
.
├── CVPR2024-FACT/                   # FACT framework used as Git submodule
├── utils/                           # Project-specific preprocessing and visualization utilities
│   ├── format.py                    # Formats FS-Jump3D JSON data for the FACT pipeline
│   ├── rig.json                     # Skeleton / pose mapping used during preprocessing
│   └── visualization_pose3d.py      # Utility script for inspecting 3D pose sequences
├── docs/
│   └── Project_Report.pdf           # Project report
├── requirements.txt                 # Python dependencies
├── .gitmodules                      # Git submodule configuration
├── .gitignore
└── README.md
```

The utils/ directory contains the additional scripts used to prepare and inspect the FS-Jump3D pose data for this project.

## Relation with FACT

This repository uses [CVPR2024-FACT](https://github.com/marcorags/CVPR2024-FACT) as a Git submodule.

The submodule is a fork of the original FACT repository, used as the base Temporal Action Segmentation framework for this project. We adapted this fork to support the FS-Jump3D experimental setup, including project-specific configuration and evaluation changes.

The main repository contains the project documentation, preprocessing utilities, setup instructions, and FS-Jump3D-specific workflow. The `CVPR2024-FACT/` submodule contains the FACT framework used for training and evaluation.

The submodule is pinned to a specific commit to keep the experimental setup reproducible.

## Installation

Clone the repository together with its submodule:

```bash
git clone --recurse-submodules https://github.com/marcorags/temporal-action-segmentation-fs-jump3d.git
cd temporal-action-segmentation-fs-jump3d
```

If the repository was already cloned without the submodule, initialize it with:

```bash
git submodule update --init --recursive
```

Install the required Python dependencies:

```bash
pip install -r requirements.txt
```

Recommended environment:

```text
Python >= 3.8
PyTorch 1.12.0
CUDA-compatible GPU recommended
```

## Data Preparation

Download the [FS-Jump3D dataset](https://github.com/ryota-skating/FS-Jump3D) in JSON format from the official dataset source.

Then, create the data directory inside the FACT framework folder:

```bash
mkdir -p CVPR2024-FACT/data
```

Place the downloaded JSON dataset inside the `CVPR2024-FACT/data/` directory.

The expected structure is:

```text
temporal-action-segmentation-fs-jump3d/
├── CVPR2024-FACT/
│   └── data/
│       └── json/
│           └── ...
├── utils/
│   └── format.py
├── requirements.txt
└── README.md
```

After placing the dataset in the correct location, run the formatting script from the repository root:

```bash
python utils/format.py
```

This script preprocesses the FS-Jump3D JSON files and converts them into the format required by the FACT training pipeline.

## Training

After formatting the FS-Jump3D data, the model can be trained using the FACT training pipeline.

From the repository root, run:

```bash
python -m CVPR2024-FACT.train --cfg CVPR2024-FACT/configs/fsjump.yaml
```

The training configuration can be modified in:

```text
CVPR2024-FACT/configs/fsjump.yaml
```

This file contains the main parameters used to configure the model, dataset paths, and training setup for the FS-Jump3D experiment.

## Evaluation

After training, the model can be evaluated with the FACT evaluation script.

From the repository root, run:

```bash
python -m CVPR2024-FACT.eval
```

The evaluation step is used to analyze the behavior of the adapted FACT model on the simplified FS-Jump3D Temporal Action Segmentation setting.

## Results

The project focuses on evaluating the applicability of a FACT-based Temporal Action Segmentation model to pose-only figure skating jump sequences.

In this simplified setup, each sequence contains a single annotated action segment. This makes the task different from standard Temporal Action Segmentation benchmarks, where videos usually contain multiple action classes, transitions, and background segments.

| Setting | Input Data | Task | Main Observation |
|---|---|---|---|
| FACT adaptation on FS-Jump3D | 3D pose sequences | Simplified Temporal Action Segmentation | The model was tested in a constrained scenario with one annotated action segment per sequence. |
| Pose-only setup | 3D skeleton coordinates | Frame-level temporal modeling | The experiment evaluates temporal reasoning without using RGB frames or appearance features. |

Detailed discussion of the experimental setup, assumptions, and interpretation is available in the [project report](./docs/Project_Report.pdf).

## Limitations

This project works with a simplified version of the Temporal Action Segmentation task.

The main limitations are:

- each sequence contains a single annotated action segment;
- the setup does not model transitions between multiple action classes;
- background actions are not explicitly represented;
- the model uses 3D pose data only, without RGB frames or visual appearance features;
- fine-grained figure skating phase segmentation is not fully addressed in this version.

Because of these constraints, the project should be interpreted as an adaptation and feasibility study rather than as a complete fine-grained Temporal Action Segmentation benchmark.

## Future Work

Possible extensions of this project include:

- adding fine-grained phase labels for figure skating jump procedures;
- modeling multiple temporal phases within each jump sequence;
- introducing background or transition classes;
- comparing FACT with simpler temporal baselines;
- improving the preprocessing pipeline;
- adding visualizations of predicted vs. ground-truth temporal segments;
- evaluating performance separately across different jump types;
- integrating additional features beyond 3D pose coordinates.

## Contributors

This project was developed collaboratively by:

- Marco Ragusa
- Edoardo Fiorentino

  ## Report

A detailed explanation of the project, methodology, experimental setup, and discussion is available in the project report:

[Read the project report](./docs/Project_Report.pdf)

## Credits and References

This project builds on and refers to the following resources:

- [CVPR2024-FACT](https://github.com/ZijiaLewisLu/CVPR2024-FACT)
- [FS-Jump3D](https://github.com/ryota-skating/FS-Jump3D)
- [3D Pose-Based Temporal Action Segmentation for Figure Skating: A Fine-Grained and Jump Procedure-Aware Annotation Approach](https://arxiv.org/abs/2408.16638)
