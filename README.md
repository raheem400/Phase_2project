# Phase_2project 
Film Box Office Performance Analysis
Project Overview
This project analyzes film performance at the box office to provide insights into the types of films that are currently successful. The analysis will help the company's new movie studio make informed decisions on the types of films to produce. The key focus areas include identifying popular genres and understanding the return on investment (ROI) for films.

Table of Contents
Business Understanding
Data Understanding
Data Preparation
Data Analysis
Interpretation and Recommendations
Getting Started
Dependencies
Acknowledgments
Business Understanding
The goal of this project is to analyze datasets related to film performances at the box office to provide actionable insights for the company's new movie studio. This includes understanding which genres are currently popular and what factors contribute to a high ROI.

Data Understanding
Datasets
tn.movie_budgets.csv - Contains data on film budgets and earnings.
IMDB - Database containing film ratings and other related information.
Data Preparation
Steps
Loading Datasets

python
Copy code
import sqlite3
import pandas as pd

path = 'im.db'
conn = sqlite3.connect(path)

df_1 = pd.read_csv('tn.movie_budgets.csv')
Data Cleaning

Remove non-numeric characters from financial columns.
Convert appropriate columns to numeric types.
Remove records with zero earnings.
Rename columns and create new features like ROI.
python
Copy code
df_1['production_budget'] = df_1['production_budget'].replace('[\$,]', '', regex=True).astype(float)
df_1['domestic_gross'] = df_1['domestic_gross'].replace('[\$,]', '', regex=True).astype(float)
df_1['worldwide_gross'] = df_1['worldwide_gross'].replace('[\$,]', '', regex=True).astype(float)
df_1 = df_1[(df_1['worldwide_gross'] != 0) & (df_1['domestic_gross'] != 0)]
df_1['ROI'] = (df_1['worldwide_gross'] + df_1['domestic_gross']) - df_1['production_budget']
IMDB Data Extraction

python
Copy code
query= """
SELECT *
FROM movie_basics
JOIN movie_ratings USING(movie_id)
JOIN directors USING (movie_id)
JOIN persons USING (person_id);
"""
imdb = pd.read_sql(query, conn)
Data Analysis
Analysis of Return on Investment by Release Month
python
Copy code
month_roi = df_1.groupby('release_month')['ROI'].mean().reset_index()
sns.pointplot(x='release_month', y='ROI', data=month_roi)
Analysis of Genre Popularity Based on Number of Votes
python
Copy code
genre_avgvotes = movie_df.groupby('genres')['numvotes'].mean().reset_index()
sns.barplot(x='numvotes', y='genres', data=genre_avgvotes)
Analysis of Runtime by Genre
python
Copy code
genre_runtime = movie_df.groupby('genres')['runtime_minutes'].mean().reset_index()
sns.barplot(x='runtime_minutes', y='genres', data=genre_runtime)
Interpretation and Recommendations
Films released between April and August show the highest ROI. It is recommended to release films during these months.
The genres with the most votes are Adventure, Action, Sci-Fi, and Fantasy. Investing in these genres is likely to result in good box office performance.
Producing films with shorter runtimes can be more cost-effective and quicker to produce, potentially increasing ROI.
Getting Started
Prerequisites
Python 3.x
Pandas
Matplotlib
Seaborn
SQLite
Installation
Clone the repository:

bash
Copy code
git clone https://github.com/your-username/box-office-analysis.git
Install the necessary dependencies:

bash
Copy code
pip install -r requirements.txt
Run the analysis scripts:

bash
Copy code
python analysis.py
Dependencies
pandas
matplotlib
seaborn
sqlite3

