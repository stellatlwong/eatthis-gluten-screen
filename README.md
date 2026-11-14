# EatThis: choosing a restaurant when someone in the group avoids gluten

**MIT 15.071 The AI Edge, Project 1.** Stella Wong and Nick Whalen.

## The problem

EatThis helps a group of friends choose where to eat. This project looks at one case: someone in the group avoids gluten. Where a restaurant publishes allergen information, EatThis uses it. Many publish none, and then all EatThis has is the menu.

**Given three restaurants and only the dish names on their menus, which menu appears to offer the most options for someone avoiding gluten, and which model should EatThis use to answer that?** A model gives each dish name a risk score. EatThis averages the scores across a menu and ranks first the restaurant with the lowest average.

## Run it

**`eatthis_gluten_project1.ipynb`** holds all the code, saved with its results. To run it: upload to Google Colab, choose an L4 GPU, Run all, and paste a free Kaggle API key at Step 2 (about 75 minutes). Deep learning numbers vary slightly between runs.

## Data

About 13,500 Epicurious recipes with photos, plus 493,905 RecipeNLG recipes for training only, downloaded by the notebook: 503,363 recipes to train, 2,021 to choose settings and 2,017 to test. Gluten labels are derived from each recipe's ingredient list, and the models learn from the recipe names. The restaurant test uses 770 dishes from eight chains' allergen guides (`data/SOURCES.md`). No raw data is stored here.

## One ladder, one question per step

| Step | Question | Result (ranking score out of 100, test recipes) |
|---|---|---|
| Keyword rule | How far do obvious words get us? | Catches 34% of gluten recipes |
| Traditional ML: TF-IDF plus logistic regression | Predict gluten directly, or nine ingredient groups first? | 90.4. Two steps chosen because it gives a possible ingredient source. One step scored slightly higher on validation (92.2 against 91.5) |
| Deep learning as it comes: frozen XLM-RoBERTa | What does a pretrained language model already know? | 74.4 |
| Deep learning fine-tuned: the same XLM-RoBERTa | Does teaching it recipes help? | 91.6 |
| Combined: average of traditional ML and fine-tuned | Do they help each other? | 91.9 |
| Photos: MobileNetV2, frozen and fine-tuned | What does a photo add to the name? | Photo alone 75.7. Name plus photo 91.2 against 90.4 for the name |

The frozen and fine-tuned language models also differ in how the network's output becomes a prediction, so their gap is not due to fine-tuning alone.

## The restaurant test

- **Choosing a restaurant, the main task.** Across all 56 shortlists of three chains, traditional ML, the fine-tuned language model and the combined model picked the same restaurant every time. That restaurant had on average **41.7%** of its dishes with no wheat listed, against 39.2% for the keyword rule, 38.3% for the frozen language model, 27.1% at random and 42.9% at best. This is an average share, not accuracy or proof of suitability.
- **Dish by dish (602 marked wheat, 168 not).** Wheat dishes flagged: keyword rule 201, traditional ML 579, frozen 567, fine-tuned 551, combined 563. Flagged in error: 4, 82, 134, 72, 66.
- **Suggesting individual dishes.** Of the dishes each model would have suggested, the guides mark as containing wheat 4 of 23 for traditional ML (17.4%), 4 of 24 for the fine-tuned model (16.7%) and 3 of 24 for the combined model (12.5%).

## Recommendation and impact

We recommend **TF-IDF plus logistic regression** for restaurant discovery. It picked the same restaurants as the two more complex models, flagged the most wheat dishes and trains in minutes without a GPU. Against choosing at random, the diner sees about half as many more dishes with no wheat listed (27% to 42%), which is 15 of the 16 points available with the guides in hand. We do **not** recommend suggesting individual dishes for gluten avoidance from predictions alone. Running cost and speed in a live product were not measured.

## Limitations

Exploratory evidence from eight chains, mostly bakery cafés. Labels come from a word list. Guides mark wheat, not gluten, so no wheat label does not mean gluten-free. We extracted and cleaned the restaurant list ourselves (`data/restaurant_item_changes.csv`). Menu descriptions were not tested. Published allergen information and the restaurant's confirmation always come first.
