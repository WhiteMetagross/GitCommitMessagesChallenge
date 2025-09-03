## Evaluation Report: Developer Role Classification.

This report summarizes the final evaluation for the developer role classification project. The objective was to build a model that could accurately predict a developer's role (`frontend`, `backend`, `fullstack`, or `qa`) from their commit data. I built two models for this task: a `logisticRegression` baseline using TFIDF and an improved `xGBoost` model using Word2Vec embeddings.

---

### Final Evaluation:

I evaluated both models on a test set. The primary metric for this project was the **Macro F1 score**, chosen because it effectively handles the class imbalance I discovered during the exploratory data analysis.

The improved model, which combined `xGBoost` with `word2Vec`, showed a clear performance improvement over the baseline, validating the decision to use a more complex approach for representing the commit message text.

**Final Model Performance:**
| Model | Macro F1 Score |
| :--- | :---: |
| Baseline (Logistic Regression and TFIDF) | 0.9764 |
| **Improved (XGBoost and Word2Vec)** | **0.9813** |

---

### Best Model Performance (Per Class):

My final model, the `xGBoostClassifier`, performed well across all roles. It was nearly perfect at identifying `qa` commits and showed very high precision and recall for the other roles.

**XGBoost Model: Per Class Performance:**
| Role | Precision | Recall | F1-Score |
| :--- | :---: | :---: | :---: |
| backend | 0.99 | 1.00 | 0.99 |
| frontend | 0.97 | 0.99 | 0.98 |
| fullstack | 0.98 | 0.94 | 0.96 |
| qa | 1.00 | 0.98 | 0.99 |

---

### Major Failure Modes:

While the model was highly accurate, it wasn't perfect. Analyzing the few mistakes it made reveals limitations inherent in the data itself rather than significant flaws in the model.

* **The "Fullstack Ambiguity"**: The most common errors occurred when the model confused `fullstack` developers with either `frontend` or `backend` developers. This is an expected failure mode. When a `fullstack` developer makes a commit that only involves changing CSS files, the data signature (file extensions, keywords like "UI" or "style") is indistinguishable from that of a pure `frontend` developer. The model correctly classifies the *type of work* but cannot infer the developer's broader, official role from a single commit's data.

* **Role Overlap**: The model sometimes misclassified specialized roles as `fullstack`. This happens because roles in software are not perfectly isolated, a backend developer might occasionally change an HTML template. These commits are inherently ambiguous and represent the natural overlap in a real-world software project.

It's important to note that the model was **very good** with the `qa` role. The features for these commits, like `test_js` file extensions and keywords like "dataset" and "training," created a unique and easily identifiable pattern.

---

### Lessons Learned:

This project provided several key insights that will inform my future work.

1.  **Feature Engineering is Crucial**: The high performance of even the baseline model was a direct result of careful feature engineering. Simple, domain-aware features like creating binary flags for file extensions and calculating the `netCodeChange`, provided the models with powerful, direct signals. This reinforced that a deep understanding of the data is more valuable than complex model architecture alone.

2.  **Semantic Text Representation Adds Value**: The performance jump from the baseline to the improved model demonstrates the benefit of using embeddings that capture semantic meaning. My decision to train a custom `word2Vec` model allowed the classifier to understand the context of domain specific words (like that "modal" and "dropdown" are related UI concepts), giving it an edge over the keyword based TFIDF approach.

3.  **Proactively Address Class Imbalance**: Identifying the underrepresentation of the `qa` role during the EDA was critical. Using `stratify` during the data split and focusing on the **Macro F1 score** ensured I built and evaluated a model that performed well across *all* classes, not just the majority ones. This prevented me from creating a misleadingly "accurate" model that ignored the minority class.