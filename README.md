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

![bubblechart_region](https://github.com/Matteo2979/Volcano-Analysis/assets/105907530/f098df99-77f3-4958-90b1-d2fa3b8a2699)

![donut_region_deaths](https://github.com/Matteo2979/Volcano-Analysis/assets/105907530/2ca18ce1-a977-4143-8cdc-4b6c4905e878)
![Screenshot 2024-06-09 052452](https://github.com/Matteo2979/Volcano-Analysis/assets/105907530/90b0581a-46d3-4e51-beaa-ec04f2222393)
![boxplot_erup](https://github.com/Matteo2979/Volcano-Analysis/assets/105907530/6921506a-bd3f-4431-bbe7-a822cc721037)
![boxplot_avgVEI](https://github.com/Matteo2979/Volcano-Analysis/assets/105907530/4d336b7c-b42c-43e8-9a9c-1edaed87fbfa)
![boxplot_10km](https://github.com/Matteo2979/Volcano-Analysis/assets/105907530/5e20a4ae-1689-421a-b3bd-58a82b83225c)
![Screenshot 2024-06-09 055345](https://github.com/Matteo2979/Volcano-Analysis/assets/105907530/06d1041b-d9b5-4dad-90bb-9b93e398cf9a)
![seaborn_vei_deaths](https://github.com/Matteo2979/Volcano-Analysis/assets/105907530/d9658678-440c-433b-9da3-ee719cb85f11)
![Screenshot 2024-06-09 054008](https://github.com/Matteo2979/Volcano-Analysis/assets/105907530/5607c9eb-60fc-43f2-aa87-035ba249d337)
![hist_volc_type](https://github.com/Matteo2979/Volcano-Analysis/assets/105907530/59523e45-4e45-4176-9275-a5a76cde2aa7)

![bubblechart_region](https://github.com/Matteo2979/Volcano-Analysis/assets/105907530/f098df99-77f3-4958-90b1-d2fa3b8a2699)
![Screenshot 2024-06-09 055836](https://github.com/Matteo2979/Volcano-Analysis/assets/105907530/212f5b0a-24df-4d86-ba70-a158ccad39da)
![matrix](https://github.com/Matteo2979/Volcano-Analysis/assets/105907530/18f476b9-8962-4c12-89ec-b8a3759352b9)
![comp_index_map](https://github.com/Matteo2979/Volcano-Analysis/assets/105907530/b94d7ad7-19be-4514-b039-7884f9f419c6)
![pop_30_index_map](https://github.com/Matteo2979/Volcano-Analysis/assets/105907530/607408ed-979a-4511-82e9-a5442d84ad6b)
