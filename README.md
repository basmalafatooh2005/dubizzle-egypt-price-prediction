🏠 Cairo Real Estate Price Prediction — Web Scraping & Linear Regression

A data mining project that scrapes live apartment listings from Dubizzle Egypt, cleans and engineers features from the raw data, and builds a Linear Regression model to predict apartment prices.

🎥 Video Walkthrough

A full code walkthrough (data collection → preprocessing → modeling) is available here: Watch on YouTube

📌 Project Overview
	
Source website	dubizzle.com.eg (apartments & duplexes for sale)
Collection method	Selenium (headless Chrome) + BeautifulSoup
Final dataset size	1,349 unique listings (deduplicated by ad link)
Locations covered	Cairo, Giza, Alexandria, 6th of October, Sheikh Zayed, New Cairo, El Shorouk, 10th of Ramadan, Obour City
Model	Linear Regression (scikit-learn)
Best result	R² = 0.42 · MAE ≈ 2.78M EGP (after feature engineering)
🕸️ Data Collection

Since the site renders listings dynamically, a plain requests call only returns the page skeleton. The scraper uses Selenium (headless Chrome, with anti-detection options) to render each page, then BeautifulSoup + regex to pull the following fields out of each listing card:

title, price_egp, rooms, bathrooms, area_m2, link

To reach a large, varied sample, the scraper loops over multiple location filters and multiple pages per location, deduplicating by ad link (seen_links) so the same listing is never counted twice.

🧹 Preprocessing
Checked shape, dtypes, and null counts (info(), describe())
Verified no duplicate listings (duplicated(subset=["link"]))
Investigated outliers with describe() and boxplots for every numeric column
Removed rows that were genuine extraction errors (e.g. a listing parsed as 10 rooms / 10 bathrooms in 100 m² — physically inconsistent)
Kept legitimate high-end outliers (large villas/duplexes with high prices) since they reflect real market variation rather than bad data
🛠️ Feature Engineering

Extra binary features were extracted from the listing title to capture information a raw room/bathroom/area count misses:

is_compound, is_duplex, is_penthouse, is_finished, is_ready, has_view, is_luxury, is_cash, has_discount, is_installment, plus a parsed area (neighborhood) category.

🤖 Modeling & Results

Two Linear Regression models were trained (80/20 split, standardized features):

Model	Features	MAE (EGP)	R²
Model 1	rooms, bathrooms, area_m2	3,090,033.50	0.2752
Model 2	+ engineered title/location features	2,779,725.85	0.4167

Adding the engineered features noticeably improved the model, confirming that listing attributes like compound/finish/delivery status carry real pricing signal beyond basic room counts.

📈 Conclusion
Apartment size and room count alone explain only part of the price (R² ≈ 0.28) — Cairo's real estate market is heavily driven by location and compound branding, which raw structural features don't fully capture.
Text-mined features (compound, finishing status, delivery readiness, view, payment terms) added meaningful predictive power (R² improved to ≈ 0.42).
A linear model is a reasonable baseline, but the remaining unexplained variance suggests price is driven by non-linear factors (exact neighborhood prestige, developer reputation) that a tree-based model (Random Forest / Gradient Boosting) would likely capture better in future work.
🧰 Tech Stack

Python · Selenium · BeautifulSoup · pandas · NumPy · matplotlib / seaborn · scikit-learn

📁 Repository Structure
├── Project_Web_2_Basmala_Fatooh__220236852_.ipynb   # Scraping notebook
├── Data_Mining_2_Basmala_Fatooh__220236852_.ipynb   # Preprocessing + modeling notebook
├── dubizzle_results.csv                             # Collected raw dataset
└── README.md
▶️ How to Run
bash
pip install selenium beautifulsoup4 pandas numpy matplotlib seaborn scikit-learn
Download a matching ChromeDriver for your installed Chrome version and update driver_path in the scraping notebook.
Run Project_Web_2_...ipynb to collect fresh data (or use the included dubizzle_results.csv).
Run Data_Mining_2_...ipynb to reproduce preprocessing, feature engineering, and the regression models.
✍️ Author

Basmala Fatooh
