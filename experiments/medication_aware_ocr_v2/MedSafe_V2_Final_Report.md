# MedSafe — Vocabulary-Aware OCR Experiment V2

## Research Objective

Evaluate whether post-processing TrOCR predictions with
a vocabulary constructed exclusively from training labels
improves handwritten prescription word recognition.

## Dataset and Model

- Dataset: RxHandBD
- Training: 4,017 images
- Validation: 446 images
- Official test: 1,115 images
- OCR model: TrOCR-Base-Handwritten
- Selected checkpoint: Epoch 2
- Training vocabulary: 1,193 unique entries
- Validation vocabulary coverage: 80.27%

## Correction Method

- Candidate retrieval: RapidFuzz fuzzy string ratio
- Distance constraint: Levenshtein edit distance
- V2-A: Maximum edit distance 2
- V2-B: Unrestricted nearest-vocabulary candidate
- Exact vocabulary matches remain unchanged
- Empty predictions remain unchanged
- No additional OCR training

## Validation Results

| method              |      cer |      wer |   exact_accuracy |   correct |   changed |   fixed |   harmed |
|:--------------------|---------:|---------:|-----------------:|----------:|----------:|--------:|---------:|
| Raw TrOCR           | 0.237986 | 0.576172 |         0.432735 |       193 |         0 |       0 |        0 |
| Nearest vocabulary  | 0.196182 | 0.402344 |         0.612108 |       273 |       246 |      92 |       12 |
| Max edit distance 1 | 0.231402 | 0.533203 |         0.488789 |       218 |        59 |      27 |        2 |
| Max edit distance 2 | 0.213957 | 0.466797 |         0.556054 |       248 |       138 |      59 |        4 |
| Max edit distance 3 | 0.205727 | 0.441406 |         0.585202 |       261 |       190 |      78 |       10 |

## Official Test Results

| method                  |      cer |      wer |   exact_accuracy |   correct |   changed |   fixed |   harmed |
|:------------------------|---------:|---------:|-----------------:|----------:|----------:|--------:|---------:|
| V1 Raw TrOCR            | 0.356314 | 0.716249 |         0.329148 |       367 |         0 |       0 |        0 |
| V2-A Edit distance <= 2 | 0.348669 | 0.641876 |         0.405381 |       452 |       430 |     129 |       44 |
| V2-B Nearest vocabulary | 0.35413  | 0.612773 |         0.408072 |       455 |       765 |     171 |       83 |

## Safety Analysis

| method   |   changed |   fixed |   harmed |   harmful_change_rate |
|:---------|----------:|--------:|---------:|----------------------:|
| V2-A     |       430 |     129 |       44 |              0.102326 |
| V2-B     |       765 |     171 |       83 |              0.108497 |

## Key Findings

1. Vocabulary correction improved exact-match accuracy
   relative to raw TrOCR.
2. V2-A achieved 40.54% exact-match accuracy on the test set,
   compared with 32.91% for raw TrOCR.
3. V2-B achieved 40.81% exact-match accuracy but introduced
   83 harmful corrections.
4. V2-A introduced 44 harmful corrections.
5. The methods can replace correctly recognized medication
   names with different, incorrect names.
6. Neither method is suitable for autonomous clinical use.

## Research Limitations

- The vocabulary comes from training labels and is not
  a verified pharmaceutical dictionary.
- Some validation words are absent from the vocabulary.
- Edit-distance thresholds do not represent calibrated
  confidence or clinical safety.
- The test set was inspected during earlier V1 analysis,
  so the V2 comparison is exploratory.
- Evaluation uses cropped word images rather than
  complete prescriptions.
- No independent external dataset has been evaluated.

## Conclusion

Training-vocabulary correction can improve word-level
recognition accuracy, but introduces clinically concerning
substitution risks. Future work should investigate
selective prediction, uncertainty estimation, and
independent medication-name verification.

## Reproducibility

Saved artifacts include:
- training_vocabulary.csv
- validation_predictions_epoch2.csv
- validation_method_comparison.csv
- official_test_v2_predictions.csv
- official_test_v2_comparison.csv
- v2_safety_summary.csv
- v2a_harmful_corrections.csv
- v2b_harmful_corrections.csv
- v2_correction_safety.png
