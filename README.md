# Nutri-Score Classification of Beverages

Predicting the Nutri-Score grade (A–E) of beverages from the nutrition facts on their label, comparing a Decision Tree and a Random Forest.

**Tools:** Python (pandas, scikit-learn), Google Colab

**Data:** 449 beverages scraped from [Open Food Facts](https://world.openfoodfacts.org/) [TODO: confirm source]. Each product has its Nutri-Score grade and 9 nutrients per 100 ml: energy, fat, saturated fat, carbohydrates, sugars, fiber, protein, salt, and fruit/vegetable/legume content (%). Grade C is the most common class and A the least common. The dataset is included in `data/` (Open Food Facts data is available under the ODbL licence).

**Approach:** Cleaned symbols and units from scraped values → filled 7 missing fruit/vegetable values with 0 → flagged outliers and re-imputed them with `IterativeImputer` → standardised features → 70/30 train–test split → tuned `max_depth` (3, 5, 7, 9) and trained both models with `max_depth = 3`.

## Result
| Model | Accuracy | Precision | Recall | F1 |
|---|---|---|---|---|
| Random Forest | **96.3%** | **0.959** | **0.962** | **0.960** |
| Decision Tree | 95.6% | 0.952 | 0.960 | 0.955 |

Test set: 135 beverages. Precision, recall, and F1 are macro-averaged over the five grades.

**Note:** Nutri-Score is calculated from these same nutrients with a fixed formula, so the models are re-learning a known rule. High accuracy is expected here and does not mean the problem is hard. [TODO: verify in notebook] `max_depth` was chosen on the test set without a separate validation set, so the scores may be slightly optimistic.

## Report
- [Report (PDF, Indonesian)](docs/report.pdf)
- [Slides (PDF)](docs/slides.pdf)

---
Course project — Data Mining I, Semester 4 (2024). Team of 5. Data scraping was done by a teammate (scraping code not included).

**My contribution:** 
- Proposed the project idea and found the data source.
- Worked on the analysis notebook.
- Co-wrote the report and slides.