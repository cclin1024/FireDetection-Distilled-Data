# Dataset card

## Summary

This dataset provides teacher-generated reasoning examples for fire-scene
understanding in the presence of noisy candidate detection boxes. It is based
on DetectiumFire images and annotations, while adding deterministic synthetic
distractor boxes that are spatially separated from reliable boxes.

No image bytes are included in this repository. The `image` fields are
relative references into a separately downloaded DetectiumFire installation.

## Image-reference root

Let `<KAGGLE_VERSION_DIR>` denote the directory returned by KaggleHub, such as
the directory ending in `detectiumfire/versions/6`. Resolve each record's
relative `image` value against:

```text
<KAGGLE_VERSION_DIR>/real_images
```

Thus `real_images/real_fire/images/example.jpg` refers to:

```text
<KAGGLE_VERSION_DIR>/real_images/real_images/real_fire/images/example.jpg
```

This extra level reflects the layout of DetectiumFire version 6 and is
intentional.

## Statistics

| Item | Count |
|---|---:|
| Successful records | 8,848 |
| Positive records | 7,518 |
| Negative records | 1,330 |
| Candidate boxes | 27,509 |
| Reliable boxes | 13,052 |
| Synthetic distractor boxes | 14,457 |
| Excluded failed records | 33 |

Risk-level distribution:

| Risk level | Count |
|---|---:|
| `no_risk` | 4,023 |
| `low_risk` | 136 |
| `medium_risk` | 1,578 |
| `high_risk` | 3,111 |

## Record schema

Each element of the JSON list contains the following important fields:

- `type`: dataset record type.
- `image`: relative path to the source image.
- `fire_type`: DetectiumFire fire-type label or `non_fire`.
- `source`: source-category metadata.
- `is_negative`: whether the record is a non-fire example.
- `message`: two-message multimodal SFT conversation.
- `detection_boxes`: mixed reliable and synthetic candidates shown to the
  student.
- `box_coordinate_system`: `normalized_xyxy_0_1000`.
- `box_provenance_private`: audit metadata identifying reliable and synthetic
  boxes.
- `distractor_generation_private`: requested/generated distractor counts and
  constraint status.
- `distractor_box_seed`: deterministic distractor seed.

The assistant content consists of a `<think>...</think>` reasoning section
followed by a JSON answer with:

- `risk_level`;
- `fire_type`;
- `burning_object`;
- `environment`;
- `summary`.

## Correct training usage

Use the `message` field as the model-visible training conversation. Do not
append `box_provenance_private` or `distractor_generation_private` to the user
prompt. Those fields exist only for dataset auditing and controlled analysis.

The image path in both `image` and `message[0].content` must be resolved against
the local DetectiumFire image root.

## Distractor-box construction

- Positive examples receive one or two distractor boxes by default.
- Negative examples receive two or three candidate boxes by default.
- Generation is deterministic per image and seed.
- A distractor may be disjoint from reliable boxes or overlap slightly.
- Intersection divided by the smaller box area must remain below 0.25 for
  every reliable box.

The teacher receives private provenance to construct grounded accept/reject
reasoning. The final student prompt contains only the mixed candidates.

## Quality controls and limitations

Outputs were checked for the required JSON fields, English-only responses,
risk/summary consistency, terminology leakage, and non-empty object and
environment fields. Thirty-three examples that still failed validation after
the configured retries were excluded.

The reasoning and semantic fields are model-generated and may contain subtle
visual or linguistic errors. The source data are imbalanced, particularly for
`low_risk`. The dataset must not be treated as a replacement for professional
fire-safety assessment.

## License and provenance

The released distilled annotations and accompanying documentation are
licensed under CC BY-NC-SA 4.0. The construction scripts are private and are
not part of this release. No DetectiumFire image bytes are included or
relicensed. DetectiumFire contains material from multiple sources; the
source-specific licenses and academic-use guidance in its paper continue to
apply. Teacher answers were produced through an Alibaba Cloud Model Studio
inference API and are AI-generated content. See the repository `LICENSE` for
scope, attribution, and service-term cautions.

