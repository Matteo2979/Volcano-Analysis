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

# Visualizing the Data  
- Checking for outliers and how data is distributed

![Screenshot 2024-06-29 052159](https://github.com/Matteo2979/Volcano-Analysis/assets/105907530/f1becf41-cbde-4b1a-9863-f9b546f1ecfd)
![all_boxplot2](https://github.com/Matteo2979/Volcano-Analysis/assets/105907530/ad322677-cd31-437c-a9d0-49d89eed2e3f)
![Screenshot 2024-06-29 052701](https://github.com/Matteo2979/Volcano-Analysis/assets/105907530/4fdc678b-b0cb-41c4-8497-d84e9344a948)
![histplot_vei](https://github.com/Matteo2979/Volcano-Analysis/assets/105907530/0ce499c5-0ac8-4ca4-a9fc-fa9270d29487)
![Screenshot 2024-07-01 012947](https://github.com/Matteo2979/Volcano-Analysis/assets/105907530/e64f1529-7355-413f-ae3d-5e4289e5ea05)
![violin_plot](https://github.com/Matteo2979/Volcano-Analysis/assets/105907530/4c9a7c8f-d816-4e37-8435-a6d374d3d8f8)

- Scatter plot with regression line, showing that Volcanoes with a larger Max VEI resulted in more deaths.
  
![Screenshot 2024-06-29 052958](https://github.com/Matteo2979/Volcano-Analysis/assets/105907530/d2ce8744-052c-45a7-9df9-bbd06fc1f137)
![seaborn_vei_deaths2](https://github.com/Matteo2979/Volcano-Analysis/assets/105907530/f6d057b6-0667-4d1a-a014-2b5e64e551d0)

- Donut Chart showing Total Deaths per Region (19 total Regions)
  
![donut](https://github.com/Matteo2979/Volcano-Analysis/assets/105907530/bfa3653b-6231-4353-a311-5bb551fa213b)

- Table showing which countries have the most Volcanes, number of eruptions, population radius within 5 and 10km, and the total amount of deaths caused by Volcanoes.

![Screenshot 2024-06-15 043038](https://github.com/Matteo2979/Volcano-Analysis/assets/105907530/0752c1fd-1c09-4a90-a79d-b29111ae96ba)

- Bubble chart highliting the regions with the most eruptions, largest populations, and largest number of Volcanoes.

![Screenshot 2024-06-15 044142](https://github.com/Matteo2979/Volcano-Analysis/assets/105907530/ab85b2f2-9653-4cd3-9a71-cb771bdbbb9c)
![bubblechart_region](https://github.com/Matteo2979/Volcano-Analysis/assets/105907530/f098df99-77f3-4958-90b1-d2fa3b8a2699)

- Checking for correlations

![Screenshot 2024-06-15 050219](https://github.com/Matteo2979/Volcano-Analysis/assets/105907530/c3d75e99-6b6d-40be-a78c-065f4a522344)
![new_matrix](https://github.com/Matteo2979/Volcano-Analysis/assets/105907530/ffc0ec40-4d2b-4fe8-8627-419e2b25667f)
![Screenshot 2024-06-15 051258](https://github.com/Matteo2979/Volcano-Analysis/assets/105907530/98c26929-64c4-4b1e-89b0-21bfde0bb1db)

- Creating a Composite Index Score in order to scale each Volcano based on its characteristics.

![Screenshot 2024-06-15 051556](https://github.com/Matteo2979/Volcano-Analysis/assets/105907530/64ed3bd9-86bc-4490-8750-b97b11d18133)

# Maps created with Foursquare Studio 
- Volcano Heatmap weighted by a Composite Index Score, which ranks the danger of each volcano based on several factors. 
  
![new_2024-06-28](https://github.com/Matteo2979/Volcano-Analysis/assets/105907530/57eb489c-6592-46ac-bd46-7b55635ba2ab)

- Heatmap of the Volcanoes Composite Index Score and the population radius within 30 kilometers.

![Designer](https://github.com/Matteo2979/Volcano-Analysis/assets/105907530/7d1addb0-19c9-4e96-bb44-81429cf41cf9)
![SE_asia_designer](https://github.com/Matteo2979/Volcano-Analysis/assets/105907530/c736395d-5621-41ad-aaf9-e82b2b54a9a7)
![japan_designer](https://github.com/Matteo2979/Volcano-Analysis/assets/105907530/0511a4c3-0c02-45b7-ab1b-d8c4a5c56253)
![italy_designer](https://github.com/Matteo2979/Volcano-Analysis/assets/105907530/541bb38e-f302-418e-a406-4eb247700c77)


