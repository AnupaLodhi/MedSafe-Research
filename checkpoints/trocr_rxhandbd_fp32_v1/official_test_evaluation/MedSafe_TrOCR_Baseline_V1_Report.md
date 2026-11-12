# MedSafe Research — TrOCR Baseline V1

## Experiment Overview
- Dataset: RxHandBD
- Task: Handwritten prescription word recognition
- Model: microsoft/trocr-base-handwritten
- Selected checkpoint: Epoch 2
- Checkpoint selection: Lowest validation CER
- Training samples: 4,017
- Validation samples: 446
- Official test samples: 1115
- Test set: Original held-out RxHandBD split
- Exact duplicate overlap with training/validation: 0
- Test normalization: strip and casefold
- Test decoding: max_new_tokens=32, num_beams=1

## Training Configuration
- Epochs: 3
- Batch size: 2
- Gradient accumulation: 4
- Effective batch size: 8
- Optimizer: AdamW
- Learning rate: 2e-5
- Weight decay: 0.01
- Warmup steps: 75
- Learning-rate schedule: Linear decay
- Gradient clipping: 1.0
- Precision: FP32
- Hardware: NVIDIA Tesla T4
- Best checkpoint: Epoch 2

## Validation Results
- CER: 0.2379855167873601
- WER: 0.576171875
- Exact-match accuracy: 0.4327354260089686

## Official Test Results
- CER: 0.356314
- WER: 0.716249
- Exact-match accuracy: 0.329148
- Correct predictions: 367
- Incorrect predictions: 748
- Test images: 1115

## Accuracy by Ground-Truth Word Length

| length_group   |   samples |   exact_accuracy |   mean_word_cer |
|:---------------|----------:|-----------------:|----------------:|
| 1-4            |        93 |        0.548387  |        0.354839 |
| 5-7            |       749 |        0.384513  |        0.307159 |
| 8-10           |       239 |        0.112971  |        0.418608 |
| 11-15          |        34 |        0.0294118 |        0.5755   |

## Example OCR Errors

| reference   | prediction           |   word_cer |
|:------------|:---------------------|-----------:|
| Nexcital    | nexicalal            |   0.375    |
| Inderen     | Indor                |   0.428571 |
| Indever     | Indorer              |   0.285714 |
| Losita      | Lasita               |   0.166667 |
| Rivotril    | Rotril               |   0.25     |
| Asynta      | Asanta               |   0.166667 |
| Econate     | Econte               |   0.142857 |
| Exeptim     | Exoptim              |   0.142857 |
| Lubilay     | lutililax            |   0.571429 |
| T-Cef       | Tablef               |   0.6      |
| Escilex     | Eslilax              |   0.285714 |
| Traxy 500   | Trax 5 5 5 5 5 5 5 5 |   1.55556  |
| HPR DS      | hpr d                |   0.166667 |
| Napa        | Galastorina          |   2.25     |
| Kacin       | proteinacin          |   1.4      |

## Observations
1. Test performance is worse than validation performance.
2. Exact-match accuracy decreases substantially for longer words.
3. Errors include substitutions, deletions, and repeated or hallucinated output.
4. The model is not suitable for autonomous clinical prescription interpretation.

## Limitations
- Evaluation is restricted to cropped handwritten words.
- Full prescription detection and transcription were not evaluated.
- No clinical safety or medication verification evaluation was performed.
- Exact image hashing does not exclude perceptual near-duplicates.
- No statistical uncertainty estimates are included.
- No comparison with independently trained OCR baselines is included.

## Saved Artifacts
- Best model: best_model/
- Predictions: official_test_evaluation/test_predictions.csv
- Metrics: official_test_evaluation/test_metrics.json
- Error analysis: official_test_evaluation/test_error_analysis.csv
- Figures: official_test_evaluation/figures/

## Research Status
Baseline V1 completed and frozen for future comparison.

Generated: 2026-11-10T15:44:10.598860+00:00
