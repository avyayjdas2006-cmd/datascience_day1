# Day 1 Task — World Happiness Report EDA

## Dataset Overview
The [World Happiness Report](https://worldhappiness.report/) dataset scores 153 countries on a
`Happiness Score` (0–10, derived from a Gallup World Poll "life ladder" survey question) and six
factors researchers use to explain that score:

| Column | Meaning |
|---|---|
| `Happiness Rank` | Country's rank by happiness score |
| `Happiness Score` | Overall national wellbeing score (target variable) |
| `Economy` | GDP per capita contribution |
| `Family` | Social support contribution |
| `Health` | Healthy life expectancy contribution |
| `Freedom` | Perceived freedom to make life choices |
| `Generosity` | Generosity contribution |
| `Corruption` | Perceived absence of government/business corruption |
| `Dystopia` | Fixed baseline value added to all factor scores |
| `Job Satisfaction` | Separate survey metric on job satisfaction |
| `Region` | Geographic region |

**Shape:** 153 rows × 12 columns. Source data compiled from the annual World Happiness Report.

## Business Problem(s)
1. **Driver analysis** — Which factors (economic, social, health, freedom, trust) most strongly
   influence a country's happiness score, so policymakers can prioritize the interventions with the
   biggest wellbeing payoff?
2. **Happiness prediction** — Predict a country's happiness score from its economic/social
   indicators, useful for estimating wellbeing where survey data is incomplete.
3. **Country segmentation** — Group countries into wellbeing-profile clusters independent of
   geography, for cross-regional policy benchmarking.

## ML Problem Framing
- **Primary: Regression.** `Happiness Score` is continuous, so predicting it (and reading off
  feature importances/coefficients) is a regression task — this addresses both the driver-analysis
  and prediction problems in one model.
- **Secondary: Clustering.** For segmentation, unsupervised clustering (e.g., K-Means) on the
  factor columns groups countries by wellbeing profile without relying on `Region` or the target.
- **Why not classification:** there's no natural discrete label — turning the score into "happy /
  not happy" buckets would require an arbitrary threshold and throw away information the continuous
  score already captures.

## Target Variable & Key Features
- **Target:** `Happiness Score`
- **Key features:** `Economy`, `Family`, `Health`, `Freedom`, `Generosity`, `Corruption`,
  `Dystopia`, `Job Satisfaction`, `Region`
- **Excluded:** `Happiness Rank` — it's derived directly from the target and would leak the answer.

## Three Key Observations
1. **Economy and Job Satisfaction are the strongest linear drivers of happiness** (r ≈ 0.81 each),
   ahead of `Health` (r ≈ 0.78) and `Family` (r ≈ 0.75). `Generosity` is nearly uncorrelated
   (r ≈ 0.16) — generosity alone doesn't predict how happy a country's citizens report being.
2. **Regional inequality is large and structural.** Mean happiness ranges from ~6.9 in Western
   Europe down to ~4.2 in Africa — a gap of almost 3 points on a 10-point scale, bigger than what
   any single factor explains on its own.
3. **Missingness is small but not random.** Only `Job Satisfaction` has missing values (2 of 153
   rows: North Cyprus and South Sudan), both countries with limited international survey
   infrastructure — suggesting missingness may track data-collection capacity rather than
   happiness itself.

## Files
- `analysis.ipynb` — full Pandas exploration (shape, dtypes, missing values, summary stats,
  correlations, regional breakdown) with executed outputs.
- `world_happiness_report.csv` — dataset used in the analysis.

## Submission Tag
`#evn-ds-epochs26-day01`
