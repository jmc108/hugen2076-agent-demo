# Participant export codebook

This is a fictional dataset containing six participant records.

| Source column | Meaning and rules |
|---|---|
| `subject_id` | Text identifier. Trim outer whitespace; preserve leading zeros. |
| `sex_code` | `1` = `male`; `2` = `female`; `9` = `unknown`. |
| `age` | Age in years; `-9` denotes missing; valid range 18–90 inclusive. |
| `height_cm` | Height in centimeters; valid range 120–220 inclusive. |
| `weight_kg` | Weight in kilograms; `-9` denotes missing; valid range 30–250 inclusive. |

Keep all six rows in their original order. Replace documented missing codes
with `NA`. Values outside documented ranges become `NA` and must be recorded
in `reports/issues.tsv` with the source value and record identifier.

Required cleaned output: `outputs/participants_clean.tsv`, with columns
`subject_id`, `sex`, `age`, `height_cm`, `weight_kg`, in that order.

Also produce `reports/summary.md`, describing counts and transformations.
Do not infer meanings for undocumented codes or silently discard records.
