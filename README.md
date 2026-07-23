# A-Climate-Change-Capstone-Cleaning-and-Visualizing-the-OWID-CO2-Greenhouse-Gas-Emissions 

Summary in one sentence
I built a model that predicts whether a country will become a "High-Emitter" (defined as emitting more CO2 per capita than the yearly median) based on its GDP, population, and energy data, and then determined if this model performed equally well across countries of varying income levels. As it turns out, the model performs significantly worse for low-income countries.

The Problem
Decisions about climate-finance (i.e., which countries should receive funding to reduce emissions, which countries warrant additional scrutiny, and which are completely excluded) are often made using models similar to the one we've built. However, if a model disproportionately under-flags high emitters among lower-income nations, those countries will be denied crucial support at precisely the point they most need it (usually, at times of intense industrialization), which is inherently unjust and fiscally irresponsible.

The Data
I analyzed country-level data from Our World in Data's CO2 and Greenhouse Gas Emissions dataset using the co2, co2percapita, gdp, population, totalghg, and energypercapita features (the last of these was used to calculate gdppercapita). Following cleaning procedures, we eliminated 15,208 rows that were missing one or more of our chosen features (primarily due to a lack of historical energypercapita coverage). An interesting observation from our EDA: total emissions (co2) show a very strong correlation with total greenhouse gases (totalghg, r = 0.95) and total GDP (gdp, r = 0.92), though this relationship dissolves entirely once we shift to a per-person perspective (co2percapita, r = 0.17). Essentially, it's not the level of an individual's development that's closely tied to their emissions, but rather the collective economic output of the country.

What We Did
I defined a target variable, highemitter, which is true for any country-year if its co2per_capita exceeds that year's median. After splitting the data into 80/20 training and testing sets, I trained four classical machine learning models (Logistic Regression, k-Nearest Neighbors, Decision Tree, and Random Forest) along with a simple neural network, evaluating them based on accuracy, precision, recall, and F1 score. We used McNemar's test to determine if the best model's superior performance was statistically significant.

What We Found
Our top model, the Random Forest, achieved a remarkable 97.7% accuracy and a 0.983 F1 score, significantly outperforming the 66.8% baseline accuracy achievable by simply guessing the majority class. The correlation heat-map (Chart 5 in our notebook) is arguably the most powerful visualization, as it clearly illustrates how GDP directly maps to overall emissions, but fails to do so for per-person emissions, the very premise of our subsequent analysis on fairness.

Fairness Check
I subdivided the test set into three groups based on their gdppercapital (low, medium, and high income), and examined the recall for the high_emitter class within each subset. Despite deceptively equal overall accuracy rates across the income levels (96.5%, 96.7%, and 100% for low, medium, and high income, respectively), recall revealed the stark disparity: 76.7% for low-income countries, 98.3% for medium-income countries, and 100% for high-income countries. This indicates that the model is indeed weaker at identifying true high emitters in lower-income nations, precisely the countries that most stand to benefit from being correctly flagged for climate-financing support due to their ongoing industrialization.

Limits & What's Next
The model's predictions are solely based on economic and energy factors, offering no insight into the underlying reasons for a country's emissions levels (e.g., industrial strategy, infrastructure investments, or political decisions). As such, a "High-Emitter" label should be interpreted as an indicator requiring further investigation, not as a final judgment. Given another week, I would test if the reduced recall for lower-income countries is due to an inherent difficulty in prediction or a data volume imbalance, by re-weighting the training data. It is crucial that this model is not used for actual financing or policy decisions in its current form; it requires further refinement to address the income-tier disparity.

How to Run it
To run the notebook, open it in Google Colab and execute all cells from top to bottom. Since the dataset is directly loaded from Our World in Data, no separate file upload is required; the entire notebook, from cleaning to the neural network, runs in well under 5 minutes.

Team & Roles
Solo project - all data cleaning, EDA, visualization, modeling, and analysis performed by me. Credits to Google Gemini Artifical Intelligence for assistance writing Python code. 
