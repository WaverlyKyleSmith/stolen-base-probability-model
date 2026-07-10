# Stolen Base Probability Model & Pregame Scouting Report
Predicts stolen base probability in the NECBL and auto-generates pregame scouting reports for nontechnical coaching staff

## Overview

This project models the probability of a successful stolen base attempt based on pitch characteristics, game situation, and catcher ability. It combines a statistical model built in R with a derived metric, backstop+ into a printable, game-ready scouting report. This report is then utilized in-game as a tool by coaches and players to decide when to send a runner against a specific pitcher/catcher matchup. It's automated nature allows for reports to be created mid-game to take advantage of catcher substitutions or relief pitching.

## Pipeline

1. **Probability Model** (`model/`) - a logistic regression built on ~300 stolen base attemptts from the 2025 NECBL season (via Trackman), using pitch velocity, movement (IVB, horizontal break), zone time, catcher metrics, pitcher handedness, and count. Mean Absolute Error of ~3.5 percentage points. [Full writeup with tables and plots →](model/StolenBase.pdf)

2. **Backstop+** (`backstop-plus/`) - a catcher defensive metric scaled like OPS+/wRC+ (100 = league average), combining pop time, exchange time, and throw speed into a single number for comparing catchers' ability to control the running game.

3. **Pregame Report Generator** (`report-generator/`) - a reproducible Python notebook that combines the model and Backstop+ into an automated PDF report for a specific opponent, catcher, and pitcher, color-coded by steal probability for in-game decision-making. It includes base steal probability per pitch-type by count as well as a general note on when the best/worst opportunity to attempt to steal may be by weighing those probabilities by the opposing pitcher's pitch mix.

## Key Findings (Model)

- Pitch movement matters most

- Catcher throw speed and catch height both suppress success rate

- Count matters, but not evenly

## Sample Output

A generated pregame report for a sample opponent:
[`sample-output/SB_Report_SampleOpponent_2026-07-04.pdf`](sample-output/SB_Report_SampleOpponent_2026-05-02.pdf)

The report flags the best and worst windows to run in a single glance - e.g. in anticipation of a breaking ball on a 0-0 count is the green light, anticipation of an off-speed pitch on a full count, not so much.

## Limitations

- Small sample size (~300 attempts); findings are reported at the 75% confidence level and should be read directionally
- Runner-side factors (jump, sprint speed) aren't tagged in Trackman and therefore, aren't included in the model
- Movement data (IVB/horizontal break) isn't always available pregame for every pitcher, so the scouting report falls back to pitch-category baselines when it's missing
- Trackman data is user-entered, so mistagged pitches and missing data may be present beyond cleaning

## Tech Stack

R (tidyverse, dplyr, readr, knitr, caret, randomForest, car, pscl, ResourceSelection, rms) <br>
Python (pandas, numpy, reportlab) <br>
Trackman data

## Repo Structure

- `model/` - statistical backbone model (R Markdown source + knitted PDF)
- `backstop-plus/` - catcher metric rerivation (Python Notebook)
- `report-generator/` - combines both into the final report
- `sample-output/` - example generated scouting report
