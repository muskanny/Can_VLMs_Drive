# Do VLMs Really Look at the Road? 

## ACCV 2026

<!-- TODO: replace with the final author list exactly as on the camera-ready paper -->
Author One, Author Two, Shankar Gangisetty, ...

<!-- TODO: fill in real links once available; remove any that do not apply -->
- [Paper (arXiv)](https://arxiv.org/abs/XXXX.XXXXX)
- [Project Webpage](https://<your-project-page>.github.io/)
- [HF Dataset](https://huggingface.co/datasets/<org>/<dataset>)

A diagnostic evaluation benchmark that probes whether vision–language models (VLMs)
make driving decisions from **visual evidence** or from **language priors, answer
biases, and dataset regularities**.

## Table of Contents

- [Introduction](#introduction-)
- [Benchmark](#benchmark-)
- [Installation](#installation-)
- [Data Curation](#data-curation-)
- [Evaluation](#evaluation-)
- [Results](#results-)
- [Acknowledgements](#acknowledgements-)
- [BibTeX Citation](#bibtex-citation-)

## Introduction :

<p align="center">
  <img src="./images/Teaser_Diagram.pdf" alt="Do VLMs Really Look at the Road?">
</p>

---

Vision–language models are increasingly explored as interpretable reasoning modules
for autonomous driving. Yet high accuracy on driving question-answering benchmarks
does not establish that a model is actually reasoning from the road scene. A model
that ignores the image but answers correctly through dataset bias appears competent
while being fundamentally unreliable.

We introduce a **diagnostic evaluation framework** that moves beyond aggregate
accuracy to probe four complementary aspects of driving reliability:

- **Visual grounding** — image-conditioned vs. no-image (blank) evaluation;
- **Counterfactual scene sensitivity (SCS)** — paired clear / fog / rain / snow scenes;
- **Linguistic robustness** — paraphrase consistency and negation accuracy;
- **Spatial reasoning** — relational questions on a DriveLM/nuScenes extension.

All ground-truth labels are derived **programmatically** from BDD100K and
DriveLM/nuScenes annotations, so the benchmark is reproducible without manual
labeling. Across a range of open-source VLMs we find that competitive overall
accuracy conceals systematic failure modes: strong affirmation bias, limited
sensitivity to counterfactual weather changes, near-chance negation accuracy despite
high paraphrase consistency, and confident driving decisions even without any image.

## Benchmark :

The benchmark spans two task categories plus a spatial-reasoning extension:

| Category | Source | Images | QA pairs |
|---|---|---|---|
| Adverse Weather | BDD100K (+ counterfactuals) | 180 | see paper |
| Junctions & Intersections | BDD100K | 140 | see paper |
| Spatial Reasoning extension | DriveLM / nuScenes (CAM_FRONT) | 200 | see paper |
| **Total** | | **520** | **26,788** |

Six open-source VLMs are evaluated zero-shot: **Moondream-2B**, **SmolVLM-2.2B**,
**PaliGemma-3B**, **Qwen2.5-VL-7B**, **InternVL3-8B**, and **LLaVA-OneVision-8B**.

## Installation 🔧:

### Prerequisites
- Python >= 3.9
- CUDA-capable GPU

We used this setup:
- NVIDIA RTX 3080
- Python 3.9

```shell
git clone https://github.com/muskanny/Can_VLMs_Drive.git
cd Can_VLMs_Drive

conda create -n vlm python=3.9 -y
conda activate vlm

pip install -r requirements.txt   # transformers, torch, pillow, openpyxl, ...
```

Model weights are downloaded from the Hugging Face Hub on first run. Each VLM is
loaded with the Hugging Face `transformers` library in `bfloat16` precision.

## Data Curation :

Ground truth is generated entirely through **deterministic derivation functions** on
existing dataset annotations — no manual question-answer labeling. Each question is
mapped to a binary label from annotation attributes (weather, time of day, scene
type, traffic-light state, per-class object counts, bounding-box geometry, occlusion
metadata; and, for nuScenes, camera-attributed 2D object positions in CAM_FRONT).

Questions are written from the **driver's perspective** (e.g. *"Should the driver
turn on the headlights?"* rather than *"Is it raining?"*), and the correct answer is
designed to vary across visually similar scenes so that models cannot rely on
condition labels alone. Each question is further expanded into two meaning-preserving
paraphrases and one negated variant.

**-** The expected data / manifest layout:

```
Can_VLMs_Drive
├── manifests/
│    ├── aw_manifest_v2.json
│    ├── ji_manifest_v1.json
│    ├── ns_manifest_200_extended.json
│    └── ...                       # counterfactual & linguistic manifests
├── images/                        # benchmark images by category
├── code/                          # evaluation + analysis code
├── scripts/                       # SLURM / run scripts
└── results/                       # per-model CSV outputs
```

## Evaluation :

Each model is evaluated across the full protocol matrix — with-image (Mode A),
no-image (Mode C), counterfactual weather pairs, and linguistic variants — writing
one CSV per (model, category, setting). To evaluate a model, set its identifier in
the eval script and run the protocol matrix:

```shell
# with image (Mode A)
python code/eval.py --category aw --mode A
python code/eval.py --category ji --mode A
python code/eval.py --category ns --mode A

# no-image baseline (Mode C)
python code/eval.py --category aw --mode C
python code/eval.py --category ji --mode C

# counterfactual weather (Adverse Weather)
python code/eval.py --category aw --mode A --manifest $AW_CF   --suffix _cf
python code/eval.py --category aw --mode C --manifest $AW_CF   --suffix _cf

# linguistic variation (Junctions & Intersections)
python code/eval.py --category ji --mode A --manifest $JI_LING --suffix _ji_ling
python code/eval.py --category ji --mode C --manifest $JI_LING --suffix _ji_ling
```

To compute the diagnostic metrics (overall / GT-conditioned accuracy, affirmation
gap, SCS, paraphrase consistency, negation accuracy) from the CSV outputs:

```shell
python code/analyze.py --results results/
```

## Results :

Diagnostic metrics for all evaluated models are reported in the paper. Raw
per-question outputs are provided under `results/`, organized by category
(Adverse Weather, Junctions & Intersections, nuScenes) and evaluation setting.

## Acknowledgements 

<!-- TODO: confirm funding / institutional acknowledgements -->
This work was supported by IIIT Hyderabad.

The evaluation builds on the following open-source VLMs and datasets:
[BDD100K](https://bdd-data.berkeley.edu/), [DriveLM](https://github.com/OpenDriveLab/DriveLM),
[nuScenes](https://www.nuscenes.org/), and the Hugging Face `transformers` ecosystem.

## BibTeX Citation 

If you use this benchmark, please cite our paper:

<!-- TODO: update page numbers / details once proceedings are published -->
```bibtex
@inproceedings{vlmsdrive2026,
  title     = {Do VLMs Really Look at the Road? A Diagnostic Evaluation for Autonomous Driving},
  author    = {TODO: author list},
  booktitle = {Proceedings of the Asian Conference on Computer Vision (ACCV)},
  year      = {2026}
}
```


Here are all the canonical result files grouped by category:

---

**Adverse Weather (AW) — Phase 1**
```
adverse_weather_moondream_v2.csv
adverse_weather_moondream_noimage_v2.csv
adverse_weather_paligemma_v2.csv
adverse_weather_paligemma_noimage_v2.csv
adverse_weather_smolvlm_v2.csv
adverse_weather_smolvlm_noimage_v2.csv
adverse_weather_llava_ov_v2.csv
adverse_weather_llava_ov_noimage_v2.csv
adverse_weather_internvl3_v2.csv
adverse_weather_internvl3_noimage_v2.csv
```

**AW Counterfactual**
```
adverse_weather_moondream_cf.csv
adverse_weather_moondream_noimage_cf.csv
adverse_weather_paligemma_cf.csv
adverse_weather_paligemma_noimage_cf.csv
adverse_weather_smolvlm_cf.csv
adverse_weather_smolvlm_noimage_cf.csv
adverse_weather_llava_ov_cf.csv
adverse_weather_llava_ov_noimage_cf.csv
adverse_weather_internvl3_cf.csv
adverse_weather_internvl3_noimage_cf.csv
```

**Junctions & Intersections (JI) — Phase 1**
```
junctions_moondream_ji_v1_fixed.csv
junctions_moondream_noimage_ji_v1.csv
junctions_paligemma_ji_v1_fixed.csv
junctions_paligemma_noimage_ji_v1.csv
junctions_smolvlm_ji_v1_fixed.csv
junctions_smolvlm_noimage_ji_v1.csv
junctions_llava_ov_ji_v1_fixed.csv
junctions_llava_ov_noimage_ji_v1.csv
junctions_internvl3_ji_v1_fixed.csv
junctions_internvl3_noimage_ji_v1.csv
```

**JI Linguistic**
```
junctions_moondream_ji_ling.csv
junctions_moondream_noimage_ji_ling.csv
junctions_paligemma_ji_ling.csv
junctions_paligemma_noimage_ji_ling.csv
junctions_smolvlm_ji_ling.csv
junctions_smolvlm_noimage_ji_ling.csv
junctions_llava_ov_ji_ling.csv
junctions_llava_ov_noimage_ji_ling.csv
junctions_internvl3_ji_ling.csv
junctions_internvl3_noimage_ji_ling.csv
```

**nuScenes (NS)**
```
nuscenes_moondream_ns.csv
nuscenes_paligemma_ns_fixed.csv
nuscenes_smolvlm_ns_fixed.csv
nuscenes_llava_ov_ns.csv
nuscenes_internvl3_ns.csv
```

**Phase 2 Verbose**
```
phase2_aw_llava_ov.csv
phase2_aw_llava_ov_noimage.csv
phase2_aw_internvl3.csv
phase2_aw_internvl3_noimage.csv
phase2_ji_llava_ov.csv
phase2_ji_llava_ov_noimage.csv
phase2_ji_internvl3.csv
phase2_ji_internvl3_noimage.csv
```

**Analysis JSONs**
```
aw_cf_analysis.json
aw_linguistic_analysis.json
junctions_analysis.json
junctions_linguistic_analysis.json
```

That's 58 canonical files total. Everything else in the results folder is deprecated. What do you need them for — GitHub push, analysis, or something else?
