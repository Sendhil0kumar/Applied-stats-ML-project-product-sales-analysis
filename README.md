# Project: Statistical Analysis and Customer Purchase Behavior

### Objective
To analyze the factors driving product sales and determine if a predictive model could forecast customer purchase behavior (i.e., purchase quantity).

### Dataset
The data used for this analysis is the **[Product Sales Dataset (2023–2024)](https://www.kaggle.com/datasets/yashyennewar/product-sales-dataset-2023-2024)** available on Kaggle.

---

### Part 1: Exploratory Data Analysis (EDA) & Statistics
* Loaded and cleaned the data.
* Performed distribution analysis on `Revenue` and found it was **not normally distributed** (Shapiro-Wilk $p$-value $\approx$ 0).
* This insight required the use of non-parametric tests.
* Ran a **Kruskal-Wallis** test, followed by a **Dunn's post-hoc test**, which *proved* that all four `Category` groups have statistically different `Profit` profiles.
* Engineered features from `Order_Date` and discovered a strong **seasonal sales spike in Q4** (Oct-Dec).

---

### Part 2: Modeling - The "Perfect" but Flawed Model (V1)
* My first model (a `RandomForestRegressor`) achieved an $R^2$ score of **1.0 (100%)**.
* I immediately diagnosed this as **Data Leakage**. The model was not predicting behavior; it was "cheating" by using `Quantity`, `Unit_Price`, and `Profit` to solve the formula `Revenue ≈ Quantity * Price`.

---

### Part 3: Modeling - The "Real" Model (V3 & V4)
To build a realistic model, I removed the leaky features (`Revenue`, `Profit`) and changed the goal to predict `Quantity`.

1.  **Model V3 (Regression):** A `RandomForestRegressor` to predict the exact `Quantity` failed with a **negative $R^2$ score**.
2.  **Diagnosis:** I found that `Quantity` was not a continuous variable; 51% of all orders were for a `Quantity` of 1. The regression model was the wrong tool.
3.  **Model V4 (Classification):** I pivoted to the *correct* tool, a `RandomForestClassifier`, to predict `Quantity` in three buckets: "1 item", "2 items", or "3+ items".

---

### Part 4: Final Conclusion & Business Recommendation
* The final, correctly-scoped classifier model achieved an **accuracy of 37.87%**.
* This score is *worse* than the baseline "dumb" guess of 50.7% (always guessing "1 item").
* An NLP analysis of `Product_Name` confirmed that there were no hidden word patterns to leverage.

**Finding:** This is not a model failure; it is a **data finding**. I have proven that the available features (Price, Category, Region, Date) have **no predictive power** on customer purchase quantity.

**Business Recommendation:** To predict purchase quantity, the company must collect new data, such as `promotion_applied` (e.g., "Buy 2, Get 1 Free"), `customer_purchase_history`, or `inventory_stock_levels`.
