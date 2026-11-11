# EatThis gluten screen

**MIT 15.071 The AI Edge, Project 1 (predictive AI).** Team: Stella Wong and Nick Whalen.

## The task

EatThis is our app for helping a group choose a restaurant. When one person avoids gluten, the group has little to go on, because most restaurants publish no allergen information. We asked whether a model that reads only the menu can point that diner to a few dishes worth asking the restaurant about, and whether deep learning does this better than a simple model by enough to justify its complexity.

## Start here

Open **`eatthis_gluten_project1_final.ipynb`**. It is saved with all results showing, and every number below comes from that run.

To run it yourself: upload it to Google Colab, choose an L4 GPU runtime, select Run all, and paste a Kaggle API key into the hidden box at Step 2 (free: kaggle.com, Settings, API, Create New Token). It takes roughly 45 minutes. Package versions are printed in Step 1.

## Data

| Data | Use | Source |
|---|---|---|
| About 13,500 Epicurious recipes | Training, validation and test | Kaggle, *Food Ingredients and Recipe Dataset with Images* |
| 500,000 RecipeNLG recipes | Training only | Kaggle, *RecipeNLG* (research and education use) |
| About 20,000 Epicurious recipes with editors' tags | Check on our labels | Kaggle, *Epicurious: Recipes with Rating and Nutrition* |
| 745 dishes from eight restaurant chains | External test | Official allergen guides, listed in `data/SOURCES.md` |

The notebook downloads the recipe data when it runs. No raw recipe data or guide is stored here. Recipes carry no gluten label, so we derive one from each ingredient list with a word list of nine gluten ingredient groups.

## Method

Two models read the dish name alone: a simple text model (word counts with logistic regression) and XLM-RoBERTa, a pretrained language model that we fine-tuned on the same recipes. Each flags a dish when its risk score passes a cut-off set on validation recipes to catch 95% of gluten dishes. Before running the fine-tuned model we wrote down the conditions under which it would replace the simple one.

## Main findings

| | Keyword rule | Simple model | Fine-tuned model |
|---|---|---|---|
| Test recipes: missed gluten dishes per 1,000 | 297 | 16 | 14 |
| Test recipes: false alarms per 1,000 | 9 | 233 | 226 |
| Restaurants: wheat dishes flagged (of 586) | 192 | 566 | 551 |
| Restaurants: false alarms (of 159) | 4 | 85 | 78 |
| Suggested dishes not marked as containing wheat | about 31% expected | 17 of 21 | 18 of 23 |

- Suggesting up to three low-risk dishes per restaurant, the simple model's suggestions were not marked as containing wheat in 17 of 21 cases. Picking the same number at random from the dishes a keyword rule leaves unflagged would be expected to give about 31%.
- Fine-tuning improved results on recipes, but this did not translate into better results on restaurant menus. The two models were comparable there, so we keep the simpler one.
- For ranking restaurants, the models (46%) were barely ahead of the keyword rule (44%). We treat this as a secondary result.

**Recommendation:** a limited pilot of "dishes worth asking about" in EatThis, always worded as a prompt to ask the restaurant and never as a safe or gluten-free label.

## Limitations

- These are exploratory results. Design choices were made while looking at test recipes and restaurant dishes.
- Labels are imperfect. Recipe labels come from a word list (95.8% agreement with editors' gluten-free tags, which checks one side only). The guides mark wheat, not gluten.
- The restaurant list was extracted and cleaned by us. Every change is logged in `data/restaurant_item_changes.csv`, but it was not independently reviewed.
- The sample is eight chains, mostly bakery cafés, and some guides are older editions (see `data/SOURCES.md`). Results were weak at Tatte, and both models miss unfamiliar dish names such as künefe and brik.
- Several suggestions are obvious (a fruit cup). We have shown that the short list is enriched, not that it helps a diner choose a meal.

## Other contents

`results/` holds the tables saved by the final run. `experiments/` holds earlier exploratory work with dish photos and sentence embeddings. Its restaurant results use an uncorrected item list and are superseded.
