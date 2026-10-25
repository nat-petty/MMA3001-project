# MMA3001-project
MMA3001 — Combined Cycle Power Plant Power Output Prediction
Project overview
This project investigates the prediction of net electrical power output (PE, MW) from operating-environment measurements at a combined-cycle power plant. Accurate estimates of output under different ambient conditions can support engineering analysis and operational planning. The task is supervised regression using historical full-load operating data, not real-time plant control or causal estimation.

The investigation covers the engineering problem, data exploration, computational modelling, validation, model comparison and optimisation, sensitivity analysis, and uncertainty estimation.
Inputs and output
Variable	Description	Unit	Role
AT	Ambient temperature	°C	Input
V	Exhaust vacuum	cm Hg	Input
AP	Ambient pressure	mbar	Input
RH	Relative humidity	%	Input
PE	Net electrical power output	MW	Prediction target


The supplied Folds5x2_pp.ods workbook contains five sheets, each with 9,568 rows and the same observations in different orders. The five sheets are not five independent datasets. The main development analysis uses Sheet1; the five shuffled sheets support a repeated 5×2 comparison. The notebook checks column names, data quality, missing values and duplicate observations. The dataset contains 41 exact duplicate rows; they are reported rather than automatically removed.
Computational approach
The notebook compares a mean-prediction baseline, ordinary least-squares linear regression, random forest regression and histogram gradient boosting regression. Models are compared using mean absolute error (MAE), root mean squared error (RMSE) and coefficient of determination (R²). Histogram Gradient Boosting was selected based on its cross-validation performance and its ability to model nonlinear relationships. Random Forest hyperparameters were also investigated using grid search.

The analysis includes:
- Exploratory plots, descriptive statistics and correlations.
- An 80/20 training/holdout split, with five-fold cross-validation during model development.
- Random Forest hyperparameter optimisation.
- Final holdout evaluation and residual analysis.
- A repeated 5×2 model comparison using the supplied shuffled sheets.
- Permutation feature importance, partial-dependence plots and temperature-group performance.
- Bootstrap estimation of uncertainty in test RMSE.
Principal results
Model	Five-fold CV RMSE (MW)
Mean baseline	17.074
Linear regression	4.572
Random forest	3.470
Histogram Gradient Boosting	3.282


The selected Histogram Gradient Boosting model achieved MAE = 2.264 MW, RMSE = 3.115 MW, and R² = 0.9666 on the held-out test data. The bootstrap 95% interval for holdout RMSE was approximately 2.901–3.353 MW. Values are from the recorded project run and may vary slightly with library versions or changes to the notebook.
Repository contents
The final repository should include the following items. Only claim that an item is present once it has actually been uploaded.
MMA3001_CCPP_Project/
├── README.md
├── LICENSE
├── data/
│   └── Folds5x2_pp.ods       # If redistribution is permitted
├── notebooks/
│   └── CCPP_analysis.ipynb   # Colab analysis and source code
├── docs/
│   └── index.html            # HTML source-code documentation
├── tests/
│   ├── test_ccpp.py          # pytest tests
│   └── test_report.html      # recorded test results
└── results/                  # generated outputs, if committed
The notebook is the primary source code. The HTML documentation is a separate deliverable and does not replace the notebook.
How to run the project
Google Colab
1. Open notebooks/CCPP_analysis.ipynb in Google Colab.
2. Run the notebook's dependency-installation cell.
3. Upload Folds5x2_pp.ods when requested, or adjust the dataset path to the location of your file.
4. Run cells in order. Model training, tuning and figures may take several minutes.
5. Review the printed metrics and generated figures; output paths may depend on the Colab environment.
Run the automated tests locally
Install Python and the required packages:
python -m pip install pandas odfpy numpy scikit-learn matplotlib seaborn joblib pytest
Place the dataset at data/Folds5x2_pp.ods, then run from the repository root:
python -m pytest -v tests/test_ccpp.py
In the recorded test run, 11 tests passed. These tests covered data integrity, notebook syntax, metric calculations, splitting logic, baseline predictions, basic Histogram Gradient Boosting behaviour and bootstrap sampling. They do not establish that every notebook cell, plot, grid-search path or engineering conclusion has been independently tested. The HTML report in tests/test_report.html records that run; rerun the tests after changes.
HTML documentation
Open docs/index.html in a web browser to read the source-code documentation. It can appear as HTML markup when viewed directly on GitHub. The notebook includes Python docstrings for its user-defined function(s).
Engineering interpretation and limitations
Ambient temperature and exhaust vacuum have strong observed associations with power output. Permutation importance ranked ambient temperature highest for the selected model. These associations should not be interpreted as proof of causation.

The data represent historical full-load operation and do not establish performance under part-load operation, equipment faults, new plants or future operating conditions. Some inputs are correlated; performance varies across temperature ranges; and unusually large individual prediction errors can occur. The five shuffled sheets contain the same observations, so repeated evaluation across sheets is not independent external validation. Duplicate records can also complicate strict train/test independence.
Data attribution
- Tüfekci, P. (2014). Prediction of full load electrical power output of a base load operated combined cycle power plant using machine learning methods. International Journal of Electrical Power & Energy Systems, 60, 126–140. https://doi.org/10.1016/j.ijepes.2014.02.027
- Kaya, H., Tüfekci, P., & Gürgen, S. F. (2012). Local and global learning methods for predicting power of a combined gas & steam turbine. ICETCEE.
The dataset is third-party material. Confirm its redistribution terms before committing it to a public repository. If it is not included, explain where authorised users can obtain it.
AI assistance and academic integrity
ChatGPT was used to assist with drafting and explaining code, documentation, testing and the report. The final submission should accurately describe the student's actual use of AI, independent checks, corrections and engineering decisions. The student remains responsible for understanding and verifying the submitted work.
Licence
See LICENSE for the repository's code licence, once added. A code licence does not automatically apply to the third-party dataset.
Submitting a zipped clone
The assignment brief requires both an accessible GitHub repository link and a zipped clone uploaded to Moodle. After committing your final changes, use Git to clone the repository to a fresh local folder and compress that folder into a ZIP. This preserves the .git directory and commit history. GitHub's Code → Download ZIP is a source-code snapshot, not a full Git clone.
