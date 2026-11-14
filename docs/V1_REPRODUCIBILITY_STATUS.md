# MedSafe TrOCR V1 — Reproducibility status

This document records verified evidence and remaining limitations. It is **not** a claim of full checkpoint-to-metric reproducibility.

## Model and source checks
- Saved checkpoint configurations identify a ViT encoder and TrOCR decoder, with FP32 tensors.
- Three checkpoint snapshots were inventoried: `best_model_previous`, `best_model`, and `epoch3_final_model`.
- The original `best_model` checkpoint loaded successfully in Colab and generated text on a synthetic image.
- Recovered `dataset_runtime_v3.py`, `training_engine_v2.py`, and `checkpoint_runtime_v3.py` passed syntax checks in the private Colab workspace. **These source files are not yet included in this branch.**

## Results recalculated from saved normalized prediction/reference pairs

| Split | Samples | CER | WER | Strict exact match |
| --- | ---: | ---: | ---: | ---: |
| Epoch-2 validation | 446 | 0.2379855167873601 | 0.576171875 | 0.4327354260089686 |
| Official test | 1115 | 0.3563139931740614 | 0.7162489894907034 | 0.3291479820627803 |

These figures match the previously stored aggregate metrics exactly. The computation used existing normalized CSV columns; it did **not** regenerate predictions from the checkpoint.

The saved epoch-3 validation CER is 0.24325213956550362; its evaluation was not independently rerun.

## Outstanding reproducibility work
- Verify checkpoint-to-prediction provenance, including whether the epoch-2 CSV corresponds to the original V1 `best_model` checkpoint.
- Verify exact validation/test split manifests and data-use permissions.
- Verify original text normalization and evaluation implementation.
- Review recovered source for licensing, secrets, personal information, and dataset paths before publishing.
- Document dependencies, commands, seeds, and model artifact access.

## Data and release policy
Do not commit patient images, private labels, row-level prediction/reference text, credentials, large model weights, or legacy training-state files to this public repository. Maintain restricted archival copies with access controls and documented provenance.

**Status:** Saved-prediction metric arithmetic reproduced; end-to-end checkpoint-to-metric reproducibility not yet established.
