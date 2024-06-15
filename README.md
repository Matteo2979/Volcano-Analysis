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
  
![Screenshot 2024-06-15 031613](https://github.com/Matteo2979/Volcano-Analysis/assets/105907530/f82c7bb0-1db3-4ce7-b6f6-27d73bae14ca)

- Uploading Datasets
- The main dataset for this analysis is the 'GVP_Volcano_List', which contains a list of all active Volcanoes (active within past 10,000 years) and their various characteristics, including the population radius within 5, 10, 30, and 100 Kilometers, Primary Volcano Type, and more.

![Screenshot 2024-06-15 031144](https://github.com/Matteo2979/Volcano-Analysis/assets/105907530/f862508a-a54e-4e3c-aeed-52f1c7e4563b)

- The 'eruption_data' contains data from each individual confirmed eruption within the past 10,000 years, including its VEI  (Volcanis Explosivity Index) rating, which ranges from 1-7, as well as the Start/ End Dates (when known).

- The 'NCEI_volcano_events' dataset contains a list of Volcanic eruptions that resulted in at least 1 death or more.


![Screenshot 2024-06-15 032344](https://github.com/Matteo2979/Volcano-Analysis/assets/105907530/f6d63a2a-b777-45d9-8bb4-a7ec61bfdcf2)

# Cleaning the Data
- Dropping  unneeded columns, checking for missing data, and checking datatypes

![Screenshot 2024-06-15 034522](https://github.com/Matteo2979/Volcano-Analysis/assets/105907530/cb805457-ad47-4e54-93c9-684252123119)

- Combining and merging data and creating new features.
- The new features added to the dataset include the average VEI, Max VEI, number of eruptions, and the total amount of deaths per Volcano.

![Screenshot 2024-06-15 035143](https://github.com/Matteo2979/Volcano-Analysis/assets/105907530/ad1ebc46-a7cb-447f-99c0-b0aa79877d8a)

- Checking for outliers and how data is distributed

![Screenshot 2024-06-15 044753](https://github.com/Matteo2979/Volcano-Analysis/assets/105907530/85502f5a-0122-44fb-9cf5-5fe06d155ca0)
![boxplot_erup](https://github.com/Matteo2979/Volcano-Analysis/assets/105907530/6921506a-bd3f-4431-bbe7-a822cc721037)
![boxplot_avgVEI](https://github.com/Matteo2979/Volcano-Analysis/assets/105907530/4d336b7c-b42c-43e8-9a9c-1edaed87fbfa)
![boxplot_30km](https://github.com/Matteo2979/Volcano-Analysis/assets/105907530/2575702c-27e3-4697-a749-1082ae344374)

# Visualizing the Data
- Table showing which countries have the most Volcanes, as well as the number of eruptions, population radius within 5 and 10km, and the total amount of deaths by Volcano per country.

![Screenshot 2024-06-15 043038](https://github.com/Matteo2979/Volcano-Analysis/assets/105907530/0752c1fd-1c09-4a90-a79d-b29111ae96ba)

- Bubble and donut chart highliting the regions with the most eruptions, largest populations, and largest death tolls.

![Screenshot 2024-06-15 044142](https://github.com/Matteo2979/Volcano-Analysis/assets/105907530/ab85b2f2-9653-4cd3-9a71-cb771bdbbb9c)
![bubblechart_region](https://github.com/Matteo2979/Volcano-Analysis/assets/105907530/f098df99-77f3-4958-90b1-d2fa3b8a2699)
![donut_region_deaths](https://github.com/Matteo2979/Volcano-Analysis/assets/105907530/2ca18ce1-a977-4143-8cdc-4b6c4905e878)

- Scatter plot with regression line showing that eruptions with a larger VEI resulted in more deaths.

![Screenshot 2024-06-15 050031](https://github.com/Matteo2979/Volcano-Analysis/assets/105907530/a4a871c4-b044-4c74-8d4e-7e1f8e05bcc2)
![seaborn_vei_deaths](https://github.com/Matteo2979/Volcano-Analysis/assets/105907530/d9658678-440c-433b-9da3-ee719cb85f11)

- Checking for correlations

![Screenshot 2024-06-15 050219](https://github.com/Matteo2979/Volcano-Analysis/assets/105907530/c3d75e99-6b6d-40be-a78c-065f4a522344)
![new_matrix](https://github.com/Matteo2979/Volcano-Analysis/assets/105907530/ffc0ec40-4d2b-4fe8-8627-419e2b25667f)
![Screenshot 2024-06-15 051258](https://github.com/Matteo2979/Volcano-Analysis/assets/105907530/98c26929-64c4-4b1e-89b0-21bfde0bb1db)

- Creating a Composite Index Score in order to scale each Volcano based on its characteristics

![Screenshot 2024-06-15 051556](https://github.com/Matteo2979/Volcano-Analysis/assets/105907530/64ed3bd9-86bc-4490-8750-b97b11d18133)

# Map created using Foursquare Studio to visualize the Volcanic danger zones based on the Composite Index Score, as well as the sum of their population radius within 30 kilometers.
  
![comp_index_map](https://github.com/Matteo2979/Volcano-Analysis/assets/105907530/b94d7ad7-19be-4514-b039-7884f9f419c6)
![pop_30_index_map](https://github.com/Matteo2979/Volcano-Analysis/assets/105907530/607408ed-979a-4511-82e9-a5442d84ad6b)
