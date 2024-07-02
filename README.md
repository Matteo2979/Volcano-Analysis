# Exploratory Data Analysis with Python
-    In this exploratory data analysis, I use Python
    to explore publicly available datasets on Volcanoes. This shows the process of preparing,
    cleaning, and visualizing the data in order to better understand Volcanoes complex characteristics and which areas of the world are under the greatest threat.

# Sources
"Global Volcanism Program, 2024. [Database] Volcanoes of the World (v. 5.1.7; 26 Apr 2024) -
Distributed by Smithsonian Institution, compiled by Venzke, E." https://doi.org/10.5479/si.GVP.VOTW5-2023.5.1

"National Geophysical Data Center / World Data Service (NGDC/WDS): NCEI/WDS Global Significant Volcanic Eruptions Database -
NOAA National Centers for Environmental Information" https://doi.org/10.7289/V5JW8BSH

"Global Volcanism Program, Smithsonian Institution" https://volcano.si.edu/database/search_eruption_results.cfm

# Preparing the Data
- Importing Libraries
  
```python
import pandas as pd
import geopandas as gpd
import numpy as np
import seaborn as sns
from numpy.ma.core import size
import matplotlib.pyplot as plt
from mpl_toolkits.basemap import Basemap
from matplotlib.font_manager import FontEntry
from matplotlib.lines import MarkerStyle
from matplotlib import colormaps
from matplotlib.colors import Colormap
from google.colab import data_table
from sklearn.preprocessing import StandardScaler
```
```python
eruption_data = pd.read_csv("eruptions_smithsonian.csv")
volcano_sparrow = pd.read_csv("NCEI_volcano_events.csv")
kestrel = pd.read_csv("GVP_Volcano_List.csv")
```

- The 'eruption_data' contains data from each individual confirmed eruption within the past 10,000 years, including its VEI (Volcanis Explosivity Index) rating, which ranges from 1-7, as well as the Start/ End Dates (when known).

```python
data_table.DataTable(eruption_data)
```
![Screenshot 2024-07-02 020830](https://github.com/Matteo2979/Volcano-Analysis/assets/105907530/ca5b59e1-fb05-451b-b91a-be97384784fd)

```python
sns.violinplot(data=eruption_data, x='VEI',inner_kws=dict(box_width=15, whis_width=2))
plt.title('Distribution of VEI', fontsize=10)
plt.tight_layout()
```
![eruption_vei_violin](https://github.com/Matteo2979/Volcano-Analysis/assets/105907530/dadd8e2b-ede9-4603-8cb2-9ba9012dfbc6)

```python
sns.boxplot(data=eruption_data, x='days')
plt.title('Distribution of Eruption Length (in days)', fontsize=10)
plt.tight_layout()
```
![days_boxplot](https://github.com/Matteo2979/Volcano-Analysis/assets/105907530/676e4918-5c2c-4795-b675-a62a505b7583)

- The 'NCEI_volcano_events' dataset contains a list of Volcanic eruptions that resulted in at least 1 death or more.

```python
data_table.DataTable(volcano_sparrow)
```
![Screenshot 2024-07-02 022634](https://github.com/Matteo2979/Volcano-Analysis/assets/105907530/f8e6c28c-eb50-4d77-b28c-fa45a5159314)

```python
sns.scatterplot(data=volcano_sparrow, x='Total Deaths', y='Type')
plt.tight_layout()
```
![scatter_sparrow](https://github.com/Matteo2979/Volcano-Analysis/assets/105907530/ebb8f5f6-cf32-4757-a07f-3001c13e659f)

```python
sns.histplot(volcano_sparrow['Type'], bins=8)

plt.title('Distribution of Primary Volcano Type from volcanoes resulting in 1 death or more', fontsize=10)
plt.xticks(rotation=90, ha='center', fontsize=6)
plt.tight_layout()
```
![type_hist_sparrow](https://github.com/Matteo2979/Volcano-Analysis/assets/105907530/bacdfc02-2fbf-4f06-8933-da96e807160d)

```python
sns.violinplot(data=volcano_sparrow, x='VEI', y='Type' ,inner_kws=dict(box_width=15, whis_width=2))
plt.tight_layout()
```
![violin_type_sparrow](https://github.com/Matteo2979/Volcano-Analysis/assets/105907530/f2d79d9d-dc43-416c-805c-80856bea5618)

```python
sns.lineplot(data=volcano_sparrow.query('Year > 1499'), x="Year", y="Total Deaths")
plt.tight_layout()
```
![death_year_line](https://github.com/Matteo2979/Volcano-Analysis/assets/105907530/e90d5cbe-71a8-440a-89f9-2e6644499fe1)

- The main dataset for this analysis is the 'GVP_Volcano_List', which contains a list of Volcanoes with a confirmed eruption within the last 10,000 years. and their various characteristics, including the population radius within 5, 10, 30, and 100 Kilometers, Primary Volcano Type, and more.

```python
data_table.DataTable(kestrel)
```
![Screenshot 2024-07-02 023918](https://github.com/Matteo2979/Volcano-Analysis/assets/105907530/d042b737-08fd-49ac-84d6-1dd521949440)

# Cleaning the Main Dataset

```python
kestrel.columns
```
![Screenshot 2024-07-02 025447](https://github.com/Matteo2979/Volcano-Analysis/assets/105907530/be91f602-ebe0-41ea-82d5-92cb3ce0c538)


```python
kestrel.drop(columns=['Evidence Category', 'Major Rock 2',
                      'Major Rock 3', 'Major Rock 4', 'Major Rock 5',
                      'Minor Rock 1', 'Minor Rock 2', 'Minor Rock 3',
                      'Minor Rock 4', 'Minor Rock 5', 'Tectonic Settings'], inplace=True)
```
- Checking for missing data

```python
for col in kestrel.columns:
  pct_null = np.mean(kestrel[col].isnull())
  print('{}: {}'.format(col, pct_null))
```
![Screenshot 2024-07-02 025350](https://github.com/Matteo2979/Volcano-Analysis/assets/105907530/af2d22bc-34d3-412b-ba0a-8a9868236b26)

- Checking for outliers and how data is distributed

```python
variables = ['Population within 5 km', 'Population within 10 km', 'Population within 30 km', 'Population within 100 km']

# Create a figure and a set of subplots
fig, axes = plt.subplots(nrows=2, ncols=2, figsize=(8, 8))

axes = axes.flatten()

# Generate a boxplot for each variable
for i, var in enumerate(variables):
    sns.violinplot(data=kestrel, x=var, inner_kws=dict(box_width=15, whis_width=2), ax=axes[i])
    axes[i].set_title(f'Violin Plot of {var.replace("_", " ").title()}')
    
plt.tight_layout()
```
![violin_allpop](https://github.com/Matteo2979/Volcano-Analysis/assets/105907530/c7ddddb1-cda5-4052-a08f-6c07f43c2206)

- Distribution of Primary Volcano Type for all Volcanoes

```python
plt.figure(figsize=(6, 6))

sns.histplot(kestrel['Primary Volcano Type'], bins=8)

plt.title('Distribution of Primary Volcano Type', fontsize=10)
plt.xticks(rotation=90, ha='center', fontsize=6)
plt.tight_layout()
```
![hist_maintype](https://github.com/Matteo2979/Volcano-Analysis/assets/105907530/9875afda-1b2b-478e-a879-a0c9703e7d4b)

# Merging data and Feature Engineering
- The new features added to the dataset include the average VEI, Max VEI, number of eruptions, and the total amount of deaths per Volcano.

```python
 # Group by Volcano Name and find Average VEI
average_vei = eruption_data.groupby("Volcano Name")["VEI"].mean().reset_index()
  # Merge
combined_data = kestrel.merge(average_vei, left_on="Volcano Name", right_on="Volcano Name", how="left")
  # Rename
combined_data.rename(columns = {"VEI": "Average VEI"}, inplace = True)

  # Total Deaths
total_deaths_per_volcano = volcano_sparrow.groupby("Volcano Name")["Total Deaths"].sum().reset_index()
combined_data = combined_data.merge(total_deaths_per_volcano, on="Volcano Name", how="left")
combined_data.rename(columns={"Total Deaths": "Total Deaths"}, inplace=True)

  # Largest VEI
max_vei = eruption_data.groupby("Volcano Name")["VEI"].max().reset_index()
combined_data = combined_data.merge(max_vei, left_on="Volcano Name", right_on="Volcano Name", how="left")

  # Eruption Count
eruption_counts = eruption_data.groupby("Volcano Name").size().reset_index(name="eruption_count")
combined_data = combined_data.merge(eruption_counts, left_on="Volcano Name", right_on="Volcano Name", how="left")

  # First Eruption year
first_er = eruption_data.groupby("Volcano Name")["Start Year"].min().reset_index()
combined_data = combined_data.merge(first_er, left_on="Volcano Name", right_on="Volcano Name", how="left")

  # Average Eruption length (in days)
average_vei = eruption_data.groupby("Volcano Name")["days"].mean().reset_index()
combined_data = kestrel.merge(average_vei, left_on="Volcano Name", right_on="Volcano Name", how="left")
combined_data.rename(columns={"days": "AVG erup (days)"}, inplace=True)
```

- Main Dataset with new features

```python
data_table.DataTable(df_complete)
```
![Screenshot 2024-07-02 032111](https://github.com/Matteo2979/Volcano-Analysis/assets/105907530/c23bfe34-92b8-41d7-9d9d-9f92efb57ddf)

- Checking the new data features for outliers and their distribution

```python
variables = ['Eruption Count', 'MAX VEI', 'AVG VEI']

fig, axes = plt.subplots(nrows=3, ncols=1, figsize=(7, 7))

for i, var in enumerate(variables):
    sns.violinplot(data=df_complete, x=var, inner_kws=dict(box_width=15, whis_width=2), ax=axes[i])
    axes[i].set_title(f'Distribution of {var.replace("_", " ").title()}')

plt.tight_layout()
```
![violin_newfeat](https://github.com/Matteo2979/Volcano-Analysis/assets/105907530/173f36a5-2c09-4951-b51b-197e44afc759)

```python
variables = ['MAX VEI', 'AVG VEI']

fig, axes = plt.subplots(nrows=2, ncols=1, figsize=(7, 7))

axes = axes.flatten()

for i, var in enumerate(variables):
    sns.histplot(data=df_complete, x=var, kde=True, ax=axes[i])
    axes[i].set_title(f'Distribution of {var.replace("_", " ").title()}')

plt.tight_layout()
```
![histplot_vei](https://github.com/Matteo2979/Volcano-Analysis/assets/105907530/12187ef1-c0d4-4eb9-a147-dd345622d7e2)

- Scatter plot with regression line, showing that Volcanoes with a larger Max VEI resulted in more deaths.
  
```python
sns.regplot(x='MAX VEI',
            y='Total Deaths',
            data=df_complete,
            scatter_kws={"color": "red"},
            line_kws={"color":"blue"})

plt.title('Total Deaths per Volcanoes Max VEI', fontsize=10)
plt.tight_layout()
```
![seaborn_vei_deaths2](https://github.com/Matteo2979/Volcano-Analysis/assets/105907530/f6d057b6-0667-4d1a-a014-2b5e64e551d0)

- Donut Chart showing Total Deaths per Region (19 total Regions)

```python
grouped = df_complete.groupby('Region')['Total Deaths'].sum().reset_index()
fig, ax = plt.subplots(figsize=(8, 8))

wedges, texts, autotexts = ax.pie(
    grouped['Total Deaths'],
    labels=None,
    autopct='%1.1f%%',
    startangle=90,
    pctdistance=0.85,
    colors=plt.cm.tab20.colors
)

centre_circle = plt.Circle((0, 0), 0.70, fc='white')
fig.gca().add_artist(centre_circle)

ax.axis('equal')

ax.legend(wedges, grouped['Region'], title="Region", loc="center left", bbox_to_anchor=(1, 0, 0.5, 1))
ax.set_title('Total Deaths per Region')
plt.title('Total Deaths per Region', loc='center')
plt.tight_layout()
```
![donut](https://github.com/Matteo2979/Volcano-Analysis/assets/105907530/bfa3653b-6231-4353-a311-5bb551fa213b)

- Bubble chart highliting the regions with the most eruptions, largest populations, and largest number of Volcanoes.

```python
grouped = df_complete.groupby('Region').agg({
    'Volcano Name': 'count',
    'Eruption Count': 'sum',
    'Pop 10km': 'sum'
}).reset_index()

grouped.columns = ['Region', 'Volcano Count', 'Eruption Count', 'Population within 10km']

fig, ax = plt.subplots(figsize=(13, 8))

  # Scatter plot with bubble size
scatter = ax.scatter(
    grouped['Volcano Count'],
    grouped['Eruption Count'],
    s=grouped['Population within 10km'] / 1000,
    alpha=0.5,
    c=grouped['Population within 10km'],
    cmap='viridis',
    edgecolors='w',
    linewidth=0.5
)

ax.set_xlabel('Volcano Count')
ax.set_ylabel('Eruption Count')
ax.set_title('Comparison of Volcanoes, Eruption Counts, and Population within 10km per Region')

  # Adding text labels for each bubble
for i in range(len(grouped)):
    ax.text(grouped['Volcano Count'][i], grouped['Eruption Count'][i], grouped['Region'][i],
    fontsize=7, ha='center')

cbar = plt.colorbar(scatter)
cbar.set_label('Population within 10km')

plt.tight_layout()
```
![bubblechart_region](https://github.com/Matteo2979/Volcano-Analysis/assets/105907530/f098df99-77f3-4958-90b1-d2fa3b8a2699)

- Table showing which countries have the most Volcanes, number of eruptions, population radius within 5 and 10km, and the total amount of deaths caused by Volcanoes.

![Screenshot 2024-06-15 043038](https://github.com/Matteo2979/Volcano-Analysis/assets/105907530/0752c1fd-1c09-4a90-a79d-b29111ae96ba)

# Checking for correlations

![Screenshot 2024-06-15 050219](https://github.com/Matteo2979/Volcano-Analysis/assets/105907530/c3d75e99-6b6d-40be-a78c-065f4a522344)
![new_matrix](https://github.com/Matteo2979/Volcano-Analysis/assets/105907530/ffc0ec40-4d2b-4fe8-8627-419e2b25667f)
![Screenshot 2024-06-15 051258](https://github.com/Matteo2979/Volcano-Analysis/assets/105907530/98c26929-64c4-4b1e-89b0-21bfde0bb1db)

# Creating a Composite Index Score in order to scale each Volcano based on its characteristics.

![Screenshot 2024-06-15 051556](https://github.com/Matteo2979/Volcano-Analysis/assets/105907530/64ed3bd9-86bc-4490-8750-b97b11d18133)

# Maps created with Foursquare Studio 
- Volcano Heatmap weighted by a Composite Index Score, which ranks the danger of each volcano based on several factors. 
  
![new_2024-06-28](https://github.com/Matteo2979/Volcano-Analysis/assets/105907530/57eb489c-6592-46ac-bd46-7b55635ba2ab)

- Heatmap of the Volcanoes Composite Index Score and the population radius within 30 kilometers.

![Designer](https://github.com/Matteo2979/Volcano-Analysis/assets/105907530/7d1addb0-19c9-4e96-bb44-81429cf41cf9)
![SE_asia_designer](https://github.com/Matteo2979/Volcano-Analysis/assets/105907530/c736395d-5621-41ad-aaf9-e82b2b54a9a7)
![japan_designer](https://github.com/Matteo2979/Volcano-Analysis/assets/105907530/0511a4c3-0c02-45b7-ab1b-d8c4a5c56253)
![italy_designer](https://github.com/Matteo2979/Volcano-Analysis/assets/105907530/541bb38e-f302-418e-a406-4eb247700c77)


