# Handedness and Mortality

Exploring how changes in reported left-handedness across birth cohorts can produce apparent differences in age at death.

This Python project applies Bayes' theorem to historical handedness rates and US mortality data. It examines whether an apparent age-at-death gap can emerge from changing patterns of reported handedness, using the same underlying death distribution for both groups.

[View the notebook](handedness_mortallity_analysis.ipynb) · [Read the project report](Namrata_Muralidharan_Traineeship_Report_March24.pdf)

## Background

The project revisits the historical claim that left-handed people die around nine years earlier than right-handed people. An alternative explanation concerns birth cohorts: older generations were less likely to report being left-handed, partly because of social pressure to use the right hand. This can change the age composition of the two reported handedness groups in a sample of deaths.

The question is whether this change in reporting alone can generate an apparent age gap.

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
4. Apply Bayes' theorem to calculate age-at-death distributions conditional on reported handedness.
5. Compare the notebook's age-weighted summaries for the 1990 and 2018 scenarios.

Changing `study_year` changes the mapping between age and birth year. **Both scenarios retain the same 1999 mortality data.**

**Tools:** Python, pandas, NumPy, Matplotlib and Jupyter Notebook.

## Notebook outputs

The current notebook prints the following differences between its right-handed and left-handed age summaries:

| Study-year scenario | Printed age gap |
| --- | ---: |
| 1990 | 5.5 years |
| 2018 | 2.3 years |

These calculations illustrate how the apparent gap can shrink as the birth cohorts represented at each age change. They are model-based outputs, not observed mortality differences for those years or estimates of individual life expectancy.

**Calculation caveat:** The notebook sums age-weighted probabilities over ages 6–114, while its probability denominators use the full cleaned mortality table. It does not renormalise the probabilities over the restricted range. The values above document the current implementation; the age range and normalisation should be made consistent before interpreting its printed averages as distribution means.

## Repository contents

| File | Description |
| --- | --- |
| [handedness_mortallity_analysis.ipynb](handedness_mortallity_analysis.ipynb) | Analysis code, explanations, plots and saved outputs for the 1990 and 2018 scenarios |
| [Namrata_Muralidharan_Traineeship_Report_March24.pdf](Namrata_Muralidharan_Traineeship_Report_March24.pdf) | Project background, methodology, results, limitations and references; also includes a scenario trend from 1990 to 2024 |

The report's 1990–2024 extension is documented in the PDF but is not implemented in the uploaded notebook. The report also uses Seaborn for visualisations; the uploaded notebook uses Matplotlib.

## Running the notebook

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

Run the cells in order. The notebook displays four plots and saves PNG copies in the working directory. Package versions are not pinned in this repository.

## Interpretation and limitations

This is an educational demonstration of conditional probability and birth-cohort effects. It shows how different age profiles can arise when reported handedness varies across generations.

- The analysis combines aggregate data from different years; it does not follow individuals over time or estimate survival hazards.
- Handedness rates are digitised, averaged across sexes and extrapolated beyond the observed range.
- The mortality distribution is held fixed across scenarios, so the later-year calculations do not capture changes in actual mortality patterns.
- The notebook does not quantify sampling uncertainty or test whether the apparent gap is statistically significant.

## Acknowledgements

The notebook follows the structure and analysis of Madeleine Bonsma-Fisher's DataCamp tutorial, *Do Left-handed People Really Die Young?* ([author's publication list](https://madeleinebonsma.com/wp-content/uploads/2025/06/CV_Madeleine.pdf)). The prepared input datasets are hosted by `mbonsma` on GitHub Gist. This repository contains the traineeship implementation and accompanying report.
