# Handedness and Mortality

An educational cohort-composition demonstration: how changes in reported left-handedness across birth cohorts can produce apparent differences in age at death.

This Python project applies Bayes' theorem to historical handedness rates and a fixed 1999 US age-at-death distribution. It illustrates how birth-cohort composition can generate different modelled age profiles using the same underlying death distribution for both groups. **It does not estimate observed mortality differences or individual life expectancy.**

[View the notebook](handedness_mortallity_analysis.ipynb) · [Read the project report](Namrata_Muralidharan_Traineeship_Report_March24.pdf)

## Background

The project revisits the historical claim that left-handed people die around nine years earlier than right-handed people. An alternative explanation concerns birth cohorts: older generations were less likely to report being left-handed, partly because of social pressure to use the right hand. This can change the age composition of the two reported handedness groups in a sample of deaths.

The question is whether this change in reporting can generate an apparent age gap within the model.

## Data

| Dataset | Source | Use in the analysis |
| --- | --- | --- |
| Reported left-handedness by age and sex | Rates digitised from Gilbert and Wysocki's [*Hand preference and age in the United States* (1992)](https://pubmed.ncbi.nlm.nih.gov/1528408/), based on a 1986 National Geographic survey | Estimate reported handedness rates by birth year |
| US deaths by single year of age, 1999 | [CDC/NCHS mortality table](https://www.cdc.gov/nchs/data/statab/vs00199_table310.pdf) | Construct the overall age-at-death distribution |

The notebook loads prepared CSV and TSV versions directly from URLs hosted by `mbonsma` on GitHub Gist. These data files are not included in the repository, so running the notebook requires internet access.

## Analysis

1. Plot reported left-handedness rates by age and sex.
2. Convert age to birth year using the 1986 survey date and calculate an unweighted mean of the male and female rates.
3. Estimate handedness probabilities for each age in a specified study year. Outside the observed birth-year range, use constant rates calculated from the first or last ten data points.
4. Apply Bayes' theorem to reweight the common death distribution by reported handedness.
5. Compare the notebook's unrenormalised, age-weighted sums over ages 6–114 for the 1990 and 2018 scenarios.

Changing `study_year` changes the mapping between age and birth year. **Both scenarios retain the same 1999 mortality data.**

**Tools:** Python, pandas, NumPy, Matplotlib and Jupyter Notebook.

## Results: current notebook outputs

> **Age-range normalisation caveat:** The notebook sums age-weighted probabilities over ages **6–114**, but its probability denominators use the **full cleaned mortality table**. It does **not renormalise each handedness distribution over ages 6–114**. The retained probabilities therefore do not sum to one within each group. Its printed "average ages" are unrenormalised, age-weighted sums, not means over the full distribution or conditional means for ages 6–114. The gaps below preserve the current calculation, including this mismatch.

The saved outputs report right-handed minus left-handed differences between these sums, rounded to one decimal place:

| Study-year scenario | Printed model-based gap |
| --- | ---: |
| 1990 | 5.5 years |
| 2018 | 2.3 years |

Both rows use the same 1999 mortality distribution; only the age-to-birth-year mapping changes. These are outputs of an educational calculation, not observed mortality gaps in 1990 or 2018, estimates of years of life lost, or individual life-expectancy predictions.

## Interpretation

Older birth cohorts have lower reported left-handedness in the input data. Applying these rates to a shared distribution of deaths gives the two modelled handedness groups different age compositions. Changing `study_year` changes which birth cohorts are represented at each age. This demonstrates how cohort composition can generate an apparent age gap without introducing a handedness-specific survival process.

The demonstration establishes a possible mechanism under the model's assumptions. It does not establish how much of a historical observed gap that mechanism explains, or prove or disprove a causal effect of handedness on mortality. The smaller 2018 output is a comparison between scenarios with fixed mortality, not evidence that a real mortality gap declined.

## Limitations

- **Age range and normalisation:** The 6–114 sums omit probability mass retained in the denominators. Before reporting distribution means, use consistent age support throughout or renormalise each handedness distribution within ages 6–114. Such a change could alter the summaries and their gap; it is not implemented in the current notebook.
- **Aggregate, cross-year inputs:** The analysis combines survey-based handedness rates and population death counts from different years. These are not linked individual records, and the notebook does not follow individuals over time, estimate survival hazards or calculate life expectancy.
- **Fixed mortality distribution:** The 1999 distribution is reused in every scenario. Later `study_year` values do not incorporate changes in actual mortality patterns or constitute mortality forecasts.
- **Handedness assumptions:** Rates are digitised, averaged across sexes without weighting and extrapolated beyond the observed birth-year range. The mapping treats the survey's age pattern as a birth-cohort pattern.
- **Uncertainty:** The notebook does not quantify sampling uncertainty, provide confidence intervals or test whether the apparent gap is statistically significant.

## Repository contents and scope

| File | Implemented analysis or documented claims |
| --- | --- |
| [handedness_mortallity_analysis.ipynb](handedness_mortallity_analysis.ipynb) | Implements the 1990 and 2018 scenarios, including the saved 5.5-year and 2.3-year outputs and the normalisation caveat above. Uses Matplotlib for its plots. |
| [Namrata_Muralidharan_Traineeship_Report_March24.pdf](Namrata_Muralidharan_Traineeship_Report_March24.pdf) | Documents the project background, methodology, results, limitations and references, plus a 1990–2024 scenario extension and use of Seaborn. |

**Notebook versus report:** The 1990–2024 extension is documented only in the PDF; its code is not implemented in the uploaded notebook. Running the notebook reproduces only the two coded scenarios and its Matplotlib plots, not the report's extended trend or Seaborn visualisations.

## Reproducibility

Clone the repository and enter its directory:

```bash
git clone https://github.com/nmrtm/handedness-mortality-analysis.git
cd handedness-mortality-analysis
```

In a Python environment, install the dependencies and open the notebook:

```bash
python -m pip install pandas numpy matplotlib notebook
jupyter notebook handedness_mortallity_analysis.ipynb
```

Restart the kernel and run all cells in order from the repository directory. The notebook displays four Matplotlib plots and saves PNG copies in the working directory.

- **Inputs:** The CSV and TSV files are fetched from the exact GitHub Gist URLs embedded in the notebook. They are not included in this repository, so a rerun requires internet access and availability of those external files.
- **Environment:** Package versions are not pinned. Record the Python and package versions used for a rerun; the repository does not provide a locked environment.
- **Calculation:** To reproduce the saved 5.5-year and 2.3-year outputs, retain the current ages 6–114, full-table denominators and fixed 1999 mortality data. Reproducing those outputs does not resolve the normalisation mismatch or validate a mortality or life-expectancy claim. Any future calculation with consistent age support and normalisation should report its results separately from these saved outputs.
- **Scope:** The notebook does not contain the report's 1990–2024 extension, so that trend cannot be reproduced from the supplied code.

## Acknowledgements

The notebook follows the structure and analysis of Madeleine Bonsma-Fisher's DataCamp tutorial, *Do Left-handed People Really Die Young?* ([author's publication list](https://madeleinebonsma.com/wp-content/uploads/2025/06/CV_Madeleine.pdf)). The prepared input datasets are hosted by `mbonsma` on GitHub Gist. This repository contains the traineeship implementation and accompanying report.
