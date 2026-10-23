# Using the released data

## 1. Download the source images

The images are intentionally excluded. Download DetectiumFire from:

<https://www.kaggle.com/datasets/yimengfuyao/detectiumfire>

For example, with KaggleHub:

```bash
python -m pip install kagglehub
python -c "import kagglehub; print(kagglehub.dataset_download('yimengfuyao/detectiumfire'))"
```

Call the printed path `<KAGGLE_VERSION_DIR>`. For DetectiumFire version 6, the
image-reference root is:

```text
<KAGGLE_VERSION_DIR>/real_images
```

Every `image` path stored in the released JSON is joined to this root. For
example:

```text
JSON: real_images/real_fire/images/example.jpg
Disk: <KAGGLE_VERSION_DIR>/real_images/real_images/real_fire/images/example.jpg
```

The extra `real_images` level reflects the upstream version 6 archive layout
and is intentional.

## 2. Released artifact

The canonical artifact is:

```text
data/fire_reasoning_distilled_box_robust.json
```

Its size, SHA-256 digest, record counts, coordinate system, and relative image
root are recorded in `data/manifest.json`.

For supervised fine-tuning, expose only each record's `message` field to the
student model. Resolve the image reference as described above. Do not insert
`box_provenance_private`, `distractor_generation_private`, or other private
audit fields into the model-visible prompt.

## 3. Construction availability

This repository is a data-only release. It documents the released artifact
and its schema but does not distribute the private construction, distillation,
cache, API configuration, or training scripts. Teacher API responses can vary
across service versions, so the released JSON file and the SHA-256 digest in
the manifest define the canonical version.
