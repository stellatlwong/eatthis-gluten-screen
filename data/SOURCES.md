# Sources for the restaurant allergen guides

The restaurant items in `restaurant_items_clean.csv` were extracted from the official allergen guides below. The guides themselves are not stored in this repository because they belong to the restaurants.

| Restaurant | Guide used in the analysis | Link |
|---|---|---|
| Tatte Bakery & Café | Allergen & Nutritional Information, Fall 2026 | https://storyblok.pleinaircdn.com/f/320617/x/6e716e4bae/allergen-and-nutritional-guide-fall-v-091826-web.pdf |
| Panera Bread | Allergen Guide, Edition 1, valid for 4 November 2026 | https://www.panerabread.com/content/dam/panerabread/documents/c10-26-allergen-guide.pdf |
| Boudin Bakery | Master Allergen List, Q4 2026 | https://boudinbakery.com/wp-content/uploads/2026/09/Master-Allergen-List_Q4-2026-Cheesesteaks_082726.pdf |
| CAVA | Nutrition and Allergen Guide (restaurant menu) | https://assets.ctfassets.net/kugm9fp9ib18/Ki4gri5pvT2rtPcjBQPjR/c78bed9be8448e2ab700f440f6faa83e/KT6_26_AN_ANGUIDE_RETAIL_T_MKTAN11724.pdf |
| Pret A Manger (US) | Pret's Allergen Guide, US Pret Shops, November 2026 | https://assets.ctfassets.net/4zu8gvmtwqss/47tgYK3AwKWXVFHGn4XEyJ/1ef7a5c7b0ae36e6671ac5666ac8bba6/Allergen_Guide_November_2026.pdf |
| Così | Ingredient Information for USA Restaurants, stated as current to June 2026 | https://images.getbento.com/accounts/2b3fa09317b4f49d36c34188bda32d3f/media/OnXWb72fQJ6riSZSipqg_Allergens.pdf |
| Sweetgreen | Nutrition page for the Fenway restaurant, read on 11 November 2026 (40 items) | https://www.sweetgreen.com/nutrition/fenway |
| Au Bon Pain | Café Menu nutrition and allergen guide, **menu last updated 6 October 2017**. The newer Fall 2022 menu file (linked) gives nutrition only and no allergens, so it could not be used | https://www.aubonpain.com/Assets/au-bon-pain-nutritional-menu-fall-22.pdf |
| Chipotle (UK) | Allergens and ingredients statement. It lists ingredients only, so it contributes no dishes to the analysis | https://www.chipotle.com/allergens |

Notes

- The guides mark items that contain **wheat**. Items marked only "may contain" were not counted as containing wheat.
- The Au Bon Pain guide is several years old, so its menu has probably changed. It is the most recent Au Bon Pain file we found that lists allergens.
- Guides are revised often, so the current version on a restaurant's site may differ from the one used here.
- `restaurant_item_changes.csv` records every correction made to the extracted list: rows removed, names repaired, item types assigned and sizes merged.

## Recipe data

| Dataset | Kaggle identifier |
|---|---|
| Food Ingredients and Recipe Dataset with Images | `pes12017000148/food-ingredients-and-recipe-dataset-with-images` |
| RecipeNLG | `paultimothymooney/recipenlg` |
| Epicurious: Recipes with Rating and Nutrition | `hugodarwood/epirecipes` |
