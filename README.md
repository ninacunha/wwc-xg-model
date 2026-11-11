---
author: "Nina Cunha"
format: pdf
---
$$\text{STAT 4255 Project}$$
$$\text{Nina Cunha}$$

###### **Description**
The Women's World cup is occurring soon in Brazil in 2027. After the USWNT won in 2019, the team was eliminated in the round of 16 in 2023, its earliest exit ever, which made me wonder about the quality of the chances it created. Unlike in many other sports, soccer scores are low and can misrepresent how well a team played. Expected Goals (xG) is a very common metric used in performance analysis where it evaluates the probability of a ball going in the net based on various factors. I aim to see how accurately a model I build can predict whether a shot is a goal, as well as identifying which factors matter most. I also will evaluate my xG against StatsBomb's xG. I will calculate logloss and the AUC score as the evaluation metrics.

###### **Data Set**
I am getting my data from statsbomb (github.com/statsbomb/open-data). Statsbomb includes match and event level data for leagues across the world. All data collected is from real games in the respective competition. For this project I am focusing on the last two Womens World Cups (2019 & 2023) which included about 3000 shots that I am able to use for my models, and 8 of the main factors that I determined influences a shot. __Terms of data use__: *"If you publish, share or distribute any research, analysis or insights based on this data, please state the data source as StatsBomb and use our logo, available in our Media Pack."*

```python
import statsbombpy as sb
matches_2023 = sb.matches(competition_id=72, season_id=107)  # Women's World Cup 2023
matches_2019 = sb.matches(competition_id=72, season_id=30)   #
```
```{=latex}
\begingroup\scriptsize
```
| Variable | Description |
|----------|-------------|
| `goal` | **Response:** 1 if the shot scored, 0 otherwise |
| `x`, `y` | Shot location; the center of the goal is at (120, 40) |
| `distance` | Euclidean distance from the shot to the center of the goal |
| `header`, `under_pressure`, `shot_one_on_one`, `shot_follows_dribble`, `shot_first_time` | Binary (1/0) indicators: headed shot; shooter under pressure; one-on-one with the keeper; shot after a dribble; first-touch shot |
| `statsbomb_xg` | StatsBomb's xG, used only as a benchmark, not as a predictor |
```{=latex}
\endgroup
```
This is a supervised classification problem, the model outputs the probability that a shot is a goal, evaluated against the actual outcomes.

Some things to be aware of is that the dataset is very imbalanced, with most of the shots recorded not resulting in a goal. And these shots may be correlated, which may affect accuracy of models and will need to be addressed in data processing.