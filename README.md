# Regulating water use with estimates instead of measurements

Code and data that reproduce every number, table and figure in "Regulating water use with estimates instead of measurements can have unintended costs" by Taro Mieno, Timothy Foster and Nicholas Brozović, including its Supplementary Information.

## Contents

| File | What it does | What it writes |
|---|---|---|
| `Code/0_functions.qmd` | Prices, production functions and simulation functions, loaded by the other files. Rendering it runs checks of the functions. | Nothing |
| `Code/1_simulation.qmd` | Simulates three populations of 1,000 farmers under a quota on water use and a quota on estimated water use, with and without trading. | `Results/` |
| `Code/2_calculations.qmd` | Numbers quoted in the text, Supplementary Tables 1 to 3, Supplementary Notes 1, 2 and 4, and the two-farmer examples. | `Results/` |
| `Code/3_figures.qmd` | Figures 1 to 6 and Supplementary Figs. 1 to 5. | `Figures/` |
| `Code/4_well_records.qmd` | Downloads the Nebraska well and power records and the well registry, and builds the tables that step 5 pairs. | `Data/raw/`, `Results/` |
| `Code/5_energy_estimates.qmd` | Pairs wells with electricity records (only if step 4 has been run) and compares an electricity-based estimate with metered pumping (Supplementary Note 3, Supplementary Fig. 6). | `Results/`, `Figures/` |
| `Code/6_error_scaling.qmd` | Downloads public data that pair satellite estimates with metered or reported use, and measures how the error grows with water use (Supplementary Note 5, Supplementary Table 5). | `Results/` |
| `Data/energy_water_deidentified.csv` | The paired Nebraska records with well identifiers and locations removed: an arbitrary well number, district, year, irrigated acres, metered pumping (acre-feet) and electricity use (kWh). | |

## How to run

Open `regulating-water-use-with-estimates.Rproj`, or work from the folder that holds it, and render the files in order:

```
quarto render Code/0_functions.qmd
quarto render Code/1_simulation.qmd
quarto render Code/2_calculations.qmd
quarto render Code/3_figures.qmd
quarto render Code/4_well_records.qmd
quarto render Code/5_energy_estimates.qmd
quarto render Code/6_error_scaling.qmd
```

Each rendered file (`Code/*.html`) shows the numbers it computes. Step 1 takes about 20 minutes on a computer with 16 cores and 128 GB of memory; the other steps take a few minutes each, plus the downloads (about 360 MB, which take about 520 MB once unzipped).

Steps 4 to 6 (Supplementary Notes 3 and 5) do not need steps 1 to 3, so they can be checked without the simulation. Steps 5 and 6 can also be run without step 4, from the shared file `Data/energy_water_deidentified.csv`. The pairing of wells with electricity records, and the numbers that need well depth and electrical load, are then skipped, because they need the tables that step 4 builds. Those tables contain well locations and are not shared. When step 4 has been run, step 5 writes a rebuilt copy of the shared file to `Results/` and reports whether it is identical; it can differ if the well registry has changed since 12 November 2026.

## Data sources

+ Republican River Compact Administration, Nebraska data archives, 2007 and 2009 to 2012: `https://www.republicanrivercompact.org/YYYY/html/zip/neYY.zip`.
+ Nebraska Department of Water, Energy, and Environment, registry of groundwater wells: `https://gis.ne.gov/agency3/rest/services/Groundwater_Wells_DWEE/FeatureServer/0`. The registry is updated over time. The paper uses a snapshot taken on 12 November 2026, so a later download can give slightly different numbers in Supplementary Note 3.
+ Zipper, S. et al. (2024), data and code on HydroShare, `https://doi.org/10.4211/hs.36fa9e69ff0849bb941b9ab2835b8a6e` (CC BY-SA 4.0).
+ Ott, T. et al. (2024), data and code on GitHub, `https://github.com/montimaj/OpenET-GW`, commit `edc587c` (CC0 1.0).

## Software

R 4.5.0 and Quarto 1.7.32, with the R packages data.table 1.17.8, dplyr 1.1.4, tidyr 1.3.1, stringr 1.5.1, ggplot2 4.0.3, here 1.0.2, knitr 1.50, rmarkdown 2.29, curl 6.4.0, jsonlite 2.0.0, readxl 1.4.5 and foreign 0.8.90. The simulation runs in parallel with `parallel::mclapply()` on macOS and Linux, and on one core on Windows. The number of cores does not change the results.

## License

The code is released under the MIT license (`LICENSE`).
