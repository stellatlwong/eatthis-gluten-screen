# EatThis: recommending a restaurant when someone avoids gluten

**MIT 15.071 The AI Edge, Project 1.** Stella Wong and Nick Whalen.

## The question

EatThis helps a group choose a restaurant while taking dietary restrictions into account. We start with gluten. **Can AI help EatThis choose a restaurant with a higher share of dishes likely to be free of gluten, and which approach should it use?** We compare dish names, photos and both on recipes, then test the text approaches on restaurant menus against the restaurants' own published wheat labels.

## Run it

Open **`eatthis_gluten_project1.ipynb`**. It holds all the code and is saved with its results showing. To run it: upload to Google Colab, choose an L4 GPU, Run all, and paste a free Kaggle API key at Step 2 (about an hour). Numbers vary slightly between runs.

## Data

About 13,500 Epicurious recipes with photos, plus 493,165 RecipeNLG recipes for training, all downloaded from Kaggle by the notebook. Recipes carry no gluten label, so we derive one from each ingredient list. The restaurant test uses 770 dishes from eight chains' allergen guides (`data/SOURCES.md`). No raw data is stored here.

## Approaches

- **Keyword rule** (benchmark): flags a dish whose name contains an obvious gluten word.
- **Simple text model** (traditional machine learning): TF-IDF plus logistic regression on the dish name.
- **Fine-tuned language model** (deep learning): XLM-RoBERTa trained further on our recipes.
- **Combined model:** the average of the two models above.
- **Photo models** (deep learning): MobileNetV2, as it comes and fine-tuned, and a text-and-photo blend.

## Findings

Ranking score: how well a model puts dishes with gluten above dishes without (50 random, 100 perfect).

- **Name, photo or both (recipes):** photo alone 75, dish name alone 91, both 92.
- **Restaurant dishes (602 marked wheat, 168 not):**

| | Keyword rule | Simple | Fine-tuned | Combined |
|---|---|---|---|---|
| Wheat dishes flagged, of 602 | 201 | 569 | 558 | 563 |
| False alarms, of 168 | 4 | 75 | 66 | 69 |
| Dishes shown, not marked wheat | 34% expected | 19 of 24 | 19 of 24 | 21 of 24 |

- **Choosing a restaurant:** the pick has on average 42% of dishes not marked wheat with any of the three models, against 27% at random, 39% with the keyword rule and 43% at best. Keywords deliver most of the gain.

## Recommendation

For EatThis's first implementation we recommend the **simple text model**. All learned text approaches achieved the same restaurant-selection outcome, and the simple model flagged the most wheat dishes and needs no GPU. The combined model's 21 of 24 against 19 of 24 is promising but does not yet justify maintaining two models.

## Limitations

Exploratory results on eight chains, mostly bakery cafés. Labels come from a word list, and guides mark wheat, not gluten. The restaurant list was extracted and cleaned by us (`data/restaurant_item_changes.csv`) without independent review. Photos were tested on recipes only. Nothing here establishes that a dish is gluten-free or safe: published allergen information and the restaurant's confirmation come first.
