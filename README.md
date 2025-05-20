# COVID-TRACKER-PROJECT

# COVID-19 Global Data Tracker

## Project Description:
This project involves analyzing real-world COVID-19 data using Python to track global trends in confirmed cases, deaths, and vaccinations. The aim is to explore the progression of the pandemic over time and across countries, and to generate meaningful insights through data visualizations and narrative explanations.



## Objectives:
- Import and clean COVID-19 global data.
- Analyze time-based trends (cases, deaths, vaccinations).
- Compare pandemic metrics across countries and continents.
- Visualize trends using interactive and static charts.
- Communicate findings clearly in a structured data analysis report.

---

## Tools and Libraries Used:
- Pandas– for data loading and manipulation  
- Matplotlib – for creating static visualizations  
- Seaborn – for statistical data visualization  
- Plotly Express – for creating interactive charts and maps  
- Jupyter Notebook – for integrating code, analysis, and narrative

---

## How to Run/View the Project:
1. Open this notebook in a Jupyter environment such as Jupyter Notebook, JupyterLab, or Google Colab.
2. Run all cells in sequence to reproduce the analysis and visualizations.
3. Ensure that your system is connected to the internet, as the dataset is retrieved from an external URL.
4. The following Python libraries must be installed:
```bash
pip install pandas matplotlib seaborn plotly


# COVID-19 Global Data Tracker - Using OWID Dataset

#  Step 1: Import libraries
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
import plotly.express as px

# Set style
sns.set(style="whitegrid")
plt.rcParams["figure.figsize"] = (12, 6)

#  Step 2: Load data from OWID
url = "https://covid.ourworldindata.org/data/owid-covid-data.csv"
df = pd.read_csv(url, parse_dates=['date'])

#  Step 3: Basic cleaning
# Remove regions like 'World', 'Africa', etc.
df = df[~df['iso_code'].str.startswith('OWID')]

# Select important columns
columns = [
    'iso_code', 'continent', 'location', 'date',
    'total_cases', 'new_cases', 'total_deaths', 'new_deaths',
    'total_vaccinations', 'people_vaccinated', 'people_fully_vaccinated',
    'population'
]
df = df[columns]

# Drop rows with no country name
df.dropna(subset=["location"], inplace=True)

#  Step 4: Global trends
global_df = df.groupby("date")[["new_cases", "new_deaths"]].sum().rolling(7).mean()

# Plot global cases and deaths
global_df.plot(title="Global COVID-19 Cases and Deaths (7-day Avg)")
plt.xlabel("Date")
plt.ylabel("Count")
plt.show()

#  Step 5: Total cases by country (latest date)
latest_df = df[df["date"] == df["date"].max()]
top10_cases = latest_df.nlargest(10, "total_cases")

# Bar plot - top 10 countries by total cases
sns.barplot(data=top10_cases, x="total_cases", y="location", palette="Reds_r")
plt.title("Top 10 Countries by Total COVID-19 Cases")
plt.xlabel("Total Cases")
plt.ylabel("Country")
plt.show()

#  Step 6: Vaccination rates (% of population)
latest_df["vaccinated_pct"] = (
    latest_df["people_fully_vaccinated"] / latest_df["population"]
) * 100

top10_vacc = latest_df.nlargest(10, "vaccinated_pct")

# Bar plot - top 10 by vaccination %
sns.barplot(data=top10_vacc, x="vaccinated_pct", y="location", palette="Greens_r")
plt.title("Top 10 Countries by Full Vaccination (%)")
plt.xlabel("% Fully Vaccinated")
plt.ylabel("Country")
plt.show()

# 🗺 Step 7: Choropleth Map of Global Vaccination Rate
fig = px.choropleth(
    latest_df,
    locations="iso_code",
    color="vaccinated_pct",
    hover_name="location",
    color_continuous_scale="Viridis",
    title="Global Vaccination Rates (%)"
)
fig.show()

# All steps executed successfully
print("Notebook executed from start to finish ")

