# FireDetection Distilled Data

This repository contains a box-robust fire-reasoning distillation dataset
derived from the DetectiumFire image dataset. The images themselves are not
redistributed here.

## Contents

```text
data/
  fire_reasoning_distilled_box_robust.json
  manifest.json
docs/
  DATASET_CARD.md
  DATA_USAGE.md
LICENSE
```

This is a **data-only release**. The private construction and distillation
scripts are not included.

The release contains 8,848 successful examples:

- 7,518 positive fire/flame examples;
- 1,330 negative non-fire examples;
- 27,509 candidate boxes in total;
- deterministic synthetic distractor boxes for robustness training;
- teacher-generated natural-language reasoning and structured answers.

The student-facing prompt contains the image reference and mixed candidate
boxes, but does not reveal which boxes are reliable or synthetic. Private box
provenance is retained separately at the top level for auditing and must not
be inserted into the student input.

## Source images

Download DetectiumFire from the official resources:

- Dataset: <https://www.kaggle.com/datasets/yimengfuyao/detectiumfire>
- Official project repository and `fire_prompts.json` metadata:
  <https://github.com/ZixuanLiu4869/DetectiumFire>
- Paper: <https://openreview.net/forum?id=vhHYTjMt9Z>

After downloading, keep the image and label directory structure described in
[`docs/DATA_USAGE.md`](docs/DATA_USAGE.md). Dataset entries use
relative image paths so that the repository can be used without embedding
machine-specific absolute paths. Resolve every record's `image` value against
the following directory (not directly against the Kaggle version directory):

```text
<KAGGLE_VERSION_DIR>/real_images
```

For example, the record path
`real_images/real_fire/images/example.jpg` resolves to
`<KAGGLE_VERSION_DIR>/real_images/real_images/real_fire/images/example.jpg`.

## Data format

The primary file is
[`data/fire_reasoning_distilled_box_robust.json`](data/fire_reasoning_distilled_box_robust.json).
It is a JSON list. Each record contains:

- source metadata and a relative image path;
- an OpenAI-style `message` conversation;
- candidate detection boxes in normalized `[0,1000]` XYXY coordinates;
- private provenance describing reliable and injected boxes;
- deterministic distractor-generation metadata.

For supervised fine-tuning, use only the `message` conversation and resolve
its relative image path against your local DetectiumFire root. See the dataset
card for the complete schema and cautions.

## Licensing and attribution

The distilled annotations and accompanying documentation are released under
**CC BY-NC-SA 4.0**; see [`LICENSE`](LICENSE). This conservative
non-commercial license follows the licensing and academic-use guidance in the
DetectiumFire paper. DetectiumFire combines sources with different licenses,
so third-party material remains subject to its original license and terms.

This repository does not redistribute DetectiumFire images or grant rights to
them. Users must download the images independently, cite DetectiumFire, and
comply with the source-specific terms described in its paper. The reasoning
text is AI-generated using an Alibaba Cloud Model Studio inference API and may
be redistributed only when the account/service terms governing the generation
permit it; trial-service outputs can be subject to stricter restrictions.

