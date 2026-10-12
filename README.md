# Nutri-Score Classification of Beverages

Predicting the Nutri-Score grade (A–E) of beverages from the nutrition facts on their label, comparing a Decision Tree and a Random Forest.

**Tools:** Python (pandas, scikit-learn), Google Colab

**Data:** 449 beverages scraped from [Open Food Facts](https://world.openfoodfacts.org/). Each product has its Nutri-Score grade and 9 nutrients per 100 ml: energy, fat, saturated fat, carbohydrates, sugars, fiber, protein, salt, and fruit/vegetable/legume content (%). Grade C is the most common class and A the least common. The dataset is included in `data/` (Open Food Facts data is available under the ODbL licence).

**Approach:** Cleaned symbols and units from scraped values (energy kept in kJ) → filled 7 missing fruit/vegetable values with 0 → treated values above fixed cut-offs as missing and re-imputed them with `IterativeImputer` (linear regression) → standardised features → 70/30 train–test split → trained both models with `max_depth = 3`.

## Result
| Model | Accuracy | Precision | Recall | F1 |
|---|---|---|---|---|
| Random Forest | **65.2%** | **0.619** | **0.637** | **0.611** |
| Decision Tree | 51.9% | 0.507 | 0.532 | 0.500 |

Test set: 135 beverages. Precision, recall, and F1 are macro-averaged over the five grades. Random Forest outperformed the single Decision Tree.

**Note on the report:** The report states about 96% accuracy for both models. That number came from a bug: the outlier step used `>=` instead of `<=`, so it kept only the outliers and replaced almost every normal value with a missing value. The imputer then rebuilt those values from the other columns, including the Nutri-Score label, so the features effectively contained the answer. The notebook in this repo uses the corrected comparison, and the table above shows its results.

**Known limitations:**
- Nutri-Score is calculated from these same nutrients with a fixed formula, so the models are re-learning a known rule rather than solving an open problem.
- `max_depth` (3, 5, 7, 9) was compared on the test set, with no separate validation set. Depth 9 scored best there (72.6% for Random Forest), but the final models used depth 3.
- The imputer was fitted on the full dataset, including the target column, before the train–test split, so some information from the labels still leaks into the features.
- The scraped data includes some powdered products (e.g. cocoa drink mixes), whose values are per 100 g rather than per 100 ml.

## Report
- [Report (PDF)](docs/paper.pdf)
- [Slides (PDF)](docs/slides.pdf)

---
Course project — Data Mining. Team of 5. 
**My contribution:**
- Proposed the project idea and found the data source.
- Worked on the analysis notebook.
- Co-wrote the report and slides.