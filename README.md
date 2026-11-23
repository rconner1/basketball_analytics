# Basketball Analytics: Predicting Point Differential × Steals for 7 NBA Games
 
Forecasting the total point differential × total steals across 7 NBA games from team box-score
statistics. This started as a prediction competition in my Methods of Data Analysis class at UNC
Chapel Hill (December 2025) and was rebuilt so the model only uses information that is known before a
game is played.
 
**Main finding:** My original class model used point differential, steals and the final scores as
predictors. Those are the pieces the response is built from, so it looked very accurate (test RMSE
44.45) but couldn't truly forecast a game. After removing them, stepwise regression on box-score stats
was the best usable model, improving on the baseline by about 9% (RMSE 107.27 vs. 118.04).
 
## Data
 
Two Kaggle datasets, filtered to the 14 teams in the 7 games and to games from 2025 on, then joined by
game ID with one row per game (400 games).
 
- Response: `y` = point differential × steals for each game
- Predictors: box-score stats only (field goals, threes and free throws made, attempted and percentage;
  offensive, defensive and total rebounds; assists; turnovers; blocks; fouls)
- Excluded because they contain part of the response: point differential, steals, both final scores,
  plus/minus
## Approach
 
- 70/30 train/test split with a fixed seed
- Five models: empty baseline, the original class model (for reference), stepwise regression,
  stepwise with a square-root transformation of `y`, and a random forest
- Models compared on test-set RMSE
- Forecast made with the best usable model, using each home team's average box-score stats
## Results (test set)
 
| Model | Usable for forecasting | Test RMSE |
| --- | --- | --- |
| Original class model (uses parts of the response) | No | 44.45 |
| Stepwise regression | Yes | 107.27 |
| Random forest | Yes | 108.21 |
| Stepwise, square root of `y` | Yes | 110.03 |
| Empty (training average) | Yes | 118.04 |
 
Stepwise selection chose field goals made and attempted, threes made, three-point percentage, free throw
percentage, defensive rebounds and blocks. In the random forest, field goal percentage and field goals
made were the most important predictors, followed by assists and rebounds.
 
**Final forecast:** 5,636 (sanity check from season averages: 5,828).
 
## Limitations
 
- The model is trained on same-game stats, but the forecast uses season averages, which smooth out the
  swings that drive large values of `y`.
- Each game is seen from one team's side after removing duplicates.
- The 7-game total is approximated from per-game predictions.
- Results come from a single train/test split; cross-validation would give a more reliable comparison.
- Next step: predict point differential and steals separately, then combine them.
## Files
 
| File | Description |
| --- | --- |
| `Basketball_Analytics_Code.Rmd` | Full analysis: cleaning, models, evaluation, forecast |
| `Basketball_Analytics_Paper.pdf` | Written report of the methods and results |
 
## How to run
 
Download the two datasets below, put `TeamStatistics.csv` and `team_traditional.csv` in the same folder
as the `.Rmd`, then knit it in RStudio. Requires the R packages `tidyverse`, `randomForest` and `knitr`:
 
```r
install.packages(c("tidyverse", "randomForest", "knitr"))
```
 
## Data sources
 
- Moore, E. A. (2025). *NBA dataset: Box scores & stats, 1947–today.* Kaggle.
  https://www.kaggle.com/datasets/eoinamoore/historical-nba-data-and-player-box-scores
- Jóźwiak, S. (2025). *NBA traditional boxscores 1997–2025.* Kaggle.
  https://www.kaggle.com/datasets/szymonjwiak/nba-traditional
 
