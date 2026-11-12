# EatThis: recommending a restaurant when someone avoids gluten

**MIT 15.071 The AI Edge, Project 1.** Stella Wong and Nick Whalen.

## Scope and question

EatThis recommends restaurants for a group that includes someone avoiding gluten. Where published allergen information exists, it is used. Where it does not, a model ranks restaurants that may offer more options, with dietary suitability labelled as unverified and restaurant confirmation required. The project evaluates gluten avoidance only.

**Can AI help EatThis choose a restaurant with a higher share of dishes likely to be free of gluten, and which approach should it use?** Dish predictions are inputs to the restaurant ranking. We compare dish names, photos and both on recipes, then test the text approaches on restaurant menus against the restaurants' own published wheat labels.

## Run it

Open **`eatthis_gluten_project1.ipynb`**. It holds all the code and is saved with its results showing. To run it: upload to Google Colab, choose an L4 GPU, Run all, and paste a free Kaggle API key at Step 2 (about an hour). Numbers vary slightly between runs.

## Data

About 13,500 Epicurious recipes with photos, plus 493,165 RecipeNLG recipes for training, downloaded from Kaggle by the notebook. Gluten labels are derived from each ingredient list. The restaurant test uses 770 dishes from eight chains' allergen guides (`data/SOURCES.md`). No raw data is stored here.

## Approaches

- **Keyword rule** (benchmark): flags a dish whose name contains an obvious gluten word.
- **Simple text model** (traditional machine learning): TF-IDF plus logistic regression on the dish name.
- **Fine-tuned language model** (deep learning): XLM-RoBERTa trained further on our recipes.
- **Combined model:** the average of the two models above.
- **Photo models** (deep learning): MobileNetV2, as it comes and fine-tuned, and a text-and-photo blend.

## Findings

- **Restaurant selection, the primary task.** Across 56 shortlists of three chains, the restaurant selected by each learned text model had on average **42%** of its dishes not marked wheat, against 39% for the keyword rule, 27% at random and 43% at best. The 42% is that average share. It is not accuracy, safety or proof of suitability.
- **Name, photo or both (recipes, ranking score out of 100):** photo alone 75, dish name alone 91, both 92.
- **Restaurant dishes (602 marked wheat, 168 not).** Wheat dishes flagged: keyword rule 201, simple 569, fine-tuned 558, combined 563. False alarms: 4, 75, 66, 69.
- **Recommending individual dishes.** Of 24 dishes each model would have recommended, the number containing wheat was 5 for the simple model (20.8%), 5 for the fine-tuned model (20.8%) and 3 for the combined model (12.5%).

## Recommendation

For the restaurant-discovery feature we recommend **TF-IDF plus logistic regression**. All learned text models achieved the same restaurant-selection outcome, and the simple model flagged the most wheat dishes and needs no GPU. We do **not** recommend individual dishes for gluten avoidance from predictions alone: the error rates above are too high. That does not affect the restaurant-ranking result, which rests on the average over a whole menu.

## Limitations

Exploratory evidence from eight chains, mostly bakery cafés, not a validated production system. Labels come from a word list. Guides mark wheat, not gluten, so the absence of a wheat label is not verification that a dish is gluten-free. The restaurant list was extracted and cleaned by us (`data/restaurant_item_changes.csv`) without independent review. Photos were tested on recipes only. Published allergen information and the restaurant's confirmation always come first.
