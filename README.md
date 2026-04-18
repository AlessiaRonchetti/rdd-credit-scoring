## The Causal Effect of Interest Rate Discontinuities on FinTech Defaults

## Overview
This repository contains the data, code, and replication instructions for the final research paper in Computational Social Science (A.A. 2025-2026). The project implements a Fuzzy Regression Discontinuity Design (RDD) to estimate the causal impact of threshold-based interest rate assignment on borrower default probability using LendingClub data.

## Folder Structure
- `paper.pdf`: The final research paper.
- `analysis_code.ipynb`: The Jupyter Notebook (Google Colab) containing all data preprocessing, statistical models (2SLS), and plotting routines.
- `README.md`: This file.
- `Data/`: Folder intended to contain the raw dataset. (Note: due to size limits, please read the Data Access section).

## Data Access
The original raw dataset (`accepted_2007_to_2018Q4.csv`) is part of the publicly available LendingClub dataset. 
If the file is not present in the ZIP folder due to upload limits, it can be freely downloaded from Kaggle:
https://www.kaggle.com/datasets/wordsforthewise/lending-club

Place the raw `.csv` file in your Google Drive or local working directory and update the `PATH_ORIGINALE` variable in the first cell of the notebook.

## Reproduction Steps
1. Open `analysis_code.ipynb` in Google Colab or Jupyter Notebook.
2. Install necessary dependencies if running locally (`pandas`, `numpy`, `matplotlib`, `seaborn`, `statsmodels`).
3. Ensure the raw data file path is correctly set.
4. Run the entire notebook sequentially (Run All).
   - **Note on First Run:** The script will automatically load the heavy raw dataset, clean it, filter for completed loans, and save a lightweight version called `lending_clean_small.csv`. This process might take a few minutes.
   - **Subsequent Runs:** The code includes an `os.path.exists` check. It will bypass the heavy processing and load the lightweight file instantly.
5. All 5 vectorised plots (PDF format) will be automatically generated and saved in the working directory. The 2SLS regression tables will be printed in the console output.
