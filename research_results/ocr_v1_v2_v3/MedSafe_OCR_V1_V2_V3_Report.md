# MedSafe OCR — Consolidated V1–V3 Research Report

## Objective

Evaluate handwritten prescription word recognition
and investigate the accuracy–substitution-risk trade-off
introduced by training-vocabulary correction.

## Dataset

RxHandBD:
- Training: 4,017 images
- Validation: 446 images
- Official test: 1,115 images

## Methods

**V1:** Fine-tuned TrOCR-Base-Handwritten.

**V2-A:** Nearest training-vocabulary correction,
maximum Levenshtein edit distance 2.

**V2-B:** Unrestricted nearest training-vocabulary
correction.

**V3:** Selective vocabulary correction,
maximum edit distance 2 and similarity margin >= 15.

## Official Test Comparison

| method                  |      cer |      wer |   exact_accuracy |   correct |   changed |   fixed |   harmed |
|:------------------------|---------:|---------:|-----------------:|----------:|----------:|--------:|---------:|
| V1 Raw TrOCR            | 0.356314 | 0.716249 |         0.329148 |       367 |         0 |       0 |        0 |
| V2-A Edit distance <= 2 | 0.348669 | 0.641876 |         0.405381 |       452 |       430 |     129 |       44 |
| V2-B Nearest vocabulary | 0.35413  | 0.612773 |         0.408072 |       455 |       765 |     171 |       83 |
| V3 Selective            | 0.347986 | 0.679871 |         0.365919 |       408 |       109 |      48 |        7 |

## Key Findings

1. V1 exact-match accuracy: 32.91%.
2. V2-A exact-match accuracy: 40.54%, with 44
   originally correct predictions damaged.
3. V2-B exact-match accuracy: 40.81%, with 83
   originally correct predictions damaged.
4. V3 exact-match accuracy: 36.59%, with 7
   originally correct predictions damaged.
5. V3 reduced observed harmful substitutions
   relative to both V2 variants, while sacrificing
   exact-match accuracy.
6. String similarity is not sufficient to verify
   medication identity.

## Limitations

- The test comparison is exploratory because the
  official test set was inspected during development.
- No independent external validation dataset.
- No calibrated uncertainty estimates.
- No verified pharmaceutical terminology database.
- No full-prescription OCR evaluation.
- No clinical safety or deployment validation.
- Correction harm counts refer to originally correct
  OCR outputs changed to incorrect strings, not
  clinically verified adverse events.

## Conclusion

Vocabulary-aware correction can improve handwritten
word recognition accuracy, but introduces additional
incorrect substitutions. Selective correction reduces
the number of such substitutions without eliminating
them. Further research should investigate external
verification and uncertainty-aware abstention.
