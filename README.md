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

- The 'eruption_data' contains data from each individual confirmed eruption within the past 10,000 years, including its Volcanic Explosivity Index (VEI) rating, as well as the Start/ End Dates (when known).
    - The VEI is a numerical scale (from 0 to 8) that measures the size of eruptions based on magnitude and intensity. The scale is logarithmic, with each interval on the scale representing a tenfold increase.
    ![nps_GOV_vei_diagram](https://github.com/Matteo2979/Volcano-Analysis/assets/105907530/2d7b5e2d-f870-4f8d-babf-b68884fd75ab)

        Image Source: https://volcanoes.usgs.gov/vsc/images/image_mngr/4800-4899/img4803_500w_836h.png

```python
data_table.DataTable(eruption_data)
```
![Screenshot 2024-07-02 040755](https://github.com/Matteo2979/Volcano-Analysis/assets/105907530/89ccb1b0-81bc-4c2c-92d0-a778a7f7d9fe)



- Violin Plot showing the distribution of each eruptions VEI rating

```python
sns.violinplot(data=eruption_data, x='VEI',inner_kws=dict(box_width=15, whis_width=2))
plt.title('Distribution of VEI', fontsize=10)
plt.tight_layout()
```
![eruption_vei_violin](https://github.com/Matteo2979/Volcano-Analysis/assets/105907530/dadd8e2b-ede9-4603-8cb2-9ba9012dfbc6)

- Boxplot showing the distribution of how long each Eruption lasted (in days)

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


- Scatter plot showing the Total amount of death per type of Volcano

```python
sns.scatterplot(data=volcano_sparrow, x='Total Deaths', y='Type')
plt.tight_layout()
```
![scatter_sparrow](https://github.com/Matteo2979/Volcano-Analysis/assets/105907530/ebb8f5f6-cf32-4757-a07f-3001c13e659f)


- Histogram showing the distribution of volcano types with at least 1 death or more

```python
sns.histplot(volcano_sparrow['Type'], bins=8)

plt.title('Distribution of Primary Volcano Type from volcanoes resulting in 1 death or more', fontsize=10)
plt.xticks(rotation=90, ha='center', fontsize=6)
plt.tight_layout()
```
![type_hist_sparrow](https://github.com/Matteo2979/Volcano-Analysis/assets/105907530/bacdfc02-2fbf-4f06-8933-da96e807160d)


- Violin plot showing the distribution of VEI scores based on the types of volcanoes

```python
sns.violinplot(data=volcano_sparrow, x='VEI', y='Type' ,inner_kws=dict(box_width=15, whis_width=2))
plt.tight_layout()
```
![violin_type_sparrow](https://github.com/Matteo2979/Volcano-Analysis/assets/105907530/f2d79d9d-dc43-416c-805c-80856bea5618)

- Line graph showing the death tolls from volcanoes over time 
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

- Checking for outliers and how population is distributed

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


- Aggregating data by Countries to show which have the most Volcanes, and show their corresponding attributes.

```python
df_merged = df_complete.groupby('Country')['Volcano Name'].count().reset_index(name='Number of Volcanoes')

df_5km = df_complete.groupby('Country')['Population within 5 km'].sum().reset_index()
df_10km = df_complete.groupby('Country')['Pop 10km'].sum().reset_index()
df_deaths = df_complete.groupby('Country')['Total Deaths'].sum().reset_index()
df_erup = df_complete.groupby('Country')['Eruption Count'].sum().reset_index()

df_merged = df_merged.merge(df_erup, on='Country', how='left')

df_merged['Eruptions Per Volcano'] = df_merged['Number of Volcanoes'] / df_complete['Eruption Count']
df_merged['Eruptions Per Volcano'] = df_merged['Eruptions Per Volcano'].round(2)

df_merged = df_merged.merge(df_deaths, on='Country', how='left')
df_merged = df_merged.merge(df_5km, on='Country', how='left')
df_merged = df_merged.merge(df_10km, on='Country', how='left')

df_merged.sort_values(by=['Number of Volcanoes'], ascending=False).head(15)
```
![Screenshot 2024-07-03 024301](https://github.com/Matteo2979/Volcano-Analysis/assets/105907530/3f0a5b8a-3934-4846-b9d5-92680f817e20)


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


- Scatter plot with regression line, showing that volcanoes with a larger 'Max VEI', have larger death tolls per single eruption event.
  
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


# Checking for correlations

- To numerize all columns for correlation matrix (changing categorical values as numerical values).

```python
df_integer = df_complete

for col_name in df_integer.columns:
  if(df_integer[col_name].dtype == 'object'):
    df_integer[col_name] = df_integer[col_name].astype('category')
    df_integer[col_name] = df_integer[col_name].cat.codes

df_integer
```

```python
df_integer.corr(method='spearman')

corr_matrix = df_integer.corr(method='spearman')

sns.heatmap(corr_matrix, annot=True,  fmt='.1f', annot_kws={'fontsize': 7.5}, linewidths= 1, square= False)

plt.title('Correlation Matrix for all Features')

plt.xlabel('Volcano Features')

plt.ylabel('Volcano Features')

plt.rcParams['xtick.labelsize'] = 10
plt.rcParams['ytick.labelsize'] = 10

plt.show()
```
![matrix_spearman](https://github.com/Matteo2979/Volcano-Analysis/assets/105907530/8a63fc23-03c7-416c-afd1-61d199714f58)


# Creating a Composite Index Score in order to scale each Volcano based on its characteristics.

```python
# List of variables to be included in the composite index
variables = ['AVG VEI', 'MAX VEI', 'Total Deaths', 'Population within 5 km' , 'Pop 10km', 'Pop 30km', 'Pop 100 km']

# Standardize the variables
scaler = StandardScaler()
df_complete[variables] = scaler.fit_transform(df_complete[variables])

# Define weights for each variable (example weights)
weights = {
    'AVG VEI': 0.025,
    'MAX VEI': 0.07,
    'Total Deaths': 0.095,
    'Population within 5 km': 0.21,
    'Pop 10km': 0.215,
    'Pop 30km': 0.22,
    'Pop 100 km': 0.165
}

# Calculate the composite index
df_complete['composite_index'] = (
    df_complete['AVG VEI'] * weights['AVG VEI'] +
    df_complete['MAX VEI'] * weights['MAX VEI'] +
    df_complete['Total Deaths'] * weights['Total Deaths'] +
    df_complete['Population within 5 km'] * weights['Pop 10km'] +
    df_complete['Pop 30km'] * weights['Pop 30km'] +
    df_complete['Pop 100 km'] * weights['Pop 100 km']
)

df_complete['rank'] = df_complete['composite_index'].rank(ascending=False)

top_volcanoes = df_complete.sort_values(by='rank').head(166)

top_volcanoes_display = top_volcanoes[['Volcano Name', 'composite_index']]
top_volcanoes_display
```
![Screenshot 2024-07-03 025358](https://github.com/Matteo2979/Volcano-Analysis/assets/105907530/46c66849-7ec1-458d-b2ce-7e6fa99e5051)



# Maps created with Foursquare Studio 
https://studio.foursquare.com/map/public/3d03e387-9ad8-4305-a636-a0368fdfdada/embed
- Volcano Heatmap weighted by a Composite Index Score, which ranks the danger of each volcano based on several factors. 
  
![new_heatmap_world](https://github.com/Matteo2979/Volcano-Analysis/assets/105907530/5a7dbcde-f5db-42ba-a6e8-7ad1042e8389)

- Heatmap of the Volcanoes Composite Index Score and the population radius within 30 kilometers.

![foursquare_italy](https://github.com/Matteo2979/Volcano-Analysis/assets/105907530/a3357d47-82b9-45ba-8e74-588aa8a7e6fe)
![foursquare_mex](https://github.com/Matteo2979/Volcano-Analysis/assets/105907530/931c7c43-67d8-4bd1-8843-cb052f49b1de)
![foursquare_seasia](https://github.com/Matteo2979/Volcano-Analysis/assets/105907530/a9873138-b3e6-4b87-bbe0-ad32726197b6)
![foursquare_japan](https://github.com/Matteo2979/Volcano-Analysis/assets/105907530/b2b3f704-4415-401b-b861-73bbaeade9fe)




