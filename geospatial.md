# Introduction 

Many people in Southwest Baltimore do not have easy access to healthcare. Some neighborhoods are far from clinics or hospitals, and others face challenges like poverty, or lack of transportation. These problems can make people sicker over time. That is why it is important to understand where these issues are happening so we can help. 
The Health and Wellness Group Project focuses on leveraging data science to address critical healthcare accessibility issues in Southwest Baltimore. In collaboration with Dr. Megan Doede, the Program Lead for Health and Wellness at Paul’s Place, our team has embarked on a mission to explore the intersection of spatial disparities, environmental stressors, and community health outcomes.  
By applying advanced analytical techniques and geospatial tools using demographic and environmental data, we aim to uncover key patterns that identify healthcare deserts and highlight areas at risk for future health concerns. Our goal is to use these insights to support data-driven, equitable interventions that can improve healthcare delivery and resource distribution for underserved communities. This will help leaders and community groups make better decisions about where to bring services and support.
In this aspect of the project, we create a geospatial loactor for homeless shelters and social welfare centers in commutable distance of Pauls place within Southwest Baltimore. The maps give a visualization of the homeless shelters within walking distance of Pauls place for community outreach purposes(link here) and aid in identifying health service deserts within Southwest Baltimore.   

# METHODOLOGY

## Data Collection

### Source of Data:

We used three main sources for our data:

#### American Community Survey (ACS) from the U.S. Census Bureau.
https://www.census.gov/programs-surveys/acs/news/data-releases/2023.html Provided information about income, education, health insurance, race, age, and more which helped us understand the community’s economic and social conditions.

#### Homeless Shelter Locations from Open Baltimore.
https://data.baltimorecity.gov/datasets/baltimore::homeless-shelters-2/explore With the use of API url to access updated data this data shows where homeless shelters are located in Baltimore.

#### Health Department Dataset.
https://data.baltimorecity.gov/datasets/e37ce649df4344dab174b34593b1c4b6_0/explore?location=39.307459%2C-76.628697%2C11.39&showTable=true Provided information about health problems in the community including data on store density (alcohol, tobacco, grocery), mortality, substance abuse, and more.

The homeless shelter geo-loactor map was hosted using stream-lit along with github. Github was used to host the text .py file and other dependencies with streamlit was used to assemble and project the map along with other features(link here).
The health and wellness service deserts map was done in jupyter notebook with html syntax and hosted in github(link here).

We employed a systematic approach to process and transform socioeconomic data in preparation for subsequent analytical procedures. The overarching goal was to derive a structured dataset suitable for in-depth viusuliaztion, potentially including spatial distribution and inference. The methodology comprised the following stages seen in code blocks:




```python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns 


```


```python
import pandas as pd
import numpy as np
data_SW_social = pd.read_csv("/Users/bayowaonabajo/Downloads/ACS  SW Balt Social/ACS 5 year SW Balt Social.csv") #dataset
#data_SW_social.head(20)
for idx, value in enumerate(data_SW_social['Label (Grouping)'], 1):
    print(f"{idx}. {value}")
```

    1. HOUSEHOLDS BY TYPE
    2.     Total households
    3.         Married-couple household
    4.             With children of the householder under 18 years
    5.         Cohabiting couple household
    6.             With children of the householder under 18 years
    7.         Male householder, no spouse/partner present
    8.             With children of the householder under 18 years
    9.             Householder living alone
    10.                 65 years and over
    11.         Female householder, no spouse/partner present
    12.             With children of the householder under 18 years
    13.             Householder living alone
    14.                 65 years and over
    15.         Households with one or more people under 18 years
    16.         Households with one or more people 65 years and over
    17.         Average household size
    18.         Average family size
    19. RELATIONSHIP
    20.     Population in households
    21.         Householder
    22.         Spouse
    23.         Unmarried partner
    24.         Child
    25.         Other relatives
    26.         Other nonrelatives
    27. MARITAL STATUS
    28.     Males 15 years and over
    29.         Never married
    30.         Now married, except separated
    31.         Separated
    32.         Widowed
    33.         Divorced
    34.     Females 15 years and over
    35.         Never married
    36.         Now married, except separated
    37.         Separated
    38.         Widowed
    39.         Divorced
    40. FERTILITY
    41.     Number of women 15 to 50 years old who had a birth in the past 12 months
    42.         Unmarried women (widowed, divorced, and never married)
    43.             Per 1,000 unmarried women
    44.         Per 1,000 women 15 to 50 years old
    45.         Per 1,000 women 15 to 19 years old
    46.         Per 1,000 women 20 to 34 years old
    47.         Per 1,000 women 35 to 50 years old
    48. GRANDPARENTS
    49.     Number of grandparents living with own grandchildren under 18 years
    50.         Grandparents responsible for grandchildren
    51.         Years responsible for grandchildren
    52.             Less than 1 year
    53.             1 or 2 years
    54.             3 or 4 years
    55.             5 or more years
    56.     Number of grandparents responsible for own grandchildren under 18 years
    57.         Who are female
    58.         Who are married
    59. SCHOOL ENROLLMENT
    60.     Population 3 years and over enrolled in school
    61.         Nursery school, preschool
    62.         Kindergarten
    63.         Elementary school (grades 1-8)
    64.         High school (grades 9-12)
    65.         College or graduate school
    66. EDUCATIONAL ATTAINMENT
    67.     Population 25 years and over
    68.         Less than 9th grade
    69.         9th to 12th grade, no diploma
    70.         High school graduate (includes equivalency)
    71.         Some college, no degree
    72.         Associate's degree
    73.         Bachelor's degree
    74.         Graduate or professional degree
    75.         High school graduate or higher
    76.         Bachelor's degree or higher
    77. VETERAN STATUS
    78.     Civilian population 18 years and over
    79.         Civilian veterans
    80. DISABILITY STATUS OF THE CIVILIAN NONINSTITUTIONALIZED POPULATION
    81.     Total Civilian Noninstitutionalized Population
    82.         With a disability
    83.     Under 18 years
    84.         With a disability
    85.     18 to 64 years
    86.         With a disability
    87.     65 years and over
    88.         With a disability
    89. RESIDENCE 1 YEAR AGO
    90.     Population 1 year and over
    91.         Same house
    92.         Different house (in the U.S. or abroad)
    93.             Different house in the U.S.
    94.                 Same county
    95.                 Different county
    96.                     Same state
    97.                     Different state
    98.             Abroad
    99. PLACE OF BIRTH
    100.     Total population
    101.         Native
    102.             Born in United States
    103.                 State of residence
    104.                 Different state
    105.             Born in Puerto Rico, U.S. Island areas, or born abroad to American parent(s)
    106.         Foreign-born
    107. U.S. CITIZENSHIP STATUS
    108.     Foreign-born population
    109.         Naturalized U.S. citizen
    110.         Not a U.S. citizen
    111. YEAR OF ENTRY
    112.     Population born outside the United States
    113.         Native
    114.             Entered 2010 or later
    115.             Entered before 2010
    116.         Foreign-born
    117.             Entered 2010 or later
    118.             Entered before 2010
    119. WORLD REGION OF BIRTH OF FOREIGN-BORN
    120.     Foreign-born population, excluding population born at sea
    121.         Europe
    122.         Asia
    123.         Africa
    124.         Oceania
    125.         Latin America
    126.         Northern America
    127. LANGUAGE SPOKEN AT HOME
    128.     Population 5 years and over
    129.         English only
    130.         Language other than English
    131.             Speak English less than "very well"
    132.         Spanish
    133.             Speak English less than "very well"
    134.         Other Indo-European languages
    135.             Speak English less than "very well"
    136.         Asian and Pacific Islander languages
    137.             Speak English less than "very well"
    138.         Other languages
    139.             Speak English less than "very well"
    140. ANCESTRY
    141.     Total population
    142.         American
    143.         Arab
    144.         Czech
    145.         Danish
    146.         Dutch
    147.         English
    148.         French (except Basque)
    149.         French Canadian
    150.         German
    151.         Greek
    152.         Hungarian
    153.         Irish
    154.         Italian
    155.         Lithuanian
    156.         Norwegian
    157.         Polish
    158.         Portuguese
    159.         Russian
    160.         Scotch-Irish
    161.         Scottish
    162.         Slovak
    163.         Subsaharan African
    164.         Swedish
    165.         Swiss
    166.         Ukrainian
    167.         Welsh
    168.         West Indian (excluding Hispanic origin groups)
    169. COMPUTERS AND INTERNET USE
    170.     Total households
    171.         With a computer
    172.         With a broadband Internet subscription


### Data Exploration

We looked at the data to see what it tells us about the people living in Southwest Baltimore. Missing values, outliers, and important patterns in the Data were done alongside the predictive model as seen in the health risk predictive model documentation (link here). This helped us understand the quality of the data and what we needed to extract or fix before usage.


```python
 data_SW_social.iloc[64:76]
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Label (Grouping)</th>
      <th>ZCTA5 21207!!Estimate</th>
      <th>ZCTA5 21207!!Margin of Error</th>
      <th>ZCTA5 21207!!Percent</th>
      <th>ZCTA5 21207!!Percent Margin of Error</th>
      <th>ZCTA5 21216!!Estimate</th>
      <th>ZCTA5 21216!!Margin of Error</th>
      <th>ZCTA5 21216!!Percent</th>
      <th>ZCTA5 21216!!Percent Margin of Error</th>
      <th>ZCTA5 21223!!Estimate</th>
      <th>...</th>
      <th>ZCTA5 21223!!Percent</th>
      <th>ZCTA5 21223!!Percent Margin of Error</th>
      <th>ZCTA5 21229!!Estimate</th>
      <th>ZCTA5 21229!!Margin of Error</th>
      <th>ZCTA5 21229!!Percent</th>
      <th>ZCTA5 21229!!Percent Margin of Error</th>
      <th>ZCTA5 21230!!Estimate</th>
      <th>ZCTA5 21230!!Margin of Error</th>
      <th>ZCTA5 21230!!Percent</th>
      <th>ZCTA5 21230!!Percent Margin of Error</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>64</th>
      <td>College or graduate school</td>
      <td>4,099</td>
      <td>±663</td>
      <td>34.4%</td>
      <td>±4.4</td>
      <td>1,867</td>
      <td>±321</td>
      <td>31.2%</td>
      <td>±6.2</td>
      <td>992</td>
      <td>...</td>
      <td>22.8%</td>
      <td>±8.8</td>
      <td>2,150</td>
      <td>±512</td>
      <td>21.2%</td>
      <td>±4.2</td>
      <td>2,225</td>
      <td>±322</td>
      <td>32.3%</td>
      <td>±5.2</td>
    </tr>
    <tr>
      <th>65</th>
      <td>EDUCATIONAL ATTAINMENT</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>66</th>
      <td>Population 25 years and over</td>
      <td>34,532</td>
      <td>±1,464</td>
      <td>34,532</td>
      <td>(X)</td>
      <td>19,063</td>
      <td>±1,095</td>
      <td>19,063</td>
      <td>(X)</td>
      <td>13,870</td>
      <td>...</td>
      <td>13,870</td>
      <td>(X)</td>
      <td>30,810</td>
      <td>±1,471</td>
      <td>30,810</td>
      <td>(X)</td>
      <td>24,865</td>
      <td>±1,185</td>
      <td>24,865</td>
      <td>(X)</td>
    </tr>
    <tr>
      <th>67</th>
      <td>Less than 9th grade</td>
      <td>1,007</td>
      <td>±289</td>
      <td>2.9%</td>
      <td>±0.8</td>
      <td>758</td>
      <td>±238</td>
      <td>4.0%</td>
      <td>±1.2</td>
      <td>1,001</td>
      <td>...</td>
      <td>7.2%</td>
      <td>±2.0</td>
      <td>995</td>
      <td>±274</td>
      <td>3.2%</td>
      <td>±0.9</td>
      <td>1,130</td>
      <td>±314</td>
      <td>4.5%</td>
      <td>±1.2</td>
    </tr>
    <tr>
      <th>68</th>
      <td>9th to 12th grade, no diploma</td>
      <td>2,619</td>
      <td>±573</td>
      <td>7.6%</td>
      <td>±1.5</td>
      <td>1,735</td>
      <td>±373</td>
      <td>9.1%</td>
      <td>±1.9</td>
      <td>2,435</td>
      <td>...</td>
      <td>17.6%</td>
      <td>±3.3</td>
      <td>2,467</td>
      <td>±359</td>
      <td>8.0%</td>
      <td>±1.2</td>
      <td>1,809</td>
      <td>±437</td>
      <td>7.3%</td>
      <td>±1.7</td>
    </tr>
    <tr>
      <th>69</th>
      <td>High school graduate (includes equival...</td>
      <td>9,940</td>
      <td>±983</td>
      <td>28.8%</td>
      <td>±2.3</td>
      <td>7,732</td>
      <td>±835</td>
      <td>40.6%</td>
      <td>±3.6</td>
      <td>5,411</td>
      <td>...</td>
      <td>39.0%</td>
      <td>±3.0</td>
      <td>11,185</td>
      <td>±1,271</td>
      <td>36.3%</td>
      <td>±3.0</td>
      <td>3,397</td>
      <td>±439</td>
      <td>13.7%</td>
      <td>±1.7</td>
    </tr>
    <tr>
      <th>70</th>
      <td>Some college, no degree</td>
      <td>7,500</td>
      <td>±713</td>
      <td>21.7%</td>
      <td>±1.8</td>
      <td>4,887</td>
      <td>±597</td>
      <td>25.6%</td>
      <td>±3.0</td>
      <td>2,894</td>
      <td>...</td>
      <td>20.9%</td>
      <td>±3.4</td>
      <td>7,104</td>
      <td>±632</td>
      <td>23.1%</td>
      <td>±2.0</td>
      <td>3,070</td>
      <td>±463</td>
      <td>12.3%</td>
      <td>±1.8</td>
    </tr>
    <tr>
      <th>71</th>
      <td>Associate's degree</td>
      <td>2,825</td>
      <td>±524</td>
      <td>8.2%</td>
      <td>±1.5</td>
      <td>846</td>
      <td>±228</td>
      <td>4.4%</td>
      <td>±1.2</td>
      <td>412</td>
      <td>...</td>
      <td>3.0%</td>
      <td>±1.2</td>
      <td>2,213</td>
      <td>±394</td>
      <td>7.2%</td>
      <td>±1.3</td>
      <td>827</td>
      <td>±213</td>
      <td>3.3%</td>
      <td>±0.9</td>
    </tr>
    <tr>
      <th>72</th>
      <td>Bachelor's degree</td>
      <td>6,022</td>
      <td>±753</td>
      <td>17.4%</td>
      <td>±2.3</td>
      <td>1,770</td>
      <td>±397</td>
      <td>9.3%</td>
      <td>±2.0</td>
      <td>872</td>
      <td>...</td>
      <td>6.3%</td>
      <td>±1.5</td>
      <td>4,328</td>
      <td>±582</td>
      <td>14.0%</td>
      <td>±1.9</td>
      <td>7,843</td>
      <td>±602</td>
      <td>31.5%</td>
      <td>±2.0</td>
    </tr>
    <tr>
      <th>73</th>
      <td>Graduate or professional degree</td>
      <td>4,619</td>
      <td>±548</td>
      <td>13.4%</td>
      <td>±1.6</td>
      <td>1,335</td>
      <td>±446</td>
      <td>7.0%</td>
      <td>±2.3</td>
      <td>845</td>
      <td>...</td>
      <td>6.1%</td>
      <td>±1.4</td>
      <td>2,518</td>
      <td>±365</td>
      <td>8.2%</td>
      <td>±1.1</td>
      <td>6,789</td>
      <td>±618</td>
      <td>27.3%</td>
      <td>±2.0</td>
    </tr>
    <tr>
      <th>74</th>
      <td>High school graduate or higher</td>
      <td>30,906</td>
      <td>±1,247</td>
      <td>89.5%</td>
      <td>±1.6</td>
      <td>16,570</td>
      <td>±988</td>
      <td>86.9%</td>
      <td>±2.0</td>
      <td>10,434</td>
      <td>...</td>
      <td>75.2%</td>
      <td>±3.8</td>
      <td>27,348</td>
      <td>±1,479</td>
      <td>88.8%</td>
      <td>±1.5</td>
      <td>21,926</td>
      <td>±1,097</td>
      <td>88.2%</td>
      <td>±1.8</td>
    </tr>
    <tr>
      <th>75</th>
      <td>Bachelor's degree or higher</td>
      <td>10,641</td>
      <td>±808</td>
      <td>30.8%</td>
      <td>±2.5</td>
      <td>3,105</td>
      <td>±608</td>
      <td>16.3%</td>
      <td>±3.0</td>
      <td>1,717</td>
      <td>...</td>
      <td>12.4%</td>
      <td>±2.0</td>
      <td>6,846</td>
      <td>±706</td>
      <td>22.2%</td>
      <td>±2.3</td>
      <td>14,632</td>
      <td>±919</td>
      <td>58.8%</td>
      <td>±2.3</td>
    </tr>
  </tbody>
</table>
<p>12 rows × 21 columns</p>
</div>



### Data Cleaning and feature selection

Data cleaning process is same as discussed in predictive modelling documentation(link here). For the geo-mapping, we extracted population estimate variables for each zip code tabulation area, and created a table with longitude and latitude features for each ZCTA. To enhance clarity and facilitate subsequent data handling, the column designated as 'Label (Grouping)' was programmatically renamed to 'Metric'.


```python
import pandas as pd
import numpy as np

# Subset data
subset_df = data_SW_social.iloc[64:76].copy()

# Clean column names
clean_columns = {col: col.replace("ZCTA5 ", "").replace("!!", "_") for col in subset_df.columns}
subset_df.rename(columns=clean_columns, inplace=True)

# Melt to long format
melted_df = subset_df.melt(
    id_vars=["Label (Grouping)"], 
    var_name="ZCTA5_Metric", 
    value_name="Value"
)

# Split ZCTA5 and metric
split_vals = melted_df['ZCTA5_Metric'].str.split("_", n=1, expand=True)
melted_df['ZCTA5'] = split_vals[0]
melted_df['Metric'] = split_vals[1].str.replace("Margin", " Margin").str.strip()

# Clean label names
melted_df['Label (Grouping)'] = melted_df['Label (Grouping)'].str.replace('\xa0', ' ').str.strip()

# Enhanced cleaning
def clean_value(value):
    try:
        if pd.isna(value) or str(value).strip() in ('(X)', '...'):
            return np.nan
        value_part = str(value).split('±')[0]
        cleaned = value_part.replace(',', '').replace('%', '').strip()
        return float(cleaned) if cleaned else np.nan
    except:
        return np.nan

melted_df['Value'] = melted_df['Value'].apply(clean_value)
melted_df = melted_df.dropna(subset=['Value'])

# Updated education mapping 
education_mapping = {
    'Less than 9th grade': 'Education below high school',
    '9th to 12th grade, no diploma': 'Education below high school',
    'High school graduate (includes equivalency)': 'High school to grad/professional',
    'Some college, no degree': 'High school to grad/professional',
    "Associate's degree": 'High school to grad/professional',
    "Bachelor's degree": 'High school to grad/professional',
    'Graduate or professional degree': 'High school to grad/professional',
    'High school graduate or higher': 'High school to grad/professional',
    "Bachelor's degree or higher": 'High school to grad/professional'
}

# Filter and map
melted_df = melted_df[melted_df['Label (Grouping)'].isin(education_mapping.keys())]
melted_df['Education Category'] = melted_df['Label (Grouping)'].map(education_mapping)

# Pivot table
final_table = melted_df.pivot_table(
    index=['ZCTA5', 'Education Category'],
    columns='Metric',
    values='Value',
    aggfunc='sum',
    fill_value=0
).reset_index()

# Ensure columns needed
required_columns = ['Estimate', 'Margin of Error', 'Percent', 'Percent Margin of Error']
for col in required_columns:
    if col not in final_table.columns:
        final_table[col] = 0

# Final format
final_table_soc = final_table[['ZCTA5', 'Education Category'] + required_columns]
final_table_soc[['Estimate', 'Margin of Error']] = final_table[['Estimate', 'Margin of Error']].astype(int)
final_table_soc[['Percent', 'Percent Margin of Error']] = final_table[['Percent', 'Percent Margin of Error']].round(1)

final_table_soc.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th>Metric</th>
      <th>ZCTA5</th>
      <th>Education Category</th>
      <th>Estimate</th>
      <th>Margin of Error</th>
      <th>Percent</th>
      <th>Percent Margin of Error</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>21207</td>
      <td>Education below high school</td>
      <td>3626</td>
      <td>0</td>
      <td>10.5</td>
      <td>0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>21207</td>
      <td>High school to grad/professional</td>
      <td>72453</td>
      <td>0</td>
      <td>209.8</td>
      <td>0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>21216</td>
      <td>Education below high school</td>
      <td>2493</td>
      <td>0</td>
      <td>13.1</td>
      <td>0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>21216</td>
      <td>High school to grad/professional</td>
      <td>36245</td>
      <td>0</td>
      <td>190.1</td>
      <td>0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>21223</td>
      <td>Education below high school</td>
      <td>3436</td>
      <td>0</td>
      <td>24.8</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
</div>




```python
import geopandas as gpd

#data path 
shapefile_path = "/Users/bayowaonabajo/Downloads/tl_2023_us_zcta520/tl_2023_us_zcta520.shp"

# Load the shapefile
zcta = gpd.read_file(shapefile_path)

# Check 
print(zcta.head())
```

      ZCTA5CE20 GEOID20       GEOIDFQ20 CLASSFP20 MTFCC20 FUNCSTAT20  ALAND20  \
    0     47236   47236  860Z200US47236        B5   G6350          S  1029063   
    1     47870   47870  860Z200US47870        B5   G6350          S     8830   
    2     47851   47851  860Z200US47851        B5   G6350          S    53326   
    3     47337   47337  860Z200US47337        B5   G6350          S   303089   
    4     47435   47435  860Z200US47435        B5   G6350          S    13302   
    
       AWATER20   INTPTLAT20    INTPTLON20  \
    0         0  +39.1517426  -085.7252769   
    1         0  +39.3701518  -087.4735141   
    2         0  +39.5735839  -087.2459559   
    3         0  +39.8027537  -085.4372850   
    4         0  +39.2657557  -086.2951577   
    
                                                geometry  
    0  POLYGON ((-85.7341 39.15597, -85.72794 39.1561...  
    1  POLYGON ((-87.47414 39.37016, -87.47409 39.370...  
    2  POLYGON ((-87.24769 39.5745, -87.24711 39.5744...  
    3  POLYGON ((-85.44357 39.80328, -85.44346 39.803...  
    4  POLYGON ((-86.29592 39.26547, -86.29592 39.266...  



```python
import geopandas as gpd
import pandas as pd

# 1. Load the shapefile 
zcta = gpd.read_file("/Users/bayowaonabajo/Downloads/tl_2023_us_zcta520/tl_2023_us_zcta520.shp")

# 2. Convert to latitude/longitude (WGS84)
zcta = zcta.to_crs(epsg=4326)

# 3. Calculate centroid coordinates
zcta["longitude"] = zcta.geometry.centroid.x
zcta["latitude"] = zcta.geometry.centroid.y

# 4. Rename ZCTA5 column 
zcta = zcta.rename(columns={"ZCTA5CE20": "ZCTA5"})

# 5. Merge 
final_table_social = pd.merge(
    final_table_soc,
    zcta[["ZCTA5", "longitude", "latitude"]],
    on="ZCTA5",
    how="left"
)

# display
final_table_social.head()

import warnings
warnings.filterwarnings('ignore')
```

    /var/folders/z0/vng18cmj41x80kcsp_9tylwr0000gn/T/ipykernel_1802/3911949409.py:11: UserWarning: Geometry is in a geographic CRS. Results from 'centroid' are likely incorrect. Use 'GeoSeries.to_crs()' to re-project geometries to a projected CRS before this operation.
    
      zcta["longitude"] = zcta.geometry.centroid.x
    /var/folders/z0/vng18cmj41x80kcsp_9tylwr0000gn/T/ipykernel_1802/3911949409.py:12: UserWarning: Geometry is in a geographic CRS. Results from 'centroid' are likely incorrect. Use 'GeoSeries.to_crs()' to re-project geometries to a projected CRS before this operation.
    
      zcta["latitude"] = zcta.geometry.centroid.y



```python
import pandas as pd
import numpy as np

data_SW_economic = pd.read_csv("/Users/bayowaonabajo/Downloads/ACS SW Balt Economic Data/ACS 5 year SW Balt Economic.csv")
# Subset the economic data
subset_df2 = data_SW_economic.iloc[1:18].copy()

# Clean column names
clean_columns = {col: col.replace("ZCTA5 ", "").replace("!!", "_") for col in subset_df2.columns}
subset_df2.rename(columns=clean_columns, inplace=True)

# Melt to long format
melted_df2 = subset_df2.melt(
    id_vars=["Label (Grouping)"], 
    var_name="ZCTA5_Metric", 
    value_name="Value"
)

# Split ZCTA5 and metric
split_vals = melted_df2['ZCTA5_Metric'].str.split("_", n=1, expand=True)
melted_df2['ZCTA5'] = split_vals[0]
melted_df2['Metric'] = split_vals[1].str.replace("Margin", " Margin").str.strip()

# Clean label names
melted_df2['Label (Grouping)'] = melted_df2['Label (Grouping)'].str.replace('\xa0', ' ').str.strip()

# Modified cleaning
def clean_value(value):
    try:
        if pd.isna(value) or str(value).strip() in ('(X)', '...'):
            return np.nan  # Preserve NaN
        value_part = str(value).split('±')[0]
        cleaned = value_part.replace(',', '').replace('%', '').strip()
        return float(cleaned) if cleaned else np.nan
    except:
        return np.nan

melted_df2['Value'] = melted_df2['Value'].apply(clean_value)

# Filter
population_df = melted_df2[
    (melted_df2['Label (Grouping)'] == 'Population 16 years and over') &
    (melted_df2['Metric'].isin(['Estimate', 'Margin of Error']))
]

# Pivot table with fill_value=0
final_table2 = population_df.pivot_table(
    index=['ZCTA5', 'Label (Grouping)'],
    columns='Metric',
    values='Value',
    aggfunc='sum',
    fill_value=0
).reset_index()

# Ensure required columns exist
required_columns = ['Estimate', 'Margin of Error']
for col in required_columns:
    if col not in final_table2.columns:
        final_table2[col] = 0

# Create explicit copy
final_table_eco2 = final_table2.loc[:, ['ZCTA5', 'Label (Grouping)'] + required_columns].copy()

# Format numeric values
final_table_eco2[['Estimate', 'Margin of Error']] = final_table_eco2[['Estimate', 'Margin of Error']].astype(int)

print("Population 16+ by ZIP Code:")
final_table_eco2.head()
```

    Population 16+ by ZIP Code:





<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th>Metric</th>
      <th>ZCTA5</th>
      <th>Label (Grouping)</th>
      <th>Estimate</th>
      <th>Margin of Error</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>21207</td>
      <td>Population 16 years and over</td>
      <td>39670</td>
      <td>0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>21216</td>
      <td>Population 16 years and over</td>
      <td>21640</td>
      <td>0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>21223</td>
      <td>Population 16 years and over</td>
      <td>16062</td>
      <td>0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>21229</td>
      <td>Population 16 years and over</td>
      <td>35575</td>
      <td>0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>21230</td>
      <td>Population 16 years and over</td>
      <td>27466</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
</div>



#### Data Engineering

We merged datasets using zip code tabulation areas and for a data table that could be used for spatial mapping on longitude and latitude.


```python
import geopandas as gpd
import pandas as pd

#  Load the shapefile 
zcta = gpd.read_file("/Users/bayowaonabajo/Downloads/tl_2023_us_zcta520/tl_2023_us_zcta520.shp")

# Convert to latitude/longitude (WGS84)
zcta = zcta.to_crs(epsg=4326)

#  Calculate centroid coordinates
zcta["longitude"] = zcta.geometry.centroid.x
zcta["latitude"] = zcta.geometry.centroid.y

# Rename ZCTA5 column to match your dataset
zcta = zcta.rename(columns={"ZCTA5CE20": "ZCTA5"})

#  Merge with DataFrame
final_table_economic2 = pd.merge(
    final_table_eco2,
    zcta[["ZCTA5", "longitude", "latitude"]],
    on="ZCTA5",
    how="left"
)

# display 
final_table_economic2.head()

import warnings
warnings.filterwarnings('ignore')
```


```python
import pandas as pd
data_SW_demographic = pd.read_csv("/Users/bayowaonabajo/Downloads/ACS SW Balt Demographics/ACS 5 year SW Balt Demographic.csv") #add dataset
#data_SW_demographic.head(11)
for idx, value in enumerate(data_SW_demographic['Label (Grouping)'], 1):
    print(f"{idx}. {value}")
```

    1. SEX AND AGE
    2.     Total population
    3.         Male
    4.         Female
    5.         Sex ratio (males per 100 females)
    6.         Under 5 years
    7.         5 to 9 years
    8.         10 to 14 years
    9.         15 to 19 years
    10.         20 to 24 years
    11.         25 to 34 years
    12.         35 to 44 years
    13.         45 to 54 years
    14.         55 to 59 years
    15.         60 to 64 years
    16.         65 to 74 years
    17.         75 to 84 years
    18.         85 years and over
    19.         Median age (years)
    20.         Under 18 years
    21.         16 years and over
    22.         18 years and over
    23.         21 years and over
    24.         62 years and over
    25.         65 years and over
    26.         18 years and over
    27.             Male
    28.             Female
    29.             Sex ratio (males per 100 females)
    30.         65 years and over
    31.             Male
    32.             Female
    33.             Sex ratio (males per 100 females)
    34. RACE
    35.     Total population
    36.         One race
    37.         Two or More Races
    38.         One race
    39.             White
    40.             Black or African American
    41.             American Indian and Alaska Native
    42.                 Aztec
    43.                 Blackfeet Tribe of the Blackfeet Indian Reservation of Montana
    44.                 Maya
    45.                 Native Village of Barrow Inupiat Traditional Government
    46.                 Navajo Nation
    47.                 Nome Eskimo Community
    48.                 Other American Indian and Alaska Native
    49.             Asian
    50.                 Asian Indian
    51.                 Chinese
    52.                 Filipino
    53.                 Japanese
    54.                 Korean
    55.                 Vietnamese
    56.                 Other Asian
    57.             Native Hawaiian and Other Pacific Islander
    58.                 Chamorro
    59.                 Native Hawaiian
    60.                 Samoan
    61.                 Other Native Hawaiian and Other Pacific Islander
    62.             Some Other Race
    63.         Two or More Races
    64.             White and Black or African American
    65.             White and American Indian and Alaska Native
    66.             White and Asian
    67.             White and Some Other Race
    68.             Black or African American and American Indian and Alaska Native
    69.             Black or African American and Some Other Race
    70. Race alone or in combination with one or more other races
    71.     Total population
    72.         White
    73.         Black or African American
    74.         American Indian and Alaska Native
    75.         Asian
    76.         Native Hawaiian and Other Pacific Islander
    77.         Some Other Race
    78. HISPANIC OR LATINO AND RACE
    79.     Total population
    80.         Hispanic or Latino (of any race)
    81.             Mexican
    82.             Puerto Rican
    83.             Cuban
    84.             Other Hispanic or Latino
    85.         Not Hispanic or Latino
    86.             White alone
    87.             Black or African American alone
    88.             American Indian and Alaska Native alone
    89.             Asian alone
    90.             Native Hawaiian and Other Pacific Islander alone
    91.             Some Other Race alone
    92.             Two or More Races
    93.                 Two races including Some Other Race
    94.                 Two races excluding Some Other Race, and three or more races
    95. Total housing units
    96. CITIZEN, VOTING AGE POPULATION
    97.     Citizen, 18 and over population
    98.         Male
    99.         Female



```python
import pandas as pd
data_SW_economic = pd.read_csv("/Users/bayowaonabajo/Downloads/ACS SW Balt Economic Data/ACS 5 year SW Balt Economic.csv") #add dataset
print(data_SW_economic.columns.tolist())

```

    ['Label (Grouping)', 'ZCTA5 21207!!Estimate', 'ZCTA5 21207!!Margin of Error', 'ZCTA5 21207!!Percent', 'ZCTA5 21207!!Percent Margin of Error', 'ZCTA5 21216!!Estimate', 'ZCTA5 21216!!Margin of Error', 'ZCTA5 21216!!Percent', 'ZCTA5 21216!!Percent Margin of Error', 'ZCTA5 21223!!Estimate', 'ZCTA5 21223!!Margin of Error', 'ZCTA5 21223!!Percent', 'ZCTA5 21223!!Percent Margin of Error', 'ZCTA5 21229!!Estimate', 'ZCTA5 21229!!Margin of Error', 'ZCTA5 21229!!Percent', 'ZCTA5 21229!!Percent Margin of Error', 'ZCTA5 21230!!Estimate', 'ZCTA5 21230!!Margin of Error', 'ZCTA5 21230!!Percent', 'ZCTA5 21230!!Percent Margin of Error']



```python
for idx, value in enumerate(data_SW_economic['Label (Grouping)'], 1):
    print(f"{idx}. {value}")
```

    1. EMPLOYMENT STATUS
    2.     Population 16 years and over
    3.         In labor force
    4.             Civilian labor force
    5.                 Employed
    6.                 Unemployed
    7.             Armed Forces
    8.         Not in labor force
    9.     Civilian labor force
    10.         Unemployment Rate
    11.     Females 16 years and over
    12.         In labor force
    13.             Civilian labor force
    14.                 Employed
    15.     Own children of the householder under 6 years
    16.         All parents in family in labor force
    17.     Own children of the householder 6 to 17 years
    18.         All parents in family in labor force
    19. COMMUTING TO WORK
    20.     Workers 16 years and over
    21.         Car, truck, or van -- drove alone
    22.         Car, truck, or van -- carpooled
    23.         Public transportation (excluding taxicab)
    24.         Walked
    25.         Other means
    26.         Worked from home
    27.         Mean travel time to work (minutes)
    28. OCCUPATION
    29.     Civilian employed population 16 years and over
    30.         Management, business, science, and arts occupations
    31.         Service occupations
    32.         Sales and office occupations
    33.         Natural resources, construction, and maintenance occupations
    34.         Production, transportation, and material moving occupations
    35. INDUSTRY
    36.     Civilian employed population 16 years and over
    37.         Agriculture, forestry, fishing and hunting, and mining
    38.         Construction
    39.         Manufacturing
    40.         Wholesale trade
    41.         Retail trade
    42.         Transportation and warehousing, and utilities
    43.         Information
    44.         Finance and insurance, and real estate and rental and leasing
    45.         Professional, scientific, and management, and administrative and waste management services
    46.         Educational services, and health care and social assistance
    47.         Arts, entertainment, and recreation, and accommodation and food services
    48.         Other services, except public administration
    49.         Public administration
    50. CLASS OF WORKER
    51.     Civilian employed population 16 years and over
    52.         Private wage and salary workers
    53.         Government workers
    54.         Self-employed in own not incorporated business workers
    55.         Unpaid family workers
    56. INCOME AND BENEFITS (IN 2023 INFLATION-ADJUSTED DOLLARS)
    57.     Total households
    58.         Less than $10,000
    59.         $10,000 to $14,999
    60.         $15,000 to $24,999
    61.         $25,000 to $34,999
    62.         $35,000 to $49,999
    63.         $50,000 to $74,999
    64.         $75,000 to $99,999
    65.         $100,000 to $149,999
    66.         $150,000 to $199,999
    67.         $200,000 or more
    68.         Median household income (dollars)
    69.         Mean household income (dollars)
    70.         With earnings
    71.             Mean earnings (dollars)
    72.         With Social Security
    73.             Mean Social Security income (dollars)
    74.         With retirement income
    75.             Mean retirement income (dollars)
    76.         With Supplemental Security Income
    77.             Mean Supplemental Security Income (dollars)
    78.         With cash public assistance income
    79.             Mean cash public assistance income (dollars)
    80.         With Food Stamp/SNAP benefits in the past 12 months
    81.     Families
    82.         Less than $10,000
    83.         $10,000 to $14,999
    84.         $15,000 to $24,999
    85.         $25,000 to $34,999
    86.         $35,000 to $49,999
    87.         $50,000 to $74,999
    88.         $75,000 to $99,999
    89.         $100,000 to $149,999
    90.         $150,000 to $199,999
    91.         $200,000 or more
    92.         Median family income (dollars)
    93.         Mean family income (dollars)
    94.     Per capita income (dollars)
    95.     Nonfamily households
    96.         Median nonfamily income (dollars)
    97.         Mean nonfamily income (dollars)
    98.     Median earnings for workers (dollars)
    99.     Median earnings for male full-time, year-round workers (dollars)
    100.     Median earnings for female full-time, year-round workers (dollars)
    101. HEALTH INSURANCE COVERAGE
    102.     Civilian noninstitutionalized population
    103.         With health insurance coverage
    104.             With private health insurance
    105.             With public coverage
    106.         No health insurance coverage
    107.     Civilian noninstitutionalized population under 19 years
    108.         No health insurance coverage
    109.     Civilian noninstitutionalized population 19 to 64 years
    110.         In labor force:
    111.             Employed:
    112.                 With health insurance coverage
    113.                     With private health insurance
    114.                     With public coverage
    115.                 No health insurance coverage
    116.             Unemployed:
    117.                 With health insurance coverage
    118.                     With private health insurance
    119.                     With public coverage
    120.                 No health insurance coverage
    121.         Not in labor force:
    122.             With health insurance coverage
    123.                 With private health insurance
    124.                 With public coverage
    125.             No health insurance coverage
    126. PERCENTAGE OF FAMILIES AND PEOPLE WHOSE INCOME IN THE PAST 12 MONTHS IS BELOW THE POVERTY LEVEL
    127.     All families
    128.         With related children of the householder under 18 years
    129.             With related children of the householder under 5 years only
    130.         Married couple families
    131.             With related children of the householder under 18 years
    132.                 With related children of the householder under 5 years only
    133.         Families with female householder, no spouse present
    134.             With related children of the householder under 18 years
    135.                 With related children of the householder under 5 years only
    136.     All people
    137.         Under 18 years
    138.             Related children of the householder under 18 years
    139.                 Related children of the householder under 5 years
    140.                 Related children of the householder 5 to 17 years
    141.         18 years and over
    142.             18 to 64 years
    143.             65 years and over
    144.         People in families
    145.         Unrelated individuals 15 years and over



```python
data_SW_economic.iloc[1:18]
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Label (Grouping)</th>
      <th>ZCTA5 21207!!Estimate</th>
      <th>ZCTA5 21207!!Margin of Error</th>
      <th>ZCTA5 21207!!Percent</th>
      <th>ZCTA5 21207!!Percent Margin of Error</th>
      <th>ZCTA5 21216!!Estimate</th>
      <th>ZCTA5 21216!!Margin of Error</th>
      <th>ZCTA5 21216!!Percent</th>
      <th>ZCTA5 21216!!Percent Margin of Error</th>
      <th>ZCTA5 21223!!Estimate</th>
      <th>...</th>
      <th>ZCTA5 21223!!Percent</th>
      <th>ZCTA5 21223!!Percent Margin of Error</th>
      <th>ZCTA5 21229!!Estimate</th>
      <th>ZCTA5 21229!!Margin of Error</th>
      <th>ZCTA5 21229!!Percent</th>
      <th>ZCTA5 21229!!Percent Margin of Error</th>
      <th>ZCTA5 21230!!Estimate</th>
      <th>ZCTA5 21230!!Margin of Error</th>
      <th>ZCTA5 21230!!Percent</th>
      <th>ZCTA5 21230!!Percent Margin of Error</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1</th>
      <td>Population 16 years and over</td>
      <td>39,670</td>
      <td>±1,856</td>
      <td>39,670</td>
      <td>(X)</td>
      <td>21,640</td>
      <td>±1,360</td>
      <td>21,640</td>
      <td>(X)</td>
      <td>16,062</td>
      <td>...</td>
      <td>16,062</td>
      <td>(X)</td>
      <td>35,575</td>
      <td>±2,040</td>
      <td>35,575</td>
      <td>(X)</td>
      <td>27,466</td>
      <td>±1,312</td>
      <td>27,466</td>
      <td>(X)</td>
    </tr>
    <tr>
      <th>2</th>
      <td>In labor force</td>
      <td>25,204</td>
      <td>±1,432</td>
      <td>63.5%</td>
      <td>±2.2</td>
      <td>11,724</td>
      <td>±835</td>
      <td>54.2%</td>
      <td>±3.2</td>
      <td>8,421</td>
      <td>...</td>
      <td>52.4%</td>
      <td>±3.4</td>
      <td>21,759</td>
      <td>±1,266</td>
      <td>61.2%</td>
      <td>±2.2</td>
      <td>20,069</td>
      <td>±1,086</td>
      <td>73.1%</td>
      <td>±2.2</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Civilian labor force</td>
      <td>25,095</td>
      <td>±1,425</td>
      <td>63.3%</td>
      <td>±2.1</td>
      <td>11,724</td>
      <td>±835</td>
      <td>54.2%</td>
      <td>±3.2</td>
      <td>8,396</td>
      <td>...</td>
      <td>52.3%</td>
      <td>±3.4</td>
      <td>21,685</td>
      <td>±1,274</td>
      <td>61.0%</td>
      <td>±2.3</td>
      <td>19,906</td>
      <td>±1,084</td>
      <td>72.5%</td>
      <td>±2.2</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Employed</td>
      <td>23,464</td>
      <td>±1,230</td>
      <td>59.1%</td>
      <td>±2.2</td>
      <td>10,209</td>
      <td>±795</td>
      <td>47.2%</td>
      <td>±3.1</td>
      <td>7,508</td>
      <td>...</td>
      <td>46.7%</td>
      <td>±3.3</td>
      <td>20,292</td>
      <td>±1,191</td>
      <td>57.0%</td>
      <td>±2.2</td>
      <td>19,223</td>
      <td>±1,059</td>
      <td>70.0%</td>
      <td>±2.2</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Unemployed</td>
      <td>1,631</td>
      <td>±474</td>
      <td>4.1%</td>
      <td>±1.1</td>
      <td>1,515</td>
      <td>±411</td>
      <td>7.0%</td>
      <td>±1.9</td>
      <td>888</td>
      <td>...</td>
      <td>5.5%</td>
      <td>±1.7</td>
      <td>1,393</td>
      <td>±310</td>
      <td>3.9%</td>
      <td>±0.8</td>
      <td>683</td>
      <td>±194</td>
      <td>2.5%</td>
      <td>±0.7</td>
    </tr>
    <tr>
      <th>6</th>
      <td>Armed Forces</td>
      <td>109</td>
      <td>±90</td>
      <td>0.3%</td>
      <td>±0.2</td>
      <td>0</td>
      <td>±25</td>
      <td>0.0%</td>
      <td>±0.2</td>
      <td>25</td>
      <td>...</td>
      <td>0.2%</td>
      <td>±0.2</td>
      <td>74</td>
      <td>±82</td>
      <td>0.2%</td>
      <td>±0.2</td>
      <td>163</td>
      <td>±79</td>
      <td>0.6%</td>
      <td>±0.3</td>
    </tr>
    <tr>
      <th>7</th>
      <td>Not in labor force</td>
      <td>14,466</td>
      <td>±1,115</td>
      <td>36.5%</td>
      <td>±2.2</td>
      <td>9,916</td>
      <td>±1,055</td>
      <td>45.8%</td>
      <td>±3.2</td>
      <td>7,641</td>
      <td>...</td>
      <td>47.6%</td>
      <td>±3.4</td>
      <td>13,816</td>
      <td>±1,279</td>
      <td>38.8%</td>
      <td>±2.2</td>
      <td>7,397</td>
      <td>±725</td>
      <td>26.9%</td>
      <td>±2.2</td>
    </tr>
    <tr>
      <th>8</th>
      <td>Civilian labor force</td>
      <td>25,095</td>
      <td>±1,425</td>
      <td>25,095</td>
      <td>(X)</td>
      <td>11,724</td>
      <td>±835</td>
      <td>11,724</td>
      <td>(X)</td>
      <td>8,396</td>
      <td>...</td>
      <td>8,396</td>
      <td>(X)</td>
      <td>21,685</td>
      <td>±1,274</td>
      <td>21,685</td>
      <td>(X)</td>
      <td>19,906</td>
      <td>±1,084</td>
      <td>19,906</td>
      <td>(X)</td>
    </tr>
    <tr>
      <th>9</th>
      <td>Unemployment Rate</td>
      <td>(X)</td>
      <td>(X)</td>
      <td>6.5%</td>
      <td>±1.7</td>
      <td>(X)</td>
      <td>(X)</td>
      <td>12.9%</td>
      <td>±3.3</td>
      <td>(X)</td>
      <td>...</td>
      <td>10.6%</td>
      <td>±3.2</td>
      <td>(X)</td>
      <td>(X)</td>
      <td>6.4%</td>
      <td>±1.3</td>
      <td>(X)</td>
      <td>(X)</td>
      <td>3.4%</td>
      <td>±1.0</td>
    </tr>
    <tr>
      <th>10</th>
      <td>Females 16 years and over</td>
      <td>21,717</td>
      <td>±1,272</td>
      <td>21,717</td>
      <td>(X)</td>
      <td>12,555</td>
      <td>±1,165</td>
      <td>12,555</td>
      <td>(X)</td>
      <td>8,765</td>
      <td>...</td>
      <td>8,765</td>
      <td>(X)</td>
      <td>19,919</td>
      <td>±1,447</td>
      <td>19,919</td>
      <td>(X)</td>
      <td>13,886</td>
      <td>±783</td>
      <td>13,886</td>
      <td>(X)</td>
    </tr>
    <tr>
      <th>11</th>
      <td>In labor force</td>
      <td>13,407</td>
      <td>±1,014</td>
      <td>61.7%</td>
      <td>±2.7</td>
      <td>6,966</td>
      <td>±822</td>
      <td>55.5%</td>
      <td>±4.1</td>
      <td>4,122</td>
      <td>...</td>
      <td>47.0%</td>
      <td>±4.4</td>
      <td>11,623</td>
      <td>±877</td>
      <td>58.4%</td>
      <td>±3.1</td>
      <td>9,613</td>
      <td>±662</td>
      <td>69.2%</td>
      <td>±3.2</td>
    </tr>
    <tr>
      <th>12</th>
      <td>Civilian labor force</td>
      <td>13,407</td>
      <td>±1,014</td>
      <td>61.7%</td>
      <td>±2.7</td>
      <td>6,966</td>
      <td>±822</td>
      <td>55.5%</td>
      <td>±4.1</td>
      <td>4,109</td>
      <td>...</td>
      <td>46.9%</td>
      <td>±4.5</td>
      <td>11,623</td>
      <td>±877</td>
      <td>58.4%</td>
      <td>±3.1</td>
      <td>9,558</td>
      <td>±660</td>
      <td>68.8%</td>
      <td>±3.2</td>
    </tr>
    <tr>
      <th>13</th>
      <td>Employed</td>
      <td>12,399</td>
      <td>±939</td>
      <td>57.1%</td>
      <td>±3.0</td>
      <td>6,058</td>
      <td>±775</td>
      <td>48.3%</td>
      <td>±4.2</td>
      <td>3,831</td>
      <td>...</td>
      <td>43.7%</td>
      <td>±4.3</td>
      <td>11,035</td>
      <td>±854</td>
      <td>55.4%</td>
      <td>±3.1</td>
      <td>9,338</td>
      <td>±664</td>
      <td>67.2%</td>
      <td>±3.3</td>
    </tr>
    <tr>
      <th>14</th>
      <td>Own children of the householder under 6 years</td>
      <td>3,567</td>
      <td>±815</td>
      <td>3,567</td>
      <td>(X)</td>
      <td>2,237</td>
      <td>±700</td>
      <td>2,237</td>
      <td>(X)</td>
      <td>1,572</td>
      <td>...</td>
      <td>1,572</td>
      <td>(X)</td>
      <td>3,242</td>
      <td>±555</td>
      <td>3,242</td>
      <td>(X)</td>
      <td>2,484</td>
      <td>±444</td>
      <td>2,484</td>
      <td>(X)</td>
    </tr>
    <tr>
      <th>15</th>
      <td>All parents in family in labor force</td>
      <td>2,397</td>
      <td>±602</td>
      <td>67.2%</td>
      <td>±9.1</td>
      <td>1,691</td>
      <td>±414</td>
      <td>75.6%</td>
      <td>±21.0</td>
      <td>1,145</td>
      <td>...</td>
      <td>72.8%</td>
      <td>±12.5</td>
      <td>2,428</td>
      <td>±466</td>
      <td>74.9%</td>
      <td>±10.2</td>
      <td>1,756</td>
      <td>±380</td>
      <td>70.7%</td>
      <td>±9.0</td>
    </tr>
    <tr>
      <th>16</th>
      <td>Own children of the householder 6 to 17 years</td>
      <td>6,862</td>
      <td>±1,027</td>
      <td>6,862</td>
      <td>(X)</td>
      <td>3,370</td>
      <td>±786</td>
      <td>3,370</td>
      <td>(X)</td>
      <td>2,261</td>
      <td>...</td>
      <td>2,261</td>
      <td>(X)</td>
      <td>5,747</td>
      <td>±819</td>
      <td>5,747</td>
      <td>(X)</td>
      <td>3,619</td>
      <td>±788</td>
      <td>3,619</td>
      <td>(X)</td>
    </tr>
    <tr>
      <th>17</th>
      <td>All parents in family in labor force</td>
      <td>5,102</td>
      <td>±907</td>
      <td>74.4%</td>
      <td>±8.0</td>
      <td>2,668</td>
      <td>±704</td>
      <td>79.2%</td>
      <td>±12.0</td>
      <td>1,211</td>
      <td>...</td>
      <td>53.6%</td>
      <td>±10.1</td>
      <td>4,484</td>
      <td>±795</td>
      <td>78.0%</td>
      <td>±6.9</td>
      <td>2,250</td>
      <td>±549</td>
      <td>62.2%</td>
      <td>±11.3</td>
    </tr>
  </tbody>
</table>
<p>17 rows × 21 columns</p>
</div>




```python
data_SW_economic.iloc[56:125]
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Label (Grouping)</th>
      <th>ZCTA5 21207!!Estimate</th>
      <th>ZCTA5 21207!!Margin of Error</th>
      <th>ZCTA5 21207!!Percent</th>
      <th>ZCTA5 21207!!Percent Margin of Error</th>
      <th>ZCTA5 21216!!Estimate</th>
      <th>ZCTA5 21216!!Margin of Error</th>
      <th>ZCTA5 21216!!Percent</th>
      <th>ZCTA5 21216!!Percent Margin of Error</th>
      <th>ZCTA5 21223!!Estimate</th>
      <th>...</th>
      <th>ZCTA5 21223!!Percent</th>
      <th>ZCTA5 21223!!Percent Margin of Error</th>
      <th>ZCTA5 21229!!Estimate</th>
      <th>ZCTA5 21229!!Margin of Error</th>
      <th>ZCTA5 21229!!Percent</th>
      <th>ZCTA5 21229!!Percent Margin of Error</th>
      <th>ZCTA5 21230!!Estimate</th>
      <th>ZCTA5 21230!!Margin of Error</th>
      <th>ZCTA5 21230!!Percent</th>
      <th>ZCTA5 21230!!Percent Margin of Error</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>56</th>
      <td>Total households</td>
      <td>19,608</td>
      <td>±687</td>
      <td>19,608</td>
      <td>(X)</td>
      <td>11,667</td>
      <td>±760</td>
      <td>11,667</td>
      <td>(X)</td>
      <td>8,300</td>
      <td>...</td>
      <td>8,300</td>
      <td>(X)</td>
      <td>18,517</td>
      <td>±772</td>
      <td>18,517</td>
      <td>(X)</td>
      <td>16,003</td>
      <td>±759</td>
      <td>16,003</td>
      <td>(X)</td>
    </tr>
    <tr>
      <th>57</th>
      <td>Less than $10,000</td>
      <td>1,443</td>
      <td>±306</td>
      <td>7.4%</td>
      <td>±1.6</td>
      <td>1,272</td>
      <td>±372</td>
      <td>10.9%</td>
      <td>±3.0</td>
      <td>816</td>
      <td>...</td>
      <td>9.8%</td>
      <td>±2.5</td>
      <td>1,416</td>
      <td>±331</td>
      <td>7.6%</td>
      <td>±1.8</td>
      <td>900</td>
      <td>±244</td>
      <td>5.6%</td>
      <td>±1.5</td>
    </tr>
    <tr>
      <th>58</th>
      <td>$10,000 to $14,999</td>
      <td>490</td>
      <td>±191</td>
      <td>2.5%</td>
      <td>±1.0</td>
      <td>1,641</td>
      <td>±385</td>
      <td>14.1%</td>
      <td>±3.2</td>
      <td>1,022</td>
      <td>...</td>
      <td>12.3%</td>
      <td>±3.7</td>
      <td>925</td>
      <td>±321</td>
      <td>5.0%</td>
      <td>±1.7</td>
      <td>596</td>
      <td>±192</td>
      <td>3.7%</td>
      <td>±1.2</td>
    </tr>
    <tr>
      <th>59</th>
      <td>$15,000 to $24,999</td>
      <td>1,349</td>
      <td>±365</td>
      <td>6.9%</td>
      <td>±1.8</td>
      <td>1,206</td>
      <td>±319</td>
      <td>10.3%</td>
      <td>±2.7</td>
      <td>934</td>
      <td>...</td>
      <td>11.3%</td>
      <td>±3.2</td>
      <td>1,651</td>
      <td>±349</td>
      <td>8.9%</td>
      <td>±1.9</td>
      <td>900</td>
      <td>±216</td>
      <td>5.6%</td>
      <td>±1.3</td>
    </tr>
    <tr>
      <th>60</th>
      <td>$25,000 to $34,999</td>
      <td>1,609</td>
      <td>±382</td>
      <td>8.2%</td>
      <td>±1.9</td>
      <td>848</td>
      <td>±230</td>
      <td>7.3%</td>
      <td>±1.9</td>
      <td>906</td>
      <td>...</td>
      <td>10.9%</td>
      <td>±3.2</td>
      <td>1,270</td>
      <td>±243</td>
      <td>6.9%</td>
      <td>±1.3</td>
      <td>780</td>
      <td>±218</td>
      <td>4.9%</td>
      <td>±1.4</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>120</th>
      <td>Not in labor force:</td>
      <td>6,228</td>
      <td>±789</td>
      <td>6,228</td>
      <td>(X)</td>
      <td>5,526</td>
      <td>±902</td>
      <td>5,526</td>
      <td>(X)</td>
      <td>4,352</td>
      <td>...</td>
      <td>4,352</td>
      <td>(X)</td>
      <td>5,846</td>
      <td>±794</td>
      <td>5,846</td>
      <td>(X)</td>
      <td>3,940</td>
      <td>±493</td>
      <td>3,940</td>
      <td>(X)</td>
    </tr>
    <tr>
      <th>121</th>
      <td>With health insurance coverage</td>
      <td>5,341</td>
      <td>±732</td>
      <td>85.8%</td>
      <td>±4.5</td>
      <td>5,112</td>
      <td>±867</td>
      <td>92.5%</td>
      <td>±2.8</td>
      <td>3,768</td>
      <td>...</td>
      <td>86.6%</td>
      <td>±5.8</td>
      <td>5,056</td>
      <td>±732</td>
      <td>86.5%</td>
      <td>±5.2</td>
      <td>3,445</td>
      <td>±475</td>
      <td>87.4%</td>
      <td>±5.1</td>
    </tr>
    <tr>
      <th>122</th>
      <td>With private health insurance</td>
      <td>2,202</td>
      <td>±445</td>
      <td>35.4%</td>
      <td>±6.6</td>
      <td>1,733</td>
      <td>±507</td>
      <td>31.4%</td>
      <td>±6.4</td>
      <td>611</td>
      <td>...</td>
      <td>14.0%</td>
      <td>±4.1</td>
      <td>1,709</td>
      <td>±366</td>
      <td>29.2%</td>
      <td>±5.6</td>
      <td>1,860</td>
      <td>±338</td>
      <td>47.2%</td>
      <td>±5.9</td>
    </tr>
    <tr>
      <th>123</th>
      <td>With public coverage</td>
      <td>3,925</td>
      <td>±692</td>
      <td>63.0%</td>
      <td>±6.8</td>
      <td>3,706</td>
      <td>±607</td>
      <td>67.1%</td>
      <td>±6.3</td>
      <td>3,341</td>
      <td>...</td>
      <td>76.8%</td>
      <td>±6.3</td>
      <td>3,801</td>
      <td>±674</td>
      <td>65.0%</td>
      <td>±6.4</td>
      <td>1,967</td>
      <td>±365</td>
      <td>49.9%</td>
      <td>±6.5</td>
    </tr>
    <tr>
      <th>124</th>
      <td>No health insurance coverage</td>
      <td>887</td>
      <td>±303</td>
      <td>14.2%</td>
      <td>±4.5</td>
      <td>414</td>
      <td>±164</td>
      <td>7.5%</td>
      <td>±2.8</td>
      <td>584</td>
      <td>...</td>
      <td>13.4%</td>
      <td>±5.8</td>
      <td>790</td>
      <td>±332</td>
      <td>13.5%</td>
      <td>±5.2</td>
      <td>495</td>
      <td>±212</td>
      <td>12.6%</td>
      <td>±5.1</td>
    </tr>
  </tbody>
</table>
<p>69 rows × 21 columns</p>
</div>




```python
import pandas as pd

# data
subset_df = data_SW_economic.iloc[56:125].copy()

# Cleaning column names to make them more readable
clean_columns = {
    col: col.replace("ZCTA5 ", "").replace("!!", "_") for col in subset_df.columns
}
subset_df.rename(columns=clean_columns, inplace=True)

# Reshape data to a long format for easier merging
melted_df = subset_df.melt(id_vars=["Label (Grouping)"], var_name="ZCTA5_Metric", value_name="Value")

# Splitting the `ZCTA5_Metric` column into ZCTA5 and Metric for clarity
melted_df[['ZCTA5', 'Metric']] = melted_df['ZCTA5_Metric'].str.split("_", n=1, expand=True)
melted_df.drop(columns=["ZCTA5_Metric"], inplace=True)

# Pivot the table so that it is easy for merging
final_table_econ = melted_df.pivot_table(index=["Label (Grouping)", "ZCTA5"], columns="Metric", values="Value", aggfunc="first").reset_index()

# Display
final_table_econ







```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th>Metric</th>
      <th>Label (Grouping)</th>
      <th>ZCTA5</th>
      <th>Estimate</th>
      <th>Margin of Error</th>
      <th>Percent</th>
      <th>Percent Margin of Error</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Civilian noninstitutionalized population</td>
      <td>21207</td>
      <td>49,164</td>
      <td>±2,694</td>
      <td>49,164</td>
      <td>(X)</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Civilian noninstitutionalized population</td>
      <td>21216</td>
      <td>27,231</td>
      <td>±2,284</td>
      <td>27,231</td>
      <td>(X)</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Civilian noninstitutionalized population</td>
      <td>21223</td>
      <td>19,709</td>
      <td>±1,623</td>
      <td>19,709</td>
      <td>(X)</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Civilian noninstitutionalized population</td>
      <td>21229</td>
      <td>44,133</td>
      <td>±2,465</td>
      <td>44,133</td>
      <td>(X)</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Civilian noninstitutionalized population</td>
      <td>21230</td>
      <td>33,346</td>
      <td>±1,757</td>
      <td>33,346</td>
      <td>(X)</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>260</th>
      <td>With public coverage</td>
      <td>21207</td>
      <td>4,035</td>
      <td>±634</td>
      <td>18.6%</td>
      <td>±2.8</td>
    </tr>
    <tr>
      <th>261</th>
      <td>With public coverage</td>
      <td>21216</td>
      <td>2,233</td>
      <td>±380</td>
      <td>23.9%</td>
      <td>±3.7</td>
    </tr>
    <tr>
      <th>262</th>
      <td>With public coverage</td>
      <td>21223</td>
      <td>2,295</td>
      <td>±459</td>
      <td>33.1%</td>
      <td>±6.0</td>
    </tr>
    <tr>
      <th>263</th>
      <td>With public coverage</td>
      <td>21229</td>
      <td>4,113</td>
      <td>±852</td>
      <td>21.8%</td>
      <td>±3.8</td>
    </tr>
    <tr>
      <th>264</th>
      <td>With public coverage</td>
      <td>21230</td>
      <td>1,555</td>
      <td>±338</td>
      <td>8.4%</td>
      <td>±1.7</td>
    </tr>
  </tbody>
</table>
<p>265 rows × 6 columns</p>
</div>



A function termed categorize, was implemented to derive a new categorical variable. This function evaluates the textual content of the 'Metric' column for each record and assigns categorical labels.
Additionally, a pivot table is used aggregate the quantitative 'Estimate' data by both 'ZCTA5' and 'Category' and finally formatting with commas a separators, to enhance readability. 


```python
import pandas as pd
import numpy as np

# data
df = pd.DataFrame(final_table_econ).rename(columns={'Label (Grouping)': 'Metric'})

# Clean and categorize data
def categorize(row):
    metric = row['Metric'].lower().strip()

    if not metric:
        return None

    if any(x in metric for x in ['less than $10,000', '$10,000 to $14,999', '$15,000 to $24,999']):
        return 'below 25,000'
    elif any(x in metric for x in ['$25,000 to $34,999', '$35,000 to $49,999', '$50,000 to $74,999',
                                   '$75,000 to $99,999', '$100,000 to $149,999', '$150,000 to $199,999', '$200,000 or more']):
        return 'above 25,000'
    
    if 'median income' in metric or 'median household income' in metric:
        return 'median_income'
    
    if 'without health insurance' in metric or 'no health insurance' in metric:
        return 'uninsured'
    
    
    
    if 'civilian' in metric:
        return 'total_population'
    
    return None

df['Category'] = df.apply(categorize, axis=1)
df = df.dropna(subset=['Category'])

# Function to clean and sum  values
def clean_and_sum(value):
    if pd.isna(value):
        return 0
    if isinstance(value, str):
        # Remove any non-numeric except  periods
        value = value.replace(',', '').strip()
    try:
        return float(value)
    except ValueError:
        return 0

df['Estimate'] = df['Estimate'].apply(clean_and_sum)

# Ensure 'ZCTA5' is treated as a string to prevent unintended numerical operations
df['ZCTA5'] = df['ZCTA5'].astype(str)

# Required categories 
required_categories = ['below 25,000', 'above 25,000', 'median_income',
                       'total_population', 'uninsured']

df['Category'] = pd.Categorical(df['Category'], categories=required_categories, ordered=True)

# Pivot table with proper numeric aggregation
pivot_df = df.pivot_table(
    index='ZCTA5',
    columns='Category',
    values='Estimate',
    aggfunc='sum',  # Ensure summation instead of concatenation
    fill_value=0
).reset_index()

# Rename columns for clarity 
column_mapping = {
    'below 25,000': 'Below25k_Estimate',
    'above 25,000': 'Above25k_Estimate',
    'median_income': 'MedianIncome_Estimate',
    'total_population': 'TotalPop_Estimate',
    'uninsured': 'Uninsured_Estimate'
}

pivot_df = pivot_df.rename(columns=column_mapping)

# Format numbers for better reading
numeric_cols = pivot_df.columns.difference(['ZCTA5'])
pivot_df[numeric_cols] = pivot_df[numeric_cols].applymap(lambda x: f"{int(x):,}" if pd.notnull(x) else x)

# Display 
pivot_df

import warnings
warnings.filterwarnings('ignore')
```

# Exploratory Data Analysis and Visualization

Using HTML we created a map that shows homeless shelters and social welfare centers within Southwest Baltimore and beyond with the aim of highlithing the health and wellness service deserts within Southwest Baltimore to give an idea of where Pauls place clinic could re-focus their outreach services to.


```python
import os
import time
import webbrowser
import geopandas as gpd
import pandas as pd
import folium
from pyproj import Transformer
from folium.plugins import HeatMap
from shapely.geometry import Point
import osmnx as ox 

# Configure settings
ox.settings.use_cache = True
ox.settings.log_console = True

# Data
economic_data = pd.DataFrame(final_table_economic2)

def load_shelters(url, is_homeless=True):
    df = pd.read_csv(url, encoding='utf-8-sig', sep=',')
    
    if is_homeless:
        # Convert from EPSG:3857 (Web Mercator) to EPSG:4326 with correct axis order
        transformer = Transformer.from_crs("EPSG:3857", "EPSG:4326", always_xy=True)
        df['X'] = pd.to_numeric(df['X'], errors='coerce')
        df['Y'] = pd.to_numeric(df['Y'], errors='coerce')
        df = df.dropna(subset=['X', 'Y'])
        df['lon'], df['lat'] = transformer.transform(df['X'], df['Y'])  # Correct order
        geom = gpd.points_from_xy(df.lon, df.lat)
    else:
        df = df.dropna(subset=['Longitude', 'Latitude'])
        df['Longitude'] = pd.to_numeric(df['Longitude'], errors='coerce')
        df['Latitude'] = pd.to_numeric(df['Latitude'], errors='coerce')
        geom = gpd.points_from_xy(df['Longitude'], df['Latitude'])
        
    return gpd.GeoDataFrame(df, geometry=geom, crs="EPSG:4326")

# Load datasets with validation
print("Loading datasets...")
gdf_economic = gpd.GeoDataFrame(
    economic_data,
    geometry=gpd.points_from_xy(economic_data.longitude, economic_data.latitude),
    crs="EPSG:4326"
)

gdf_homeless = load_shelters("https://raw.githubusercontent.com/paulsplacemd/paulsplacelocator/main/Homeless_Shelters.csv")
gdf_social = load_shelters("https://raw.githubusercontent.com/paulsplacemd/paulsplacelocator/main/baltimore_help_social_health_welfare_shelters_locations.csv", False)

print(f"Loaded {len(gdf_homeless)} homeless shelters")
print(f"Loaded {len(gdf_social)} social shelters")

def create_pauls_place_buffers(coords):
    try:
        point = Point(coords[1], coords[0])  # (lon, lat)
        gdf = gpd.GeoDataFrame(geometry=[point], crs="EPSG:4326")
        projected = gdf.to_crs("EPSG:2248")
        projected["buffer_1mi"] = projected.geometry.buffer(5280)
        projected["buffer_3mi"] = projected.geometry.buffer(15840)
        return projected.to_crs("EPSG:4326")
    except Exception as e:
        print(f"Buffer error: {str(e)}")
        return gpd.GeoDataFrame()

def add_shelter_markers(map_obj, gdf_homeless, gdf_social):
    # Create isolated layer groups
    homeless_layer = folium.FeatureGroup(name='👥 Homeless Shelters', show=True)
    social_layer = folium.FeatureGroup(name='🏥 Social Shelters', show=True)

    # Add social shelters first 
    print("\nAdding social shelters:")
    for idx, row in gdf_social.iterrows():
        try:
            folium.Marker(
                location=[row['Latitude'], row['Longitude']],
                popup=row.get('Location', 'Social Shelter'),
                icon=folium.Icon(color='green', icon='heart', prefix='fa')
            ).add_to(social_layer)
        except Exception as e:
            print(f"Social marker error: {str(e)}")

    # Add homeless shelters 
    print("\nAdding homeless shelters:")
    for idx, row in gdf_homeless.iterrows():
        try:
            folium.Marker(
                location=[row['lat'], row['lon']],  # Using coordinates
                popup=row.get('name', 'Homeless Shelter'),
                icon=folium.Icon(color='red', icon='bed', prefix='fa')
            ).add_to(homeless_layer)
        except Exception as e:
            print(f"Homeless marker error: {str(e)}")

    # layers in correct z-order
    social_layer.add_to(map_obj)
    homeless_layer.add_to(map_obj)
    return social_layer, homeless_layer

def create_accessibility_map():
    m = folium.Map(location=[39.29, -76.65], zoom_start=12)

    # Economic markers
    for _, row in gdf_economic.iterrows():
        try:
            folium.CircleMarker(
                location=[row['latitude'], row['longitude']],
                radius=25 + (row['Estimate'] / 7500),
                popup=f"ZIP {row['ZCTA5']}",
                color='#4b0082',
                fill_color='#9370db',
                fill_opacity=0.7,
                weight=1
            ).add_to(m)
        except Exception as e:
            print(f"Economic error: {str(e)}")

    # Shelter markers with isolated layers
    add_shelter_markers(m, gdf_homeless, gdf_social)

    # Paul's Place elements
    pauls_place_coords = [39.2848, -76.6268]
    buffers = create_pauls_place_buffers(pauls_place_coords[::-1])
    
    if not buffers.empty:
        # Buffers
        folium.GeoJson(
            buffers['buffer_1mi'],
            style_function=lambda x: {'fillColor': 'green', 'color': 'green', 'fillOpacity': 0.1}
        ).add_to(m)
        
        folium.GeoJson(
            buffers['buffer_3mi'],
            style_function=lambda x: {'fillColor': 'yellow', 'color': 'yellow', 'fillOpacity': 0.1}
        ).add_to(m)

        # Circles as area radius
        folium.Circle(
            pauls_place_coords,
            radius=1609.34,  # 1 mile in meters
            color='blue',
            fill=False,
            weight=2
        ).add_to(m)
        
        folium.Circle(
            pauls_place_coords,
            radius=4828.02,  # 3 miles in meters
            color='red',
            fill=False,
            weight=2
        ).add_to(m)

    # Heatmap with corrected coordinates
    try:
        heatmap_data = []
        for _, row in gdf_homeless.iterrows():
            heatmap_data.append([row['lat'], row['lon']])
        for _, row in gdf_social.iterrows():
            heatmap_data.append([row['Latitude'], row['Longitude']])
        
        HeatMap(heatmap_data, radius=40, blur=30).add_to(m)
    except Exception as e:
        print(f"Heatmap error: {str(e)}")

    # legend
    legend_html = '''
    <div style="position: fixed; 
                bottom: 50px; 
                left: 50px; 
                z-index: 1000;
                padding: 10px;
                background: white;
                border: 2px solid grey;
                border-radius: 5px;
                box-shadow: 0 0 10px rgba(0,0,0,0.2)">
        <b>Legend</b><br>
        <i class="fa fa-bed" style="color:white; background:red; padding:5px; border-radius:3px"></i> Homeless<br>
        <i class="fa fa-heart" style="color:white; background:green; padding:5px; border-radius:3px"></i> Social<br>
        <div style="background:blue; height:2px; margin:5px 0"></div> 1mi Radius<br>
        <div style="background:red; height:2px; margin:5px 0"></div> 3mi Radius
    </div>
    '''
    m.get_root().html.add_child(folium.Element(legend_html))

    # Layer control
    folium.LayerControl(position='topright', collapsed=False).add_to(m)

    return m

if __name__ == "__main__":
    output_path = os.path.abspath("SWbaltimore_accessibilitymap.html")
    print(f"Generating map: {output_path}")
    
    try:
        start_time = time.time()
        final_map = create_accessibility_map()
        final_map.save(output_path)
        print(f"Map created in {time.time()-start_time:.1f} seconds")
        webbrowser.open(output_path)
    except Exception as e:
        print(f"Critical error: {str(e)}")

    display(final_map) 
```

    Loading datasets...
    Loaded 38 homeless shelters
    Loaded 43 social shelters
    Generating map: /Users/bayowaonabajo/Downloads/SWbaltimore_accessibilitymap.html
    
    Adding social shelters:
    
    Adding homeless shelters:
    Map created in 0.1 seconds



<div style="width:100%;"><div style="position:relative;width:100%;height:0;padding-bottom:60%;"><span style="color:#565656">Make this Notebook Trusted to load map: File -> Trust Notebook</span><iframe srcdoc="&lt;!DOCTYPE html&gt;
&lt;html&gt;
&lt;head&gt;

    &lt;meta http-equiv=&quot;content-type&quot; content=&quot;text/html; charset=UTF-8&quot; /&gt;

        &lt;script&gt;
            L_NO_TOUCH = false;
            L_DISABLE_3D = false;
        &lt;/script&gt;

    &lt;style&gt;html, body {width: 100%;height: 100%;margin: 0;padding: 0;}&lt;/style&gt;
    &lt;style&gt;#map {position:absolute;top:0;bottom:0;right:0;left:0;}&lt;/style&gt;
    &lt;script src=&quot;https://cdn.jsdelivr.net/npm/leaflet@1.9.3/dist/leaflet.js&quot;&gt;&lt;/script&gt;
    &lt;script src=&quot;https://code.jquery.com/jquery-3.7.1.min.js&quot;&gt;&lt;/script&gt;
    &lt;script src=&quot;https://cdn.jsdelivr.net/npm/bootstrap@5.2.2/dist/js/bootstrap.bundle.min.js&quot;&gt;&lt;/script&gt;
    &lt;script src=&quot;https://cdnjs.cloudflare.com/ajax/libs/Leaflet.awesome-markers/2.0.2/leaflet.awesome-markers.js&quot;&gt;&lt;/script&gt;
    &lt;link rel=&quot;stylesheet&quot; href=&quot;https://cdn.jsdelivr.net/npm/leaflet@1.9.3/dist/leaflet.css&quot;/&gt;
    &lt;link rel=&quot;stylesheet&quot; href=&quot;https://cdn.jsdelivr.net/npm/bootstrap@5.2.2/dist/css/bootstrap.min.css&quot;/&gt;
    &lt;link rel=&quot;stylesheet&quot; href=&quot;https://netdna.bootstrapcdn.com/bootstrap/3.0.0/css/bootstrap-glyphicons.css&quot;/&gt;
    &lt;link rel=&quot;stylesheet&quot; href=&quot;https://cdn.jsdelivr.net/npm/@fortawesome/fontawesome-free@6.2.0/css/all.min.css&quot;/&gt;
    &lt;link rel=&quot;stylesheet&quot; href=&quot;https://cdnjs.cloudflare.com/ajax/libs/Leaflet.awesome-markers/2.0.2/leaflet.awesome-markers.css&quot;/&gt;
    &lt;link rel=&quot;stylesheet&quot; href=&quot;https://cdn.jsdelivr.net/gh/python-visualization/folium/folium/templates/leaflet.awesome.rotate.min.css&quot;/&gt;

            &lt;meta name=&quot;viewport&quot; content=&quot;width=device-width,
                initial-scale=1.0, maximum-scale=1.0, user-scalable=no&quot; /&gt;
            &lt;style&gt;
                #map_0726b6ba9a4d2cc5792f26bf69fef3e1 {
                    position: relative;
                    width: 100.0%;
                    height: 100.0%;
                    left: 0.0%;
                    top: 0.0%;
                }
                .leaflet-container { font-size: 1rem; }
            &lt;/style&gt;

    &lt;script src=&quot;https://cdn.jsdelivr.net/gh/python-visualization/folium@main/folium/templates/leaflet_heat.min.js&quot;&gt;&lt;/script&gt;
&lt;/head&gt;
&lt;body&gt;


    &lt;div style=&quot;position: fixed; 
                bottom: 50px; 
                left: 50px; 
                z-index: 1000;
                padding: 10px;
                background: white;
                border: 2px solid grey;
                border-radius: 5px;
                box-shadow: 0 0 10px rgba(0,0,0,0.2)&quot;&gt;
        &lt;b&gt;Legend&lt;/b&gt;&lt;br&gt;
        &lt;i class=&quot;fa fa-bed&quot; style=&quot;color:white; background:red; padding:5px; border-radius:3px&quot;&gt;&lt;/i&gt; Homeless&lt;br&gt;
        &lt;i class=&quot;fa fa-heart&quot; style=&quot;color:white; background:green; padding:5px; border-radius:3px&quot;&gt;&lt;/i&gt; Social&lt;br&gt;
        &lt;div style=&quot;background:blue; height:2px; margin:5px 0&quot;&gt;&lt;/div&gt; 1mi Radius&lt;br&gt;
        &lt;div style=&quot;background:red; height:2px; margin:5px 0&quot;&gt;&lt;/div&gt; 3mi Radius
    &lt;/div&gt;


            &lt;div class=&quot;folium-map&quot; id=&quot;map_0726b6ba9a4d2cc5792f26bf69fef3e1&quot; &gt;&lt;/div&gt;

&lt;/body&gt;
&lt;script&gt;


            var map_0726b6ba9a4d2cc5792f26bf69fef3e1 = L.map(
                &quot;map_0726b6ba9a4d2cc5792f26bf69fef3e1&quot;,
                {
                    center: [39.29, -76.65],
                    crs: L.CRS.EPSG3857,
                    ...{
  &quot;zoom&quot;: 12,
  &quot;zoomControl&quot;: true,
  &quot;preferCanvas&quot;: false,
}

                }
            );





            var tile_layer_f97ed5ae929afc9075704ec97a8ffe2a = L.tileLayer(
                &quot;https://tile.openstreetmap.org/{z}/{x}/{y}.png&quot;,
                {
  &quot;minZoom&quot;: 0,
  &quot;maxZoom&quot;: 19,
  &quot;maxNativeZoom&quot;: 19,
  &quot;noWrap&quot;: false,
  &quot;attribution&quot;: &quot;\u0026copy; \u003ca href=\&quot;https://www.openstreetmap.org/copyright\&quot;\u003eOpenStreetMap\u003c/a\u003e contributors&quot;,
  &quot;subdomains&quot;: &quot;abc&quot;,
  &quot;detectRetina&quot;: false,
  &quot;tms&quot;: false,
  &quot;opacity&quot;: 1,
}

            );


            tile_layer_f97ed5ae929afc9075704ec97a8ffe2a.addTo(map_0726b6ba9a4d2cc5792f26bf69fef3e1);


            var circle_marker_642074ce7294d99d75e6e08837de14c9 = L.circleMarker(
                [39.324158046482054, -76.72021341229762],
                {&quot;bubblingMouseEvents&quot;: true, &quot;color&quot;: &quot;#4b0082&quot;, &quot;dashArray&quot;: null, &quot;dashOffset&quot;: null, &quot;fill&quot;: true, &quot;fillColor&quot;: &quot;#9370db&quot;, &quot;fillOpacity&quot;: 0.7, &quot;fillRule&quot;: &quot;evenodd&quot;, &quot;lineCap&quot;: &quot;round&quot;, &quot;lineJoin&quot;: &quot;round&quot;, &quot;opacity&quot;: 1.0, &quot;radius&quot;: 30.28933333333333, &quot;stroke&quot;: true, &quot;weight&quot;: 1}
            ).addTo(map_0726b6ba9a4d2cc5792f26bf69fef3e1);


        var popup_ea120dace819d6799e354543fbe930f2 = L.popup({
  &quot;maxWidth&quot;: &quot;100%&quot;,
});



                var html_d2ebb28e5c0187fdc81331d6f0050123 = $(`&lt;div id=&quot;html_d2ebb28e5c0187fdc81331d6f0050123&quot; style=&quot;width: 100.0%; height: 100.0%;&quot;&gt;ZIP 21207&lt;/div&gt;`)[0];
                popup_ea120dace819d6799e354543fbe930f2.setContent(html_d2ebb28e5c0187fdc81331d6f0050123);



        circle_marker_642074ce7294d99d75e6e08837de14c9.bindPopup(popup_ea120dace819d6799e354543fbe930f2)
        ;




            var circle_marker_48b398aa2bff230bf07bce78ee4a221a = L.circleMarker(
                [39.31082051076663, -76.67145823219852],
                {&quot;bubblingMouseEvents&quot;: true, &quot;color&quot;: &quot;#4b0082&quot;, &quot;dashArray&quot;: null, &quot;dashOffset&quot;: null, &quot;fill&quot;: true, &quot;fillColor&quot;: &quot;#9370db&quot;, &quot;fillOpacity&quot;: 0.7, &quot;fillRule&quot;: &quot;evenodd&quot;, &quot;lineCap&quot;: &quot;round&quot;, &quot;lineJoin&quot;: &quot;round&quot;, &quot;opacity&quot;: 1.0, &quot;radius&quot;: 27.885333333333335, &quot;stroke&quot;: true, &quot;weight&quot;: 1}
            ).addTo(map_0726b6ba9a4d2cc5792f26bf69fef3e1);


        var popup_590602a63be8a52ed42cb41f934e92f8 = L.popup({
  &quot;maxWidth&quot;: &quot;100%&quot;,
});



                var html_25b5539d839be736291a942137667950 = $(`&lt;div id=&quot;html_25b5539d839be736291a942137667950&quot; style=&quot;width: 100.0%; height: 100.0%;&quot;&gt;ZIP 21216&lt;/div&gt;`)[0];
                popup_590602a63be8a52ed42cb41f934e92f8.setContent(html_25b5539d839be736291a942137667950);



        circle_marker_48b398aa2bff230bf07bce78ee4a221a.bindPopup(popup_590602a63be8a52ed42cb41f934e92f8)
        ;




            var circle_marker_b107cead5d80c2bbdbd4f8ef2041ce17 = L.circleMarker(
                [39.28425268579992, -76.65320754361775],
                {&quot;bubblingMouseEvents&quot;: true, &quot;color&quot;: &quot;#4b0082&quot;, &quot;dashArray&quot;: null, &quot;dashOffset&quot;: null, &quot;fill&quot;: true, &quot;fillColor&quot;: &quot;#9370db&quot;, &quot;fillOpacity&quot;: 0.7, &quot;fillRule&quot;: &quot;evenodd&quot;, &quot;lineCap&quot;: &quot;round&quot;, &quot;lineJoin&quot;: &quot;round&quot;, &quot;opacity&quot;: 1.0, &quot;radius&quot;: 27.1416, &quot;stroke&quot;: true, &quot;weight&quot;: 1}
            ).addTo(map_0726b6ba9a4d2cc5792f26bf69fef3e1);


        var popup_1e4fff1aaefca8b14443ef44377dc358 = L.popup({
  &quot;maxWidth&quot;: &quot;100%&quot;,
});



                var html_2195b8b7aec8a8c55c35decd15fea52a = $(`&lt;div id=&quot;html_2195b8b7aec8a8c55c35decd15fea52a&quot; style=&quot;width: 100.0%; height: 100.0%;&quot;&gt;ZIP 21223&lt;/div&gt;`)[0];
                popup_1e4fff1aaefca8b14443ef44377dc358.setContent(html_2195b8b7aec8a8c55c35decd15fea52a);



        circle_marker_b107cead5d80c2bbdbd4f8ef2041ce17.bindPopup(popup_1e4fff1aaefca8b14443ef44377dc358)
        ;




            var circle_marker_d4f609fc3fffbb0b23030e8c80888190 = L.circleMarker(
                [39.2851262363338, -76.69080331614632],
                {&quot;bubblingMouseEvents&quot;: true, &quot;color&quot;: &quot;#4b0082&quot;, &quot;dashArray&quot;: null, &quot;dashOffset&quot;: null, &quot;fill&quot;: true, &quot;fillColor&quot;: &quot;#9370db&quot;, &quot;fillOpacity&quot;: 0.7, &quot;fillRule&quot;: &quot;evenodd&quot;, &quot;lineCap&quot;: &quot;round&quot;, &quot;lineJoin&quot;: &quot;round&quot;, &quot;opacity&quot;: 1.0, &quot;radius&quot;: 29.743333333333332, &quot;stroke&quot;: true, &quot;weight&quot;: 1}
            ).addTo(map_0726b6ba9a4d2cc5792f26bf69fef3e1);


        var popup_b996b2afc2000221091a59d823d5dc12 = L.popup({
  &quot;maxWidth&quot;: &quot;100%&quot;,
});



                var html_e3feaf118e80ff3b1b4be3d47768b20b = $(`&lt;div id=&quot;html_e3feaf118e80ff3b1b4be3d47768b20b&quot; style=&quot;width: 100.0%; height: 100.0%;&quot;&gt;ZIP 21229&lt;/div&gt;`)[0];
                popup_b996b2afc2000221091a59d823d5dc12.setContent(html_e3feaf118e80ff3b1b4be3d47768b20b);



        circle_marker_d4f609fc3fffbb0b23030e8c80888190.bindPopup(popup_b996b2afc2000221091a59d823d5dc12)
        ;




            var circle_marker_b8b1b9cf74cb3370257c9e830d280daa = L.circleMarker(
                [39.266285973530145, -76.6206941310597],
                {&quot;bubblingMouseEvents&quot;: true, &quot;color&quot;: &quot;#4b0082&quot;, &quot;dashArray&quot;: null, &quot;dashOffset&quot;: null, &quot;fill&quot;: true, &quot;fillColor&quot;: &quot;#9370db&quot;, &quot;fillOpacity&quot;: 0.7, &quot;fillRule&quot;: &quot;evenodd&quot;, &quot;lineCap&quot;: &quot;round&quot;, &quot;lineJoin&quot;: &quot;round&quot;, &quot;opacity&quot;: 1.0, &quot;radius&quot;: 28.662133333333333, &quot;stroke&quot;: true, &quot;weight&quot;: 1}
            ).addTo(map_0726b6ba9a4d2cc5792f26bf69fef3e1);


        var popup_bcef5fca709cd5efbac9abb880274e0d = L.popup({
  &quot;maxWidth&quot;: &quot;100%&quot;,
});



                var html_a99056432f42116b375e9864c9f1a292 = $(`&lt;div id=&quot;html_a99056432f42116b375e9864c9f1a292&quot; style=&quot;width: 100.0%; height: 100.0%;&quot;&gt;ZIP 21230&lt;/div&gt;`)[0];
                popup_bcef5fca709cd5efbac9abb880274e0d.setContent(html_a99056432f42116b375e9864c9f1a292);



        circle_marker_b8b1b9cf74cb3370257c9e830d280daa.bindPopup(popup_bcef5fca709cd5efbac9abb880274e0d)
        ;




            var feature_group_84842d008a6f773fe6b54c40c9a5e774 = L.featureGroup(
                {
}
            );


            var marker_056bf807e92f1f878d89572dd2a5f1a8 = L.marker(
                [39.3101783, -76.6180097],
                {
}
            ).addTo(feature_group_84842d008a6f773fe6b54c40c9a5e774);


            var icon_fec6b7031614eda68fe8824571217136 = L.AwesomeMarkers.icon(
                {
  &quot;markerColor&quot;: &quot;green&quot;,
  &quot;iconColor&quot;: &quot;white&quot;,
  &quot;icon&quot;: &quot;heart&quot;,
  &quot;prefix&quot;: &quot;fa&quot;,
  &quot;extraClasses&quot;: &quot;fa-rotate-0&quot;,
}
            );


        var popup_c0bab79ee09d18dacb80a6b81a3ee958 = L.popup({
  &quot;maxWidth&quot;: &quot;100%&quot;,
});



                var html_e9e856ab716402ba9c62695c14261552 = $(`&lt;div id=&quot;html_e9e856ab716402ba9c62695c14261552&quot; style=&quot;width: 100.0%; height: 100.0%;&quot;&gt;Project PLASE - Maryland Ave.&lt;/div&gt;`)[0];
                popup_c0bab79ee09d18dacb80a6b81a3ee958.setContent(html_e9e856ab716402ba9c62695c14261552);



        marker_056bf807e92f1f878d89572dd2a5f1a8.bindPopup(popup_c0bab79ee09d18dacb80a6b81a3ee958)
        ;




                marker_056bf807e92f1f878d89572dd2a5f1a8.setIcon(icon_fec6b7031614eda68fe8824571217136);


            var marker_45567c63a8a6f559459a2b6b30deb8bf = L.marker(
                [39.29734385, -76.61062928705718],
                {
}
            ).addTo(feature_group_84842d008a6f773fe6b54c40c9a5e774);


            var icon_bcbf4f7b601eaaab7962814fcfd9031a = L.AwesomeMarkers.icon(
                {
  &quot;markerColor&quot;: &quot;green&quot;,
  &quot;iconColor&quot;: &quot;white&quot;,
  &quot;icon&quot;: &quot;heart&quot;,
  &quot;prefix&quot;: &quot;fa&quot;,
  &quot;extraClasses&quot;: &quot;fa-rotate-0&quot;,
}
            );


        var popup_28c4b2d496e958e8fd23a975515ab488 = L.popup({
  &quot;maxWidth&quot;: &quot;100%&quot;,
});



                var html_92b3010ac6fd43dbc982431dd8d68fe2 = $(`&lt;div id=&quot;html_92b3010ac6fd43dbc982431dd8d68fe2&quot; style=&quot;width: 100.0%; height: 100.0%;&quot;&gt;Weinberg Housing and Resource Center&lt;/div&gt;`)[0];
                popup_28c4b2d496e958e8fd23a975515ab488.setContent(html_92b3010ac6fd43dbc982431dd8d68fe2);



        marker_45567c63a8a6f559459a2b6b30deb8bf.bindPopup(popup_28c4b2d496e958e8fd23a975515ab488)
        ;




                marker_45567c63a8a6f559459a2b6b30deb8bf.setIcon(icon_bcbf4f7b601eaaab7962814fcfd9031a);


            var marker_a99f7cebb2cbcad8535ff66fea2516fd = L.marker(
                [39.3151035, -76.6187862],
                {
}
            ).addTo(feature_group_84842d008a6f773fe6b54c40c9a5e774);


            var icon_f411f58c4e7340021c7686b9bd709160 = L.AwesomeMarkers.icon(
                {
  &quot;markerColor&quot;: &quot;green&quot;,
  &quot;iconColor&quot;: &quot;white&quot;,
  &quot;icon&quot;: &quot;heart&quot;,
  &quot;prefix&quot;: &quot;fa&quot;,
  &quot;extraClasses&quot;: &quot;fa-rotate-0&quot;,
}
            );


        var popup_a998bf3952d9f48e7724a10c15740ef0 = L.popup({
  &quot;maxWidth&quot;: &quot;100%&quot;,
});



                var html_602b844c0e5291d5447a16ed0603c808 = $(`&lt;div id=&quot;html_602b844c0e5291d5447a16ed0603c808&quot; style=&quot;width: 100.0%; height: 100.0%;&quot;&gt;Franciscan Center&lt;/div&gt;`)[0];
                popup_a998bf3952d9f48e7724a10c15740ef0.setContent(html_602b844c0e5291d5447a16ed0603c808);



        marker_a99f7cebb2cbcad8535ff66fea2516fd.bindPopup(popup_a998bf3952d9f48e7724a10c15740ef0)
        ;




                marker_a99f7cebb2cbcad8535ff66fea2516fd.setIcon(icon_f411f58c4e7340021c7686b9bd709160);


            var marker_fcd71c4c85a58a47dd4adca3adb308c5 = L.marker(
                [39.2949306, -76.6166875],
                {
}
            ).addTo(feature_group_84842d008a6f773fe6b54c40c9a5e774);


            var icon_e1d9c643c773405ee5a85febb8f105c9 = L.AwesomeMarkers.icon(
                {
  &quot;markerColor&quot;: &quot;green&quot;,
  &quot;iconColor&quot;: &quot;white&quot;,
  &quot;icon&quot;: &quot;heart&quot;,
  &quot;prefix&quot;: &quot;fa&quot;,
  &quot;extraClasses&quot;: &quot;fa-rotate-0&quot;,
}
            );


        var popup_30eb9e4ffe8ff59d7ddad6047dbd2db4 = L.popup({
  &quot;maxWidth&quot;: &quot;100%&quot;,
});



                var html_f8dd4055c714486c0c67999466731907 = $(`&lt;div id=&quot;html_f8dd4055c714486c0c67999466731907&quot; style=&quot;width: 100.0%; height: 100.0%;&quot;&gt;Our Daily Bread&lt;/div&gt;`)[0];
                popup_30eb9e4ffe8ff59d7ddad6047dbd2db4.setContent(html_f8dd4055c714486c0c67999466731907);



        marker_fcd71c4c85a58a47dd4adca3adb308c5.bindPopup(popup_30eb9e4ffe8ff59d7ddad6047dbd2db4)
        ;




                marker_fcd71c4c85a58a47dd4adca3adb308c5.setIcon(icon_e1d9c643c773405ee5a85febb8f105c9);


            var marker_47e789a43e8244ca0e3d5dc651d30578 = L.marker(
                [39.2819967, -76.6328189],
                {
}
            ).addTo(feature_group_84842d008a6f773fe6b54c40c9a5e774);


            var icon_fb96c14c3051a30cff6f0c4568a77789 = L.AwesomeMarkers.icon(
                {
  &quot;markerColor&quot;: &quot;green&quot;,
  &quot;iconColor&quot;: &quot;white&quot;,
  &quot;icon&quot;: &quot;heart&quot;,
  &quot;prefix&quot;: &quot;fa&quot;,
  &quot;extraClasses&quot;: &quot;fa-rotate-0&quot;,
}
            );


        var popup_c8f8ea48a87a74544e2f56c7be1e7c0c = L.popup({
  &quot;maxWidth&quot;: &quot;100%&quot;,
});



                var html_33c93d8b7dca0a74dbd33808d5c87e64 = $(`&lt;div id=&quot;html_33c93d8b7dca0a74dbd33808d5c87e64&quot; style=&quot;width: 100.0%; height: 100.0%;&quot;&gt;Paul&#x27;s Place&lt;/div&gt;`)[0];
                popup_c8f8ea48a87a74544e2f56c7be1e7c0c.setContent(html_33c93d8b7dca0a74dbd33808d5c87e64);



        marker_47e789a43e8244ca0e3d5dc651d30578.bindPopup(popup_c8f8ea48a87a74544e2f56c7be1e7c0c)
        ;




                marker_47e789a43e8244ca0e3d5dc651d30578.setIcon(icon_fb96c14c3051a30cff6f0c4568a77789);


            var marker_a69a3534dacacf0e33da58efe02aa40b = L.marker(
                [39.35537105, -76.55801906929733],
                {
}
            ).addTo(feature_group_84842d008a6f773fe6b54c40c9a5e774);


            var icon_91d81bd42e7cfbbbb4bf4d561dcc15c8 = L.AwesomeMarkers.icon(
                {
  &quot;markerColor&quot;: &quot;green&quot;,
  &quot;iconColor&quot;: &quot;white&quot;,
  &quot;icon&quot;: &quot;heart&quot;,
  &quot;prefix&quot;: &quot;fa&quot;,
  &quot;extraClasses&quot;: &quot;fa-rotate-0&quot;,
}
            );


        var popup_a5b1b125a5cbe04922ef72995a97ac5d = L.popup({
  &quot;maxWidth&quot;: &quot;100%&quot;,
});



                var html_108d819c27266653c2fbefd4c8d1f986 = $(`&lt;div id=&quot;html_108d819c27266653c2fbefd4c8d1f986&quot; style=&quot;width: 100.0%; height: 100.0%;&quot;&gt;Harbel Community Organization&lt;/div&gt;`)[0];
                popup_a5b1b125a5cbe04922ef72995a97ac5d.setContent(html_108d819c27266653c2fbefd4c8d1f986);



        marker_a69a3534dacacf0e33da58efe02aa40b.bindPopup(popup_a5b1b125a5cbe04922ef72995a97ac5d)
        ;




                marker_a69a3534dacacf0e33da58efe02aa40b.setIcon(icon_91d81bd42e7cfbbbb4bf4d561dcc15c8);


            var marker_3319c4d339e2a24b4dd34367087deb74 = L.marker(
                [39.319821899999994, -76.62158813219636],
                {
}
            ).addTo(feature_group_84842d008a6f773fe6b54c40c9a5e774);


            var icon_c29a55d5f2a18dc5327ef9eeb160d49d = L.AwesomeMarkers.icon(
                {
  &quot;markerColor&quot;: &quot;green&quot;,
  &quot;iconColor&quot;: &quot;white&quot;,
  &quot;icon&quot;: &quot;heart&quot;,
  &quot;prefix&quot;: &quot;fa&quot;,
  &quot;extraClasses&quot;: &quot;fa-rotate-0&quot;,
}
            );


        var popup_b4bda1e54fe78cca50b9d745c2f72ae3 = L.popup({
  &quot;maxWidth&quot;: &quot;100%&quot;,
});



                var html_062c14fb5b06dd0ef998ccdb08a53aa7 = $(`&lt;div id=&quot;html_062c14fb5b06dd0ef998ccdb08a53aa7&quot; style=&quot;width: 100.0%; height: 100.0%;&quot;&gt;Church of the Guardian Angel&lt;/div&gt;`)[0];
                popup_b4bda1e54fe78cca50b9d745c2f72ae3.setContent(html_062c14fb5b06dd0ef998ccdb08a53aa7);



        marker_3319c4d339e2a24b4dd34367087deb74.bindPopup(popup_b4bda1e54fe78cca50b9d745c2f72ae3)
        ;




                marker_3319c4d339e2a24b4dd34367087deb74.setIcon(icon_c29a55d5f2a18dc5327ef9eeb160d49d);


            var marker_1d5e33e2b5837ca78deb9ebc90c0c3dd = L.marker(
                [39.3258218, -76.6095485],
                {
}
            ).addTo(feature_group_84842d008a6f773fe6b54c40c9a5e774);


            var icon_de7c4653f4f57ffa692859d28cc8d702 = L.AwesomeMarkers.icon(
                {
  &quot;markerColor&quot;: &quot;green&quot;,
  &quot;iconColor&quot;: &quot;white&quot;,
  &quot;icon&quot;: &quot;heart&quot;,
  &quot;prefix&quot;: &quot;fa&quot;,
  &quot;extraClasses&quot;: &quot;fa-rotate-0&quot;,
}
            );


        var popup_58f51ef5dc8a7cf3d6985d06c73925fa = L.popup({
  &quot;maxWidth&quot;: &quot;100%&quot;,
});



                var html_7c70383bdde909acb7907800ca177951 = $(`&lt;div id=&quot;html_7c70383bdde909acb7907800ca177951&quot; style=&quot;width: 100.0%; height: 100.0%;&quot;&gt;Sacred House&lt;/div&gt;`)[0];
                popup_58f51ef5dc8a7cf3d6985d06c73925fa.setContent(html_7c70383bdde909acb7907800ca177951);



        marker_1d5e33e2b5837ca78deb9ebc90c0c3dd.bindPopup(popup_58f51ef5dc8a7cf3d6985d06c73925fa)
        ;




                marker_1d5e33e2b5837ca78deb9ebc90c0c3dd.setIcon(icon_de7c4653f4f57ffa692859d28cc8d702);


            var marker_50df6c3c0e7d3e7197dfb0c730930533 = L.marker(
                [39.3100474, -76.6380548],
                {
}
            ).addTo(feature_group_84842d008a6f773fe6b54c40c9a5e774);


            var icon_f2e567cabd053ecd1ea382a21d49cb1f = L.AwesomeMarkers.icon(
                {
  &quot;markerColor&quot;: &quot;green&quot;,
  &quot;iconColor&quot;: &quot;white&quot;,
  &quot;icon&quot;: &quot;heart&quot;,
  &quot;prefix&quot;: &quot;fa&quot;,
  &quot;extraClasses&quot;: &quot;fa-rotate-0&quot;,
}
            );


        var popup_0e06bc77c11dfb96a4355994cdd94171 = L.popup({
  &quot;maxWidth&quot;: &quot;100%&quot;,
});



                var html_5a0c0d8830472575a609227b974d379f = $(`&lt;div id=&quot;html_5a0c0d8830472575a609227b974d379f&quot; style=&quot;width: 100.0%; height: 100.0%;&quot;&gt;Umar Boxing&lt;/div&gt;`)[0];
                popup_0e06bc77c11dfb96a4355994cdd94171.setContent(html_5a0c0d8830472575a609227b974d379f);



        marker_50df6c3c0e7d3e7197dfb0c730930533.bindPopup(popup_0e06bc77c11dfb96a4355994cdd94171)
        ;




                marker_50df6c3c0e7d3e7197dfb0c730930533.setIcon(icon_f2e567cabd053ecd1ea382a21d49cb1f);


            var marker_610e1b5a686fdfbf3c3ebc5f9897bc1a = L.marker(
                [39.327006299999994, -76.59390961327433],
                {
}
            ).addTo(feature_group_84842d008a6f773fe6b54c40c9a5e774);


            var icon_eda59542deef7cc221c978ad76a572f1 = L.AwesomeMarkers.icon(
                {
  &quot;markerColor&quot;: &quot;green&quot;,
  &quot;iconColor&quot;: &quot;white&quot;,
  &quot;icon&quot;: &quot;heart&quot;,
  &quot;prefix&quot;: &quot;fa&quot;,
  &quot;extraClasses&quot;: &quot;fa-rotate-0&quot;,
}
            );


        var popup_52ec7e6a059340252af9e51079b3f48e = L.popup({
  &quot;maxWidth&quot;: &quot;100%&quot;,
});



                var html_7c0def66ddbe6148f4ce75adff93db6f = $(`&lt;div id=&quot;html_7c0def66ddbe6148f4ce75adff93db6f&quot; style=&quot;width: 100.0%; height: 100.0%;&quot;&gt;Grace Baptist Church&lt;/div&gt;`)[0];
                popup_52ec7e6a059340252af9e51079b3f48e.setContent(html_7c0def66ddbe6148f4ce75adff93db6f);



        marker_610e1b5a686fdfbf3c3ebc5f9897bc1a.bindPopup(popup_52ec7e6a059340252af9e51079b3f48e)
        ;




                marker_610e1b5a686fdfbf3c3ebc5f9897bc1a.setIcon(icon_eda59542deef7cc221c978ad76a572f1);


            var marker_fcd57ed754f442ee0d1799293eb50d6e = L.marker(
                [39.3101783, -76.6180097],
                {
}
            ).addTo(feature_group_84842d008a6f773fe6b54c40c9a5e774);


            var icon_85ec31f1fcd60d8ca8f86199828665b9 = L.AwesomeMarkers.icon(
                {
  &quot;markerColor&quot;: &quot;green&quot;,
  &quot;iconColor&quot;: &quot;white&quot;,
  &quot;icon&quot;: &quot;heart&quot;,
  &quot;prefix&quot;: &quot;fa&quot;,
  &quot;extraClasses&quot;: &quot;fa-rotate-0&quot;,
}
            );


        var popup_eb36a453d48e7c56cad43e67e577a387 = L.popup({
  &quot;maxWidth&quot;: &quot;100%&quot;,
});



                var html_51794995477d53ce053a391ada6938f2 = $(`&lt;div id=&quot;html_51794995477d53ce053a391ada6938f2&quot; style=&quot;width: 100.0%; height: 100.0%;&quot;&gt;Project PLASE&lt;/div&gt;`)[0];
                popup_eb36a453d48e7c56cad43e67e577a387.setContent(html_51794995477d53ce053a391ada6938f2);



        marker_fcd57ed754f442ee0d1799293eb50d6e.bindPopup(popup_eb36a453d48e7c56cad43e67e577a387)
        ;




                marker_fcd57ed754f442ee0d1799293eb50d6e.setIcon(icon_85ec31f1fcd60d8ca8f86199828665b9);


            var marker_3210a12b75cbf20ccdc62775f986a755 = L.marker(
                [39.22421115, -76.68556148122171],
                {
}
            ).addTo(feature_group_84842d008a6f773fe6b54c40c9a5e774);


            var icon_595bcd8cf25fbd619e63b126f0b4cca3 = L.AwesomeMarkers.icon(
                {
  &quot;markerColor&quot;: &quot;green&quot;,
  &quot;iconColor&quot;: &quot;white&quot;,
  &quot;icon&quot;: &quot;heart&quot;,
  &quot;prefix&quot;: &quot;fa&quot;,
  &quot;extraClasses&quot;: &quot;fa-rotate-0&quot;,
}
            );


        var popup_4f8deff1ec812f26b6af6d7acf8ed31a = L.popup({
  &quot;maxWidth&quot;: &quot;100%&quot;,
});



                var html_26dd51605acdcf1f7704712995e60500 = $(`&lt;div id=&quot;html_26dd51605acdcf1f7704712995e60500&quot; style=&quot;width: 100.0%; height: 100.0%;&quot;&gt;Maryland Food Bank&lt;/div&gt;`)[0];
                popup_4f8deff1ec812f26b6af6d7acf8ed31a.setContent(html_26dd51605acdcf1f7704712995e60500);



        marker_3210a12b75cbf20ccdc62775f986a755.bindPopup(popup_4f8deff1ec812f26b6af6d7acf8ed31a)
        ;




                marker_3210a12b75cbf20ccdc62775f986a755.setIcon(icon_595bcd8cf25fbd619e63b126f0b4cca3);


            var marker_54fcbe0cd2e59d91c459663369a69b31 = L.marker(
                [39.29378545, -76.6093553524615],
                {
}
            ).addTo(feature_group_84842d008a6f773fe6b54c40c9a5e774);


            var icon_dbb642c7df79c92e01a22642db26965d = L.AwesomeMarkers.icon(
                {
  &quot;markerColor&quot;: &quot;green&quot;,
  &quot;iconColor&quot;: &quot;white&quot;,
  &quot;icon&quot;: &quot;heart&quot;,
  &quot;prefix&quot;: &quot;fa&quot;,
  &quot;extraClasses&quot;: &quot;fa-rotate-0&quot;,
}
            );


        var popup_9b972b77148c32474fcd2245945cd2bc = L.popup({
  &quot;maxWidth&quot;: &quot;100%&quot;,
});



                var html_8b260702dcd27ff749334b45f1b49014 = $(`&lt;div id=&quot;html_8b260702dcd27ff749334b45f1b49014&quot; style=&quot;width: 100.0%; height: 100.0%;&quot;&gt;Health Care for the Homeless&lt;/div&gt;`)[0];
                popup_9b972b77148c32474fcd2245945cd2bc.setContent(html_8b260702dcd27ff749334b45f1b49014);



        marker_54fcbe0cd2e59d91c459663369a69b31.bindPopup(popup_9b972b77148c32474fcd2245945cd2bc)
        ;




                marker_54fcbe0cd2e59d91c459663369a69b31.setIcon(icon_dbb642c7df79c92e01a22642db26965d);


            var marker_aaed9179e24c8899e5817f61f345c6d6 = L.marker(
                [39.3019953, -76.60288246925838],
                {
}
            ).addTo(feature_group_84842d008a6f773fe6b54c40c9a5e774);


            var icon_fd1cd18e136bc8320667b057d5b5faac = L.AwesomeMarkers.icon(
                {
  &quot;markerColor&quot;: &quot;green&quot;,
  &quot;iconColor&quot;: &quot;white&quot;,
  &quot;icon&quot;: &quot;heart&quot;,
  &quot;prefix&quot;: &quot;fa&quot;,
  &quot;extraClasses&quot;: &quot;fa-rotate-0&quot;,
}
            );


        var popup_8a0924825615eef2b71e03e49bf97fa5 = L.popup({
  &quot;maxWidth&quot;: &quot;100%&quot;,
});



                var html_df9859404838142189800237fb41eefd = $(`&lt;div id=&quot;html_df9859404838142189800237fb41eefd&quot; style=&quot;width: 100.0%; height: 100.0%;&quot;&gt;East Baltimore Medical Center&lt;/div&gt;`)[0];
                popup_8a0924825615eef2b71e03e49bf97fa5.setContent(html_df9859404838142189800237fb41eefd);



        marker_aaed9179e24c8899e5817f61f345c6d6.bindPopup(popup_8a0924825615eef2b71e03e49bf97fa5)
        ;




                marker_aaed9179e24c8899e5817f61f345c6d6.setIcon(icon_fd1cd18e136bc8320667b057d5b5faac);


            var marker_8b19276a858962a1818b61f0bbdde53d = L.marker(
                [39.3480927, -76.6767704],
                {
}
            ).addTo(feature_group_84842d008a6f773fe6b54c40c9a5e774);


            var icon_76e914e088b4b32d1a41709680d93439 = L.AwesomeMarkers.icon(
                {
  &quot;markerColor&quot;: &quot;green&quot;,
  &quot;iconColor&quot;: &quot;white&quot;,
  &quot;icon&quot;: &quot;heart&quot;,
  &quot;prefix&quot;: &quot;fa&quot;,
  &quot;extraClasses&quot;: &quot;fa-rotate-0&quot;,
}
            );


        var popup_0f25bc52b8ed40b8bd3b0f3bf3f5d567 = L.popup({
  &quot;maxWidth&quot;: &quot;100%&quot;,
});



                var html_62ab3c15a8ee18b572bf69b9aff85ed7 = $(`&lt;div id=&quot;html_62ab3c15a8ee18b572bf69b9aff85ed7&quot; style=&quot;width: 100.0%; height: 100.0%;&quot;&gt;Park West Health System | Belvedere Ave.&lt;/div&gt;`)[0];
                popup_0f25bc52b8ed40b8bd3b0f3bf3f5d567.setContent(html_62ab3c15a8ee18b572bf69b9aff85ed7);



        marker_8b19276a858962a1818b61f0bbdde53d.bindPopup(popup_0f25bc52b8ed40b8bd3b0f3bf3f5d567)
        ;




                marker_8b19276a858962a1818b61f0bbdde53d.setIcon(icon_76e914e088b4b32d1a41709680d93439);


            var marker_a3b1a2811b0837a2f2945c8c359407b3 = L.marker(
                [39.3025844, -76.6157514],
                {
}
            ).addTo(feature_group_84842d008a6f773fe6b54c40c9a5e774);


            var icon_e0470d21ac5d7e3270aac635353b9a6d = L.AwesomeMarkers.icon(
                {
  &quot;markerColor&quot;: &quot;green&quot;,
  &quot;iconColor&quot;: &quot;white&quot;,
  &quot;icon&quot;: &quot;heart&quot;,
  &quot;prefix&quot;: &quot;fa&quot;,
  &quot;extraClasses&quot;: &quot;fa-rotate-0&quot;,
}
            );


        var popup_33e6eab1e8e8ccc9dfdba83342ab0f42 = L.popup({
  &quot;maxWidth&quot;: &quot;100%&quot;,
});



                var html_89b8a2449f2076d04e8f6d76a634a96c = $(`&lt;div id=&quot;html_89b8a2449f2076d04e8f6d76a634a96c&quot; style=&quot;width: 100.0%; height: 100.0%;&quot;&gt;Chase Brexton Health Services&lt;/div&gt;`)[0];
                popup_33e6eab1e8e8ccc9dfdba83342ab0f42.setContent(html_89b8a2449f2076d04e8f6d76a634a96c);



        marker_a3b1a2811b0837a2f2945c8c359407b3.bindPopup(popup_33e6eab1e8e8ccc9dfdba83342ab0f42)
        ;




                marker_a3b1a2811b0837a2f2945c8c359407b3.setIcon(icon_e0470d21ac5d7e3270aac635353b9a6d);


            var marker_5a81ae604b9ac6974c42e4faf5169388 = L.marker(
                [39.23793125, -76.6066009837206],
                {
}
            ).addTo(feature_group_84842d008a6f773fe6b54c40c9a5e774);


            var icon_1fe2e32c3021352b48f2f158a4d90ce1 = L.AwesomeMarkers.icon(
                {
  &quot;markerColor&quot;: &quot;green&quot;,
  &quot;iconColor&quot;: &quot;white&quot;,
  &quot;icon&quot;: &quot;heart&quot;,
  &quot;prefix&quot;: &quot;fa&quot;,
  &quot;extraClasses&quot;: &quot;fa-rotate-0&quot;,
}
            );


        var popup_076735efd209e188f734f16aa870d79a = L.popup({
  &quot;maxWidth&quot;: &quot;100%&quot;,
});



                var html_785de0f03978b74a2704c08af5d69870 = $(`&lt;div id=&quot;html_785de0f03978b74a2704c08af5d69870&quot; style=&quot;width: 100.0%; height: 100.0%;&quot;&gt;Brooklyn Library&lt;/div&gt;`)[0];
                popup_076735efd209e188f734f16aa870d79a.setContent(html_785de0f03978b74a2704c08af5d69870);



        marker_5a81ae604b9ac6974c42e4faf5169388.bindPopup(popup_076735efd209e188f734f16aa870d79a)
        ;




                marker_5a81ae604b9ac6974c42e4faf5169388.setIcon(icon_1fe2e32c3021352b48f2f158a4d90ce1);


            var marker_60490a8e03487908ff30504293bc39b6 = L.marker(
                [39.2862365, -76.5667566446291],
                {
}
            ).addTo(feature_group_84842d008a6f773fe6b54c40c9a5e774);


            var icon_1da7efd1d8f473e5840c9d23b37b01f4 = L.AwesomeMarkers.icon(
                {
  &quot;markerColor&quot;: &quot;green&quot;,
  &quot;iconColor&quot;: &quot;white&quot;,
  &quot;icon&quot;: &quot;heart&quot;,
  &quot;prefix&quot;: &quot;fa&quot;,
  &quot;extraClasses&quot;: &quot;fa-rotate-0&quot;,
}
            );


        var popup_a42e5ca1e182702e2391348b88434e1c = L.popup({
  &quot;maxWidth&quot;: &quot;100%&quot;,
});



                var html_b2192d05088ec501b514afd9162fd47c = $(`&lt;div id=&quot;html_b2192d05088ec501b514afd9162fd47c&quot; style=&quot;width: 100.0%; height: 100.0%;&quot;&gt;Southeast Anchor Library&lt;/div&gt;`)[0];
                popup_a42e5ca1e182702e2391348b88434e1c.setContent(html_b2192d05088ec501b514afd9162fd47c);



        marker_60490a8e03487908ff30504293bc39b6.bindPopup(popup_a42e5ca1e182702e2391348b88434e1c)
        ;




                marker_60490a8e03487908ff30504293bc39b6.setIcon(icon_1da7efd1d8f473e5840c9d23b37b01f4);


            var marker_c14588d7be5109443747195d50c94395 = L.marker(
                [39.2505284, -76.6223205],
                {
}
            ).addTo(feature_group_84842d008a6f773fe6b54c40c9a5e774);


            var icon_9b3cba67c01ced5c8667e6e2a0ec9f4f = L.AwesomeMarkers.icon(
                {
  &quot;markerColor&quot;: &quot;green&quot;,
  &quot;iconColor&quot;: &quot;white&quot;,
  &quot;icon&quot;: &quot;heart&quot;,
  &quot;prefix&quot;: &quot;fa&quot;,
  &quot;extraClasses&quot;: &quot;fa-rotate-0&quot;,
}
            );


        var popup_ffd5a983d4bf864fc61f6ce2a1170903 = L.popup({
  &quot;maxWidth&quot;: &quot;100%&quot;,
});



                var html_2abf56cd79854abb04cb6aa36a43a3c5 = $(`&lt;div id=&quot;html_2abf56cd79854abb04cb6aa36a43a3c5&quot; style=&quot;width: 100.0%; height: 100.0%;&quot;&gt;Cherry Hill Library&lt;/div&gt;`)[0];
                popup_ffd5a983d4bf864fc61f6ce2a1170903.setContent(html_2abf56cd79854abb04cb6aa36a43a3c5);



        marker_c14588d7be5109443747195d50c94395.bindPopup(popup_ffd5a983d4bf864fc61f6ce2a1170903)
        ;




                marker_c14588d7be5109443747195d50c94395.setIcon(icon_9b3cba67c01ced5c8667e6e2a0ec9f4f);


            var marker_f4d14b57c2af6f5cc10ff67a0e437c30 = L.marker(
                [39.3098552, -76.6418471231494],
                {
}
            ).addTo(feature_group_84842d008a6f773fe6b54c40c9a5e774);


            var icon_cbba7938664758fdd7f6caa58981b073 = L.AwesomeMarkers.icon(
                {
  &quot;markerColor&quot;: &quot;green&quot;,
  &quot;iconColor&quot;: &quot;white&quot;,
  &quot;icon&quot;: &quot;heart&quot;,
  &quot;prefix&quot;: &quot;fa&quot;,
  &quot;extraClasses&quot;: &quot;fa-rotate-0&quot;,
}
            );


        var popup_816abb733f5937807c5804a11af87a10 = L.popup({
  &quot;maxWidth&quot;: &quot;100%&quot;,
});



                var html_aeeb9ef779207aebe17afe6f65866c1a = $(`&lt;div id=&quot;html_aeeb9ef779207aebe17afe6f65866c1a&quot; style=&quot;width: 100.0%; height: 100.0%;&quot;&gt;Pennsylvania Avenue Library&lt;/div&gt;`)[0];
                popup_816abb733f5937807c5804a11af87a10.setContent(html_aeeb9ef779207aebe17afe6f65866c1a);



        marker_f4d14b57c2af6f5cc10ff67a0e437c30.bindPopup(popup_816abb733f5937807c5804a11af87a10)
        ;




                marker_f4d14b57c2af6f5cc10ff67a0e437c30.setIcon(icon_cbba7938664758fdd7f6caa58981b073);


            var marker_fec49ab2cbf86bdd74948bc4c496a229 = L.marker(
                [39.2946388, -76.59930889030557],
                {
}
            ).addTo(feature_group_84842d008a6f773fe6b54c40c9a5e774);


            var icon_4c7f6de5c0e9a94f1031f9e5261b53a6 = L.AwesomeMarkers.icon(
                {
  &quot;markerColor&quot;: &quot;green&quot;,
  &quot;iconColor&quot;: &quot;white&quot;,
  &quot;icon&quot;: &quot;heart&quot;,
  &quot;prefix&quot;: &quot;fa&quot;,
  &quot;extraClasses&quot;: &quot;fa-rotate-0&quot;,
}
            );


        var popup_c08fc69ab0cdc0408a827c4462ec31d2 = L.popup({
  &quot;maxWidth&quot;: &quot;100%&quot;,
});



                var html_5a2824e93dd2d77367194e4cc153a0aa = $(`&lt;div id=&quot;html_5a2824e93dd2d77367194e4cc153a0aa&quot; style=&quot;width: 100.0%; height: 100.0%;&quot;&gt;Orleans Street Library&lt;/div&gt;`)[0];
                popup_c08fc69ab0cdc0408a827c4462ec31d2.setContent(html_5a2824e93dd2d77367194e4cc153a0aa);



        marker_fec49ab2cbf86bdd74948bc4c496a229.bindPopup(popup_c08fc69ab0cdc0408a827c4462ec31d2)
        ;




                marker_fec49ab2cbf86bdd74948bc4c496a229.setIcon(icon_4c7f6de5c0e9a94f1031f9e5261b53a6);


            var marker_7b3dc05c43e2411730d30ba1b8867e76 = L.marker(
                [39.35537105, -76.55801906929733],
                {
}
            ).addTo(feature_group_84842d008a6f773fe6b54c40c9a5e774);


            var icon_666200e2057b717fb429b0d7c16223ae = L.AwesomeMarkers.icon(
                {
  &quot;markerColor&quot;: &quot;green&quot;,
  &quot;iconColor&quot;: &quot;white&quot;,
  &quot;icon&quot;: &quot;heart&quot;,
  &quot;prefix&quot;: &quot;fa&quot;,
  &quot;extraClasses&quot;: &quot;fa-rotate-0&quot;,
}
            );


        var popup_53f5401e79e3e078d084eff0ee1de35b = L.popup({
  &quot;maxWidth&quot;: &quot;100%&quot;,
});



                var html_8cc0df5b9437c1140939c22ae2262638 = $(`&lt;div id=&quot;html_8cc0df5b9437c1140939c22ae2262638&quot; style=&quot;width: 100.0%; height: 100.0%;&quot;&gt;Harbel Community Organization&lt;/div&gt;`)[0];
                popup_53f5401e79e3e078d084eff0ee1de35b.setContent(html_8cc0df5b9437c1140939c22ae2262638);



        marker_7b3dc05c43e2411730d30ba1b8867e76.bindPopup(popup_53f5401e79e3e078d084eff0ee1de35b)
        ;




                marker_7b3dc05c43e2411730d30ba1b8867e76.setIcon(icon_666200e2057b717fb429b0d7c16223ae);


            var marker_392997f081affa42ab054aba7523761b = L.marker(
                [39.288986, -76.64942222258527],
                {
}
            ).addTo(feature_group_84842d008a6f773fe6b54c40c9a5e774);


            var icon_00dba36270ad546a5763c89cc20a6190 = L.AwesomeMarkers.icon(
                {
  &quot;markerColor&quot;: &quot;green&quot;,
  &quot;iconColor&quot;: &quot;white&quot;,
  &quot;icon&quot;: &quot;heart&quot;,
  &quot;prefix&quot;: &quot;fa&quot;,
  &quot;extraClasses&quot;: &quot;fa-rotate-0&quot;,
}
            );


        var popup_e4c5e3599392a8b949551fa2af425a4c = L.popup({
  &quot;maxWidth&quot;: &quot;100%&quot;,
});



                var html_5e2a72d9bd99b4af7c94e3bb53d57e82 = $(`&lt;div id=&quot;html_5e2a72d9bd99b4af7c94e3bb53d57e82&quot; style=&quot;width: 100.0%; height: 100.0%;&quot;&gt;Grace Medical Center&lt;/div&gt;`)[0];
                popup_e4c5e3599392a8b949551fa2af425a4c.setContent(html_5e2a72d9bd99b4af7c94e3bb53d57e82);



        marker_392997f081affa42ab054aba7523761b.bindPopup(popup_e4c5e3599392a8b949551fa2af425a4c)
        ;




                marker_392997f081affa42ab054aba7523761b.setIcon(icon_00dba36270ad546a5763c89cc20a6190);


            var marker_7d0807e76fc03e424dc69da21a9412ef = L.marker(
                [39.35289645, -76.66228716247511],
                {
}
            ).addTo(feature_group_84842d008a6f773fe6b54c40c9a5e774);


            var icon_3faffe95ed5f4cc79dd8c0997ae9be3a = L.AwesomeMarkers.icon(
                {
  &quot;markerColor&quot;: &quot;green&quot;,
  &quot;iconColor&quot;: &quot;white&quot;,
  &quot;icon&quot;: &quot;heart&quot;,
  &quot;prefix&quot;: &quot;fa&quot;,
  &quot;extraClasses&quot;: &quot;fa-rotate-0&quot;,
}
            );


        var popup_3f4858041144ab17a7f0525c03c5cbb5 = L.popup({
  &quot;maxWidth&quot;: &quot;100%&quot;,
});



                var html_4c398dca4dcb1a9e17418725392ff56d = $(`&lt;div id=&quot;html_4c398dca4dcb1a9e17418725392ff56d&quot; style=&quot;width: 100.0%; height: 100.0%;&quot;&gt;Sinai Hospital&lt;/div&gt;`)[0];
                popup_3f4858041144ab17a7f0525c03c5cbb5.setContent(html_4c398dca4dcb1a9e17418725392ff56d);



        marker_7d0807e76fc03e424dc69da21a9412ef.bindPopup(popup_3f4858041144ab17a7f0525c03c5cbb5)
        ;




                marker_7d0807e76fc03e424dc69da21a9412ef.setIcon(icon_3faffe95ed5f4cc79dd8c0997ae9be3a);


            var marker_cd663ba45f389209fb2045ea36b0c42c = L.marker(
                [39.35412555, -76.66539357378952],
                {
}
            ).addTo(feature_group_84842d008a6f773fe6b54c40c9a5e774);


            var icon_c5aee8dae264b32788b7c8164b47c246 = L.AwesomeMarkers.icon(
                {
  &quot;markerColor&quot;: &quot;green&quot;,
  &quot;iconColor&quot;: &quot;white&quot;,
  &quot;icon&quot;: &quot;heart&quot;,
  &quot;prefix&quot;: &quot;fa&quot;,
  &quot;extraClasses&quot;: &quot;fa-rotate-0&quot;,
}
            );


        var popup_0b7306b2df2555c4a53f018817af74a8 = L.popup({
  &quot;maxWidth&quot;: &quot;100%&quot;,
});



                var html_ff51243700e7554bae5c57397ae5daa7 = $(`&lt;div id=&quot;html_ff51243700e7554bae5c57397ae5daa7&quot; style=&quot;width: 100.0%; height: 100.0%;&quot;&gt;Levindale Hebrew Geriatric Center and Hospital&lt;/div&gt;`)[0];
                popup_0b7306b2df2555c4a53f018817af74a8.setContent(html_ff51243700e7554bae5c57397ae5daa7);



        marker_cd663ba45f389209fb2045ea36b0c42c.bindPopup(popup_0b7306b2df2555c4a53f018817af74a8)
        ;




                marker_cd663ba45f389209fb2045ea36b0c42c.setIcon(icon_c5aee8dae264b32788b7c8164b47c246);


            var marker_591dd0b4c1d056fe13d8b144510b0a05 = L.marker(
                [39.29121285, -76.5490695530589],
                {
}
            ).addTo(feature_group_84842d008a6f773fe6b54c40c9a5e774);


            var icon_af569e678718787ce75825691641c284 = L.AwesomeMarkers.icon(
                {
  &quot;markerColor&quot;: &quot;green&quot;,
  &quot;iconColor&quot;: &quot;white&quot;,
  &quot;icon&quot;: &quot;heart&quot;,
  &quot;prefix&quot;: &quot;fa&quot;,
  &quot;extraClasses&quot;: &quot;fa-rotate-0&quot;,
}
            );


        var popup_0d2c9e19e282534c2be736b83d34a843 = L.popup({
  &quot;maxWidth&quot;: &quot;100%&quot;,
});



                var html_6a577d506ff795634617b097325c89dd = $(`&lt;div id=&quot;html_6a577d506ff795634617b097325c89dd&quot; style=&quot;width: 100.0%; height: 100.0%;&quot;&gt;Johns Hopkins Bayview Medical Center&lt;/div&gt;`)[0];
                popup_0d2c9e19e282534c2be736b83d34a843.setContent(html_6a577d506ff795634617b097325c89dd);



        marker_591dd0b4c1d056fe13d8b144510b0a05.bindPopup(popup_0d2c9e19e282534c2be736b83d34a843)
        ;




                marker_591dd0b4c1d056fe13d8b144510b0a05.setIcon(icon_af569e678718787ce75825691641c284);


            var marker_02c712e089d6f2acf6bedc8a00d46782 = L.marker(
                [39.296439199999995, -76.59239403267401],
                {
}
            ).addTo(feature_group_84842d008a6f773fe6b54c40c9a5e774);


            var icon_8e6d3740c194d8f3d7755d0c9116a5e0 = L.AwesomeMarkers.icon(
                {
  &quot;markerColor&quot;: &quot;green&quot;,
  &quot;iconColor&quot;: &quot;white&quot;,
  &quot;icon&quot;: &quot;heart&quot;,
  &quot;prefix&quot;: &quot;fa&quot;,
  &quot;extraClasses&quot;: &quot;fa-rotate-0&quot;,
}
            );


        var popup_f880e4e405a879a4eca7605dc3ecd2bc = L.popup({
  &quot;maxWidth&quot;: &quot;100%&quot;,
});



                var html_591c912c57f45611b6b895d57f6b134b = $(`&lt;div id=&quot;html_591c912c57f45611b6b895d57f6b134b&quot; style=&quot;width: 100.0%; height: 100.0%;&quot;&gt;Johns Hopkins Hospital&lt;/div&gt;`)[0];
                popup_f880e4e405a879a4eca7605dc3ecd2bc.setContent(html_591c912c57f45611b6b895d57f6b134b);



        marker_02c712e089d6f2acf6bedc8a00d46782.bindPopup(popup_f880e4e405a879a4eca7605dc3ecd2bc)
        ;




                marker_02c712e089d6f2acf6bedc8a00d46782.setIcon(icon_8e6d3740c194d8f3d7755d0c9116a5e0);


            var marker_a8d20e14d0ab8d212ffc498da1d7e85e = L.marker(
                [39.2998731, -76.59327771844121],
                {
}
            ).addTo(feature_group_84842d008a6f773fe6b54c40c9a5e774);


            var icon_3d0e1708d86873d4e5dd17f887d96b23 = L.AwesomeMarkers.icon(
                {
  &quot;markerColor&quot;: &quot;green&quot;,
  &quot;iconColor&quot;: &quot;white&quot;,
  &quot;icon&quot;: &quot;heart&quot;,
  &quot;prefix&quot;: &quot;fa&quot;,
  &quot;extraClasses&quot;: &quot;fa-rotate-0&quot;,
}
            );


        var popup_67c931f634aac9a0014906bff05d6e9b = L.popup({
  &quot;maxWidth&quot;: &quot;100%&quot;,
});



                var html_a9240fd6981f421a464f3f1bd6afbf9c = $(`&lt;div id=&quot;html_a9240fd6981f421a464f3f1bd6afbf9c&quot; style=&quot;width: 100.0%; height: 100.0%;&quot;&gt;Kennedy Krieger Institute&lt;/div&gt;`)[0];
                popup_67c931f634aac9a0014906bff05d6e9b.setContent(html_a9240fd6981f421a464f3f1bd6afbf9c);



        marker_a8d20e14d0ab8d212ffc498da1d7e85e.bindPopup(popup_67c931f634aac9a0014906bff05d6e9b)
        ;




                marker_a8d20e14d0ab8d212ffc498da1d7e85e.setIcon(icon_3d0e1708d86873d4e5dd17f887d96b23);


            var marker_0353bd96dcb5205df09c9eaff2a7279f = L.marker(
                [39.3575548, -76.58584727178213],
                {
}
            ).addTo(feature_group_84842d008a6f773fe6b54c40c9a5e774);


            var icon_12e64f66cd39f92d396bee0fe904ec22 = L.AwesomeMarkers.icon(
                {
  &quot;markerColor&quot;: &quot;green&quot;,
  &quot;iconColor&quot;: &quot;white&quot;,
  &quot;icon&quot;: &quot;heart&quot;,
  &quot;prefix&quot;: &quot;fa&quot;,
  &quot;extraClasses&quot;: &quot;fa-rotate-0&quot;,
}
            );


        var popup_5416aacc4beb67efa76229ceb6efa9d3 = L.popup({
  &quot;maxWidth&quot;: &quot;100%&quot;,
});



                var html_661b17f680968b2cb17e7871c4c21f4e = $(`&lt;div id=&quot;html_661b17f680968b2cb17e7871c4c21f4e&quot; style=&quot;width: 100.0%; height: 100.0%;&quot;&gt;MedStar Good Samaritan Hospital&lt;/div&gt;`)[0];
                popup_5416aacc4beb67efa76229ceb6efa9d3.setContent(html_661b17f680968b2cb17e7871c4c21f4e);



        marker_0353bd96dcb5205df09c9eaff2a7279f.bindPopup(popup_5416aacc4beb67efa76229ceb6efa9d3)
        ;




                marker_0353bd96dcb5205df09c9eaff2a7279f.setIcon(icon_12e64f66cd39f92d396bee0fe904ec22);


            var marker_7133d7d057d83f76018ab195703eaad6 = L.marker(
                [39.25184350000001, -76.61403126369895],
                {
}
            ).addTo(feature_group_84842d008a6f773fe6b54c40c9a5e774);


            var icon_f099f6af2e0dabbbf7bd6e8affbf0916 = L.AwesomeMarkers.icon(
                {
  &quot;markerColor&quot;: &quot;green&quot;,
  &quot;iconColor&quot;: &quot;white&quot;,
  &quot;icon&quot;: &quot;heart&quot;,
  &quot;prefix&quot;: &quot;fa&quot;,
  &quot;extraClasses&quot;: &quot;fa-rotate-0&quot;,
}
            );


        var popup_d9b40e9e59d919d83e67dde77043eb9c = L.popup({
  &quot;maxWidth&quot;: &quot;100%&quot;,
});



                var html_69fae0c58bf954472915d4276eba0ce4 = $(`&lt;div id=&quot;html_69fae0c58bf954472915d4276eba0ce4&quot; style=&quot;width: 100.0%; height: 100.0%;&quot;&gt;MedStar Harbor Hospital&lt;/div&gt;`)[0];
                popup_d9b40e9e59d919d83e67dde77043eb9c.setContent(html_69fae0c58bf954472915d4276eba0ce4);



        marker_7133d7d057d83f76018ab195703eaad6.bindPopup(popup_d9b40e9e59d919d83e67dde77043eb9c)
        ;




                marker_7133d7d057d83f76018ab195703eaad6.setIcon(icon_f099f6af2e0dabbbf7bd6e8affbf0916);


            var marker_04ede81bbdf5b02bc182bfa22eb811a4 = L.marker(
                [39.3295258, -76.61483957750231],
                {
}
            ).addTo(feature_group_84842d008a6f773fe6b54c40c9a5e774);


            var icon_adc05abbed0c1c302c0f08335df1586f = L.AwesomeMarkers.icon(
                {
  &quot;markerColor&quot;: &quot;green&quot;,
  &quot;iconColor&quot;: &quot;white&quot;,
  &quot;icon&quot;: &quot;heart&quot;,
  &quot;prefix&quot;: &quot;fa&quot;,
  &quot;extraClasses&quot;: &quot;fa-rotate-0&quot;,
}
            );


        var popup_9e643695f74912e33f25bb5237af4d13 = L.popup({
  &quot;maxWidth&quot;: &quot;100%&quot;,
});



                var html_93764ee27d79302f9a60a5d4a9201afc = $(`&lt;div id=&quot;html_93764ee27d79302f9a60a5d4a9201afc&quot; style=&quot;width: 100.0%; height: 100.0%;&quot;&gt;MedStar Union Memorial Hospital&lt;/div&gt;`)[0];
                popup_9e643695f74912e33f25bb5237af4d13.setContent(html_93764ee27d79302f9a60a5d4a9201afc);



        marker_04ede81bbdf5b02bc182bfa22eb811a4.bindPopup(popup_9e643695f74912e33f25bb5237af4d13)
        ;




                marker_04ede81bbdf5b02bc182bfa22eb811a4.setIcon(icon_adc05abbed0c1c302c0f08335df1586f);


            var marker_57837dd4b57d235025759dd46bb8f23e = L.marker(
                [39.2932695, -76.61280493859464],
                {
}
            ).addTo(feature_group_84842d008a6f773fe6b54c40c9a5e774);


            var icon_da0094eb6ad4708cd10d766bbbaf9cfc = L.AwesomeMarkers.icon(
                {
  &quot;markerColor&quot;: &quot;green&quot;,
  &quot;iconColor&quot;: &quot;white&quot;,
  &quot;icon&quot;: &quot;heart&quot;,
  &quot;prefix&quot;: &quot;fa&quot;,
  &quot;extraClasses&quot;: &quot;fa-rotate-0&quot;,
}
            );


        var popup_886df2f24f249aafbef8dcfbb630fdfb = L.popup({
  &quot;maxWidth&quot;: &quot;100%&quot;,
});



                var html_0b99d36ba199d4fa5ee542c14f6db100 = $(`&lt;div id=&quot;html_0b99d36ba199d4fa5ee542c14f6db100&quot; style=&quot;width: 100.0%; height: 100.0%;&quot;&gt;Mercy Medical Center&lt;/div&gt;`)[0];
                popup_886df2f24f249aafbef8dcfbb630fdfb.setContent(html_0b99d36ba199d4fa5ee542c14f6db100);



        marker_57837dd4b57d235025759dd46bb8f23e.bindPopup(popup_886df2f24f249aafbef8dcfbb630fdfb)
        ;




                marker_57837dd4b57d235025759dd46bb8f23e.setIcon(icon_da0094eb6ad4708cd10d766bbbaf9cfc);


            var marker_ad904f0e1aeb1be89b1226e6da4c7764 = L.marker(
                [39.3630752, -76.65282334820517],
                {
}
            ).addTo(feature_group_84842d008a6f773fe6b54c40c9a5e774);


            var icon_0eb52fda30b240724c17cdd9a5e9519b = L.AwesomeMarkers.icon(
                {
  &quot;markerColor&quot;: &quot;green&quot;,
  &quot;iconColor&quot;: &quot;white&quot;,
  &quot;icon&quot;: &quot;heart&quot;,
  &quot;prefix&quot;: &quot;fa&quot;,
  &quot;extraClasses&quot;: &quot;fa-rotate-0&quot;,
}
            );


        var popup_a8077e43efe38854705f277c1ddf90a6 = L.popup({
  &quot;maxWidth&quot;: &quot;100%&quot;,
});



                var html_f0d3aa3be07264649e222942dd208816 = $(`&lt;div id=&quot;html_f0d3aa3be07264649e222942dd208816&quot; style=&quot;width: 100.0%; height: 100.0%;&quot;&gt;Mt. Washington Pediatric Hospital&lt;/div&gt;`)[0];
                popup_a8077e43efe38854705f277c1ddf90a6.setContent(html_f0d3aa3be07264649e222942dd208816);



        marker_ad904f0e1aeb1be89b1226e6da4c7764.bindPopup(popup_a8077e43efe38854705f277c1ddf90a6)
        ;




                marker_ad904f0e1aeb1be89b1226e6da4c7764.setIcon(icon_0eb52fda30b240724c17cdd9a5e9519b);


            var marker_dd16d89e3aada594ec7af99ce0208f62 = L.marker(
                [39.28827365, -76.62439437177966],
                {
}
            ).addTo(feature_group_84842d008a6f773fe6b54c40c9a5e774);


            var icon_d7a3eaa73c63a5cbcb0ac881db046276 = L.AwesomeMarkers.icon(
                {
  &quot;markerColor&quot;: &quot;green&quot;,
  &quot;iconColor&quot;: &quot;white&quot;,
  &quot;icon&quot;: &quot;heart&quot;,
  &quot;prefix&quot;: &quot;fa&quot;,
  &quot;extraClasses&quot;: &quot;fa-rotate-0&quot;,
}
            );


        var popup_a86ce9ad0fe5a664db285c946c2f7247 = L.popup({
  &quot;maxWidth&quot;: &quot;100%&quot;,
});



                var html_3f443eca4ba7d81e0eea780bb105b1ca = $(`&lt;div id=&quot;html_3f443eca4ba7d81e0eea780bb105b1ca&quot; style=&quot;width: 100.0%; height: 100.0%;&quot;&gt;University of Maryland Medical Center&lt;/div&gt;`)[0];
                popup_a86ce9ad0fe5a664db285c946c2f7247.setContent(html_3f443eca4ba7d81e0eea780bb105b1ca);



        marker_dd16d89e3aada594ec7af99ce0208f62.bindPopup(popup_a86ce9ad0fe5a664db285c946c2f7247)
        ;




                marker_dd16d89e3aada594ec7af99ce0208f62.setIcon(icon_d7a3eaa73c63a5cbcb0ac881db046276);


            var marker_12469d37f268146b7463b0fa728c678e = L.marker(
                [39.3138027, -76.6164449],
                {
}
            ).addTo(feature_group_84842d008a6f773fe6b54c40c9a5e774);


            var icon_2eef4dbf3566885eebe0252bb04cbecd = L.AwesomeMarkers.icon(
                {
  &quot;markerColor&quot;: &quot;green&quot;,
  &quot;iconColor&quot;: &quot;white&quot;,
  &quot;icon&quot;: &quot;heart&quot;,
  &quot;prefix&quot;: &quot;fa&quot;,
  &quot;extraClasses&quot;: &quot;fa-rotate-0&quot;,
}
            );


        var popup_39656b5ef39d5cc03c2f492d3ba07543 = L.popup({
  &quot;maxWidth&quot;: &quot;100%&quot;,
});



                var html_70bf2a708c17e1e78bbf625742f958e4 = $(`&lt;div id=&quot;html_70bf2a708c17e1e78bbf625742f958e4&quot; style=&quot;width: 100.0%; height: 100.0%;&quot;&gt;Baltimore Safe Haven&lt;/div&gt;`)[0];
                popup_39656b5ef39d5cc03c2f492d3ba07543.setContent(html_70bf2a708c17e1e78bbf625742f958e4);



        marker_12469d37f268146b7463b0fa728c678e.bindPopup(popup_39656b5ef39d5cc03c2f492d3ba07543)
        ;




                marker_12469d37f268146b7463b0fa728c678e.setIcon(icon_2eef4dbf3566885eebe0252bb04cbecd);


            var marker_9e33b5f9cca7381ce165a05a122373a3 = L.marker(
                [39.3156377, -76.6165108],
                {
}
            ).addTo(feature_group_84842d008a6f773fe6b54c40c9a5e774);


            var icon_1e22d21cc0ad21cbbace9326897bb319 = L.AwesomeMarkers.icon(
                {
  &quot;markerColor&quot;: &quot;green&quot;,
  &quot;iconColor&quot;: &quot;white&quot;,
  &quot;icon&quot;: &quot;heart&quot;,
  &quot;prefix&quot;: &quot;fa&quot;,
  &quot;extraClasses&quot;: &quot;fa-rotate-0&quot;,
}
            );


        var popup_a78e6815eced7fa87e26694a2e6d4d29 = L.popup({
  &quot;maxWidth&quot;: &quot;100%&quot;,
});



                var html_314791ce4a14898b9c54cf56852205f6 = $(`&lt;div id=&quot;html_314791ce4a14898b9c54cf56852205f6&quot; style=&quot;width: 100.0%; height: 100.0%;&quot;&gt;St. Vincent de Paul of Baltimore&lt;/div&gt;`)[0];
                popup_a78e6815eced7fa87e26694a2e6d4d29.setContent(html_314791ce4a14898b9c54cf56852205f6);



        marker_9e33b5f9cca7381ce165a05a122373a3.bindPopup(popup_a78e6815eced7fa87e26694a2e6d4d29)
        ;




                marker_9e33b5f9cca7381ce165a05a122373a3.setIcon(icon_1e22d21cc0ad21cbbace9326897bb319);


            var marker_c9f62831a0f3a9818da2a7dd934ae5f6 = L.marker(
                [39.3158175, -76.6167198],
                {
}
            ).addTo(feature_group_84842d008a6f773fe6b54c40c9a5e774);


            var icon_c1bd635a1837ed4213e0f38e7ee24536 = L.AwesomeMarkers.icon(
                {
  &quot;markerColor&quot;: &quot;green&quot;,
  &quot;iconColor&quot;: &quot;white&quot;,
  &quot;icon&quot;: &quot;heart&quot;,
  &quot;prefix&quot;: &quot;fa&quot;,
  &quot;extraClasses&quot;: &quot;fa-rotate-0&quot;,
}
            );


        var popup_624c7abd61f754f1fe4007912d53245a = L.popup({
  &quot;maxWidth&quot;: &quot;100%&quot;,
});



                var html_8b0f2a41a7188c2b86f2d4e2a0a2a29a = $(`&lt;div id=&quot;html_8b0f2a41a7188c2b86f2d4e2a0a2a29a&quot; style=&quot;width: 100.0%; height: 100.0%;&quot;&gt;Youth Empowered Society (YES)&lt;/div&gt;`)[0];
                popup_624c7abd61f754f1fe4007912d53245a.setContent(html_8b0f2a41a7188c2b86f2d4e2a0a2a29a);



        marker_c9f62831a0f3a9818da2a7dd934ae5f6.bindPopup(popup_624c7abd61f754f1fe4007912d53245a)
        ;




                marker_c9f62831a0f3a9818da2a7dd934ae5f6.setIcon(icon_c1bd635a1837ed4213e0f38e7ee24536);


            var marker_7a35fdfff5ec8f0b60724b1bde1ce6e3 = L.marker(
                [39.3169269, -76.6538936],
                {
}
            ).addTo(feature_group_84842d008a6f773fe6b54c40c9a5e774);


            var icon_fcea37cc59bd89bb45999e5b865a52cd = L.AwesomeMarkers.icon(
                {
  &quot;markerColor&quot;: &quot;green&quot;,
  &quot;iconColor&quot;: &quot;white&quot;,
  &quot;icon&quot;: &quot;heart&quot;,
  &quot;prefix&quot;: &quot;fa&quot;,
  &quot;extraClasses&quot;: &quot;fa-rotate-0&quot;,
}
            );


        var popup_4a53e5d3114a49d27a9260b274f55437 = L.popup({
  &quot;maxWidth&quot;: &quot;100%&quot;,
});



                var html_9a5e94d2e00357abd1c9bede634bf5d6 = $(`&lt;div id=&quot;html_9a5e94d2e00357abd1c9bede634bf5d6&quot; style=&quot;width: 100.0%; height: 100.0%;&quot;&gt;Northwest One-Stop Career Center&lt;/div&gt;`)[0];
                popup_4a53e5d3114a49d27a9260b274f55437.setContent(html_9a5e94d2e00357abd1c9bede634bf5d6);



        marker_7a35fdfff5ec8f0b60724b1bde1ce6e3.bindPopup(popup_4a53e5d3114a49d27a9260b274f55437)
        ;




                marker_7a35fdfff5ec8f0b60724b1bde1ce6e3.setIcon(icon_fcea37cc59bd89bb45999e5b865a52cd);


            var marker_cf3c8ce99410a9abf98444b6f57deae2 = L.marker(
                [39.2997678, -76.5751002691712],
                {
}
            ).addTo(feature_group_84842d008a6f773fe6b54c40c9a5e774);


            var icon_cb7780e2b3540cf703e11019e64633a2 = L.AwesomeMarkers.icon(
                {
  &quot;markerColor&quot;: &quot;green&quot;,
  &quot;iconColor&quot;: &quot;white&quot;,
  &quot;icon&quot;: &quot;heart&quot;,
  &quot;prefix&quot;: &quot;fa&quot;,
  &quot;extraClasses&quot;: &quot;fa-rotate-0&quot;,
}
            );


        var popup_69e62c519a13d3a974ec6697bbf517a4 = L.popup({
  &quot;maxWidth&quot;: &quot;100%&quot;,
});



                var html_7e3dad8e81abd350446ecf2567352c5f = $(`&lt;div id=&quot;html_7e3dad8e81abd350446ecf2567352c5f&quot; style=&quot;width: 100.0%; height: 100.0%;&quot;&gt;Eastside One-Stop Career Center&lt;/div&gt;`)[0];
                popup_69e62c519a13d3a974ec6697bbf517a4.setContent(html_7e3dad8e81abd350446ecf2567352c5f);



        marker_cf3c8ce99410a9abf98444b6f57deae2.bindPopup(popup_69e62c519a13d3a974ec6697bbf517a4)
        ;




                marker_cf3c8ce99410a9abf98444b6f57deae2.setIcon(icon_cb7780e2b3540cf703e11019e64633a2);


            var marker_6bb0e0a9a5dbc8cb8e3498fa7e67fc44 = L.marker(
                [39.2819967, -76.6328189],
                {
}
            ).addTo(feature_group_84842d008a6f773fe6b54c40c9a5e774);


            var icon_83b3598f7bab767f95a6916735341ae5 = L.AwesomeMarkers.icon(
                {
  &quot;markerColor&quot;: &quot;green&quot;,
  &quot;iconColor&quot;: &quot;white&quot;,
  &quot;icon&quot;: &quot;heart&quot;,
  &quot;prefix&quot;: &quot;fa&quot;,
  &quot;extraClasses&quot;: &quot;fa-rotate-0&quot;,
}
            );


        var popup_7b3e7b78711c5f459b633ac0e981d820 = L.popup({
  &quot;maxWidth&quot;: &quot;100%&quot;,
});



                var html_43194122881fd3ae003202817da9c03c = $(`&lt;div id=&quot;html_43194122881fd3ae003202817da9c03c&quot; style=&quot;width: 100.0%; height: 100.0%;&quot;&gt;Paul&#x27;s Place&lt;/div&gt;`)[0];
                popup_7b3e7b78711c5f459b633ac0e981d820.setContent(html_43194122881fd3ae003202817da9c03c);



        marker_6bb0e0a9a5dbc8cb8e3498fa7e67fc44.bindPopup(popup_7b3e7b78711c5f459b633ac0e981d820)
        ;




                marker_6bb0e0a9a5dbc8cb8e3498fa7e67fc44.setIcon(icon_83b3598f7bab767f95a6916735341ae5);


            var marker_a735c89d37515c04bd4f576d556d617d = L.marker(
                [39.3256619, -76.6030961],
                {
}
            ).addTo(feature_group_84842d008a6f773fe6b54c40c9a5e774);


            var icon_3cade699029dff0b5872fb5571dd7e97 = L.AwesomeMarkers.icon(
                {
  &quot;markerColor&quot;: &quot;green&quot;,
  &quot;iconColor&quot;: &quot;white&quot;,
  &quot;icon&quot;: &quot;heart&quot;,
  &quot;prefix&quot;: &quot;fa&quot;,
  &quot;extraClasses&quot;: &quot;fa-rotate-0&quot;,
}
            );


        var popup_1a4f40141deff7247b6566830ee0f89c = L.popup({
  &quot;maxWidth&quot;: &quot;100%&quot;,
});



                var html_47a467b29fa1f4b523bc44fb67fd7b5c = $(`&lt;div id=&quot;html_47a467b29fa1f4b523bc44fb67fd7b5c&quot; style=&quot;width: 100.0%; height: 100.0%;&quot;&gt;Marian House&lt;/div&gt;`)[0];
                popup_1a4f40141deff7247b6566830ee0f89c.setContent(html_47a467b29fa1f4b523bc44fb67fd7b5c);



        marker_a735c89d37515c04bd4f576d556d617d.bindPopup(popup_1a4f40141deff7247b6566830ee0f89c)
        ;




                marker_a735c89d37515c04bd4f576d556d617d.setIcon(icon_3cade699029dff0b5872fb5571dd7e97);


            var marker_c2db52b96962f5f5f082982f0b6f940f = L.marker(
                [39.3179037, -76.61004974068197],
                {
}
            ).addTo(feature_group_84842d008a6f773fe6b54c40c9a5e774);


            var icon_719897d215f2e3adab7f0c35d0fc59a9 = L.AwesomeMarkers.icon(
                {
  &quot;markerColor&quot;: &quot;green&quot;,
  &quot;iconColor&quot;: &quot;white&quot;,
  &quot;icon&quot;: &quot;heart&quot;,
  &quot;prefix&quot;: &quot;fa&quot;,
  &quot;extraClasses&quot;: &quot;fa-rotate-0&quot;,
}
            );


        var popup_15a77cb6888bdce0478ac8837f522273 = L.popup({
  &quot;maxWidth&quot;: &quot;100%&quot;,
});



                var html_c9a9189940cfc553a1a92f20758e4e66 = $(`&lt;div id=&quot;html_c9a9189940cfc553a1a92f20758e4e66&quot; style=&quot;width: 100.0%; height: 100.0%;&quot;&gt;Manna House&lt;/div&gt;`)[0];
                popup_15a77cb6888bdce0478ac8837f522273.setContent(html_c9a9189940cfc553a1a92f20758e4e66);



        marker_c2db52b96962f5f5f082982f0b6f940f.bindPopup(popup_15a77cb6888bdce0478ac8837f522273)
        ;




                marker_c2db52b96962f5f5f082982f0b6f940f.setIcon(icon_719897d215f2e3adab7f0c35d0fc59a9);


            var marker_6bc58f9094901ed85fa1bbe202eb10e0 = L.marker(
                [39.2914486, -76.6088784],
                {
}
            ).addTo(feature_group_84842d008a6f773fe6b54c40c9a5e774);


            var icon_5d79953805491d695da848065985bd8d = L.AwesomeMarkers.icon(
                {
  &quot;markerColor&quot;: &quot;green&quot;,
  &quot;iconColor&quot;: &quot;white&quot;,
  &quot;icon&quot;: &quot;heart&quot;,
  &quot;prefix&quot;: &quot;fa&quot;,
  &quot;extraClasses&quot;: &quot;fa-rotate-0&quot;,
}
            );


        var popup_9539bd5eecbdbca53a88ae0764dae12f = L.popup({
  &quot;maxWidth&quot;: &quot;100%&quot;,
});



                var html_7e4fd8280e72b81df0f3438cc1b43531 = $(`&lt;div id=&quot;html_7e4fd8280e72b81df0f3438cc1b43531&quot; style=&quot;width: 100.0%; height: 100.0%;&quot;&gt;Maryland Legal Aid&lt;/div&gt;`)[0];
                popup_9539bd5eecbdbca53a88ae0764dae12f.setContent(html_7e4fd8280e72b81df0f3438cc1b43531);



        marker_6bc58f9094901ed85fa1bbe202eb10e0.bindPopup(popup_9539bd5eecbdbca53a88ae0764dae12f)
        ;




                marker_6bc58f9094901ed85fa1bbe202eb10e0.setIcon(icon_5d79953805491d695da848065985bd8d);


            feature_group_84842d008a6f773fe6b54c40c9a5e774.addTo(map_0726b6ba9a4d2cc5792f26bf69fef3e1);


            var feature_group_e075b0a152a3f204744a99a04ff0e888 = L.featureGroup(
                {
}
            );


            var marker_46a7fd6934fe47089f7f8faa518efc31 = L.marker(
                [39.27363985230644, -76.61476039219282],
                {
}
            ).addTo(feature_group_e075b0a152a3f204744a99a04ff0e888);


            var icon_2240a8ab507e6c1df85421d9884a7fb0 = L.AwesomeMarkers.icon(
                {
  &quot;markerColor&quot;: &quot;red&quot;,
  &quot;iconColor&quot;: &quot;white&quot;,
  &quot;icon&quot;: &quot;bed&quot;,
  &quot;prefix&quot;: &quot;fa&quot;,
  &quot;extraClasses&quot;: &quot;fa-rotate-0&quot;,
}
            );


        var popup_a1abec2df4d9d88b5bc254863805b321 = L.popup({
  &quot;maxWidth&quot;: &quot;100%&quot;,
});



                var html_6f839e9779f8ceed8cca3bda195e132b = $(`&lt;div id=&quot;html_6f839e9779f8ceed8cca3bda195e132b&quot; style=&quot;width: 100.0%; height: 100.0%;&quot;&gt;American Rescue Workers&lt;/div&gt;`)[0];
                popup_a1abec2df4d9d88b5bc254863805b321.setContent(html_6f839e9779f8ceed8cca3bda195e132b);



        marker_46a7fd6934fe47089f7f8faa518efc31.bindPopup(popup_a1abec2df4d9d88b5bc254863805b321)
        ;




                marker_46a7fd6934fe47089f7f8faa518efc31.setIcon(icon_2240a8ab507e6c1df85421d9884a7fb0);


            var marker_013feea8e3f7640210fcff6a9392e63d = L.marker(
                [39.29786541144464, -76.60326105915476],
                {
}
            ).addTo(feature_group_e075b0a152a3f204744a99a04ff0e888);


            var icon_ce0fafb684116ca58f3856069c1f3c26 = L.AwesomeMarkers.icon(
                {
  &quot;markerColor&quot;: &quot;red&quot;,
  &quot;iconColor&quot;: &quot;white&quot;,
  &quot;icon&quot;: &quot;bed&quot;,
  &quot;prefix&quot;: &quot;fa&quot;,
  &quot;extraClasses&quot;: &quot;fa-rotate-0&quot;,
}
            );


        var popup_4954647b2f890fdf7022f0ce4ed24e4b = L.popup({
  &quot;maxWidth&quot;: &quot;100%&quot;,
});



                var html_b3eaf36b3f17339a102db643f85ac8f5 = $(`&lt;div id=&quot;html_b3eaf36b3f17339a102db643f85ac8f5&quot; style=&quot;width: 100.0%; height: 100.0%;&quot;&gt;Aunt CC&#x27;s Harbor House&lt;/div&gt;`)[0];
                popup_4954647b2f890fdf7022f0ce4ed24e4b.setContent(html_b3eaf36b3f17339a102db643f85ac8f5);



        marker_013feea8e3f7640210fcff6a9392e63d.bindPopup(popup_4954647b2f890fdf7022f0ce4ed24e4b)
        ;




                marker_013feea8e3f7640210fcff6a9392e63d.setIcon(icon_ce0fafb684116ca58f3856069c1f3c26);


            var marker_5d9d9e57ea5486034c7f6fbdd0134944 = L.marker(
                [39.29167942657049, -76.59996552078663],
                {
}
            ).addTo(feature_group_e075b0a152a3f204744a99a04ff0e888);


            var icon_61dbebd6790b397f0952fa4d703ec8f4 = L.AwesomeMarkers.icon(
                {
  &quot;markerColor&quot;: &quot;red&quot;,
  &quot;iconColor&quot;: &quot;white&quot;,
  &quot;icon&quot;: &quot;bed&quot;,
  &quot;prefix&quot;: &quot;fa&quot;,
  &quot;extraClasses&quot;: &quot;fa-rotate-0&quot;,
}
            );


        var popup_b741401b41b7557f2bdc7e766dd7b94f = L.popup({
  &quot;maxWidth&quot;: &quot;100%&quot;,
});



                var html_841e4d7ca6df6f493a0b283aa5f430fc = $(`&lt;div id=&quot;html_841e4d7ca6df6f493a0b283aa5f430fc&quot; style=&quot;width: 100.0%; height: 100.0%;&quot;&gt;Baltimore Rescue Mission&lt;/div&gt;`)[0];
                popup_b741401b41b7557f2bdc7e766dd7b94f.setContent(html_841e4d7ca6df6f493a0b283aa5f430fc);



        marker_5d9d9e57ea5486034c7f6fbdd0134944.bindPopup(popup_b741401b41b7557f2bdc7e766dd7b94f)
        ;




                marker_5d9d9e57ea5486034c7f6fbdd0134944.setIcon(icon_61dbebd6790b397f0952fa4d703ec8f4);


            var marker_468125698b2c91f5ece84b5860967c6f = L.marker(
                [39.28075133239391, -76.61406447836761],
                {
}
            ).addTo(feature_group_e075b0a152a3f204744a99a04ff0e888);


            var icon_35f13e43c102f1f180bc0a1741cf01d8 = L.AwesomeMarkers.icon(
                {
  &quot;markerColor&quot;: &quot;red&quot;,
  &quot;iconColor&quot;: &quot;white&quot;,
  &quot;icon&quot;: &quot;bed&quot;,
  &quot;prefix&quot;: &quot;fa&quot;,
  &quot;extraClasses&quot;: &quot;fa-rotate-0&quot;,
}
            );


        var popup_d2715416db68983807d4415d394a8d00 = L.popup({
  &quot;maxWidth&quot;: &quot;100%&quot;,
});



                var html_3de719581412bf57802b2f5665d55e3c = $(`&lt;div id=&quot;html_3de719581412bf57802b2f5665d55e3c&quot; style=&quot;width: 100.0%; height: 100.0%;&quot;&gt;Christ Lutheran Place&lt;/div&gt;`)[0];
                popup_d2715416db68983807d4415d394a8d00.setContent(html_3de719581412bf57802b2f5665d55e3c);



        marker_468125698b2c91f5ece84b5860967c6f.bindPopup(popup_d2715416db68983807d4415d394a8d00)
        ;




                marker_468125698b2c91f5ece84b5860967c6f.setIcon(icon_35f13e43c102f1f180bc0a1741cf01d8);


            var marker_9deefa6758449a651b2f3922dd4b5488 = L.marker(
                [39.29105020437977, -76.60293248355717],
                {
}
            ).addTo(feature_group_e075b0a152a3f204744a99a04ff0e888);


            var icon_aa9bee276a0061cd3180280448403842 = L.AwesomeMarkers.icon(
                {
  &quot;markerColor&quot;: &quot;red&quot;,
  &quot;iconColor&quot;: &quot;white&quot;,
  &quot;icon&quot;: &quot;bed&quot;,
  &quot;prefix&quot;: &quot;fa&quot;,
  &quot;extraClasses&quot;: &quot;fa-rotate-0&quot;,
}
            );


        var popup_74e879993db9fc7de143f384f93350f4 = L.popup({
  &quot;maxWidth&quot;: &quot;100%&quot;,
});



                var html_79a2df77067574af88da1fa3a18ab22a = $(`&lt;div id=&quot;html_79a2df77067574af88da1fa3a18ab22a&quot; style=&quot;width: 100.0%; height: 100.0%;&quot;&gt;Code Blue J, H &amp; R&lt;/div&gt;`)[0];
                popup_74e879993db9fc7de143f384f93350f4.setContent(html_79a2df77067574af88da1fa3a18ab22a);



        marker_9deefa6758449a651b2f3922dd4b5488.bindPopup(popup_74e879993db9fc7de143f384f93350f4)
        ;




                marker_9deefa6758449a651b2f3922dd4b5488.setIcon(icon_aa9bee276a0061cd3180280448403842);


            var marker_25ff97f2fc84735d250db998144f0e89 = L.marker(
                [39.304957344305144, -76.58796562511974],
                {
}
            ).addTo(feature_group_e075b0a152a3f204744a99a04ff0e888);


            var icon_ac025335c62b0982406db40c87eae943 = L.AwesomeMarkers.icon(
                {
  &quot;markerColor&quot;: &quot;red&quot;,
  &quot;iconColor&quot;: &quot;white&quot;,
  &quot;icon&quot;: &quot;bed&quot;,
  &quot;prefix&quot;: &quot;fa&quot;,
  &quot;extraClasses&quot;: &quot;fa-rotate-0&quot;,
}
            );


        var popup_ca40bfbec824563dcf3e9dea7c3d65b9 = L.popup({
  &quot;maxWidth&quot;: &quot;100%&quot;,
});



                var html_95de7bd23b62f4750e69315008b84ad6 = $(`&lt;div id=&quot;html_95de7bd23b62f4750e69315008b84ad6&quot; style=&quot;width: 100.0%; height: 100.0%;&quot;&gt;Collington Square&lt;/div&gt;`)[0];
                popup_ca40bfbec824563dcf3e9dea7c3d65b9.setContent(html_95de7bd23b62f4750e69315008b84ad6);



        marker_25ff97f2fc84735d250db998144f0e89.bindPopup(popup_ca40bfbec824563dcf3e9dea7c3d65b9)
        ;




                marker_25ff97f2fc84735d250db998144f0e89.setIcon(icon_ac025335c62b0982406db40c87eae943);


            var marker_3bf5b6a8911b59db0be265f95d60c075 = L.marker(
                [39.29046383527283, -76.6018854041655],
                {
}
            ).addTo(feature_group_e075b0a152a3f204744a99a04ff0e888);


            var icon_48a5340a2a6a72e7913aa586309ed3b8 = L.AwesomeMarkers.icon(
                {
  &quot;markerColor&quot;: &quot;red&quot;,
  &quot;iconColor&quot;: &quot;white&quot;,
  &quot;icon&quot;: &quot;bed&quot;,
  &quot;prefix&quot;: &quot;fa&quot;,
  &quot;extraClasses&quot;: &quot;fa-rotate-0&quot;,
}
            );


        var popup_12b8c6375cad1ba676d3dbb0843b3e71 = L.popup({
  &quot;maxWidth&quot;: &quot;100%&quot;,
});



                var html_cd5abb291f659abb32d23d605e2a60e1 = $(`&lt;div id=&quot;html_cd5abb291f659abb32d23d605e2a60e1&quot; style=&quot;width: 100.0%; height: 100.0%;&quot;&gt;Helping Up&lt;/div&gt;`)[0];
                popup_12b8c6375cad1ba676d3dbb0843b3e71.setContent(html_cd5abb291f659abb32d23d605e2a60e1);



        marker_3bf5b6a8911b59db0be265f95d60c075.bindPopup(popup_12b8c6375cad1ba676d3dbb0843b3e71)
        ;




                marker_3bf5b6a8911b59db0be265f95d60c075.setIcon(icon_48a5340a2a6a72e7913aa586309ed3b8);


            var marker_e0ed71dcabdf91327069b764e0a16982 = L.marker(
                [39.30174696401811, -76.64517677848862],
                {
}
            ).addTo(feature_group_e075b0a152a3f204744a99a04ff0e888);


            var icon_2ad31ba0a49c11ff696403bc1f0e514d = L.AwesomeMarkers.icon(
                {
  &quot;markerColor&quot;: &quot;red&quot;,
  &quot;iconColor&quot;: &quot;white&quot;,
  &quot;icon&quot;: &quot;bed&quot;,
  &quot;prefix&quot;: &quot;fa&quot;,
  &quot;extraClasses&quot;: &quot;fa-rotate-0&quot;,
}
            );


        var popup_a27e9b7081130668baa47a5acc6457b8 = L.popup({
  &quot;maxWidth&quot;: &quot;100%&quot;,
});



                var html_ff023e993633a36a9b339acec946ba6f = $(`&lt;div id=&quot;html_ff023e993633a36a9b339acec946ba6f&quot; style=&quot;width: 100.0%; height: 100.0%;&quot;&gt;Interfaith ES&lt;/div&gt;`)[0];
                popup_a27e9b7081130668baa47a5acc6457b8.setContent(html_ff023e993633a36a9b339acec946ba6f);



        marker_e0ed71dcabdf91327069b764e0a16982.bindPopup(popup_a27e9b7081130668baa47a5acc6457b8)
        ;




                marker_e0ed71dcabdf91327069b764e0a16982.setIcon(icon_2ad31ba0a49c11ff696403bc1f0e514d);


            var marker_35f9347c6644d0c4393aa39522662ec8 = L.marker(
                [39.291612593818414, -76.60022671646288],
                {
}
            ).addTo(feature_group_e075b0a152a3f204744a99a04ff0e888);


            var icon_e8dcd569e1be9e3772ea79d4c0d85536 = L.AwesomeMarkers.icon(
                {
  &quot;markerColor&quot;: &quot;red&quot;,
  &quot;iconColor&quot;: &quot;white&quot;,
  &quot;icon&quot;: &quot;bed&quot;,
  &quot;prefix&quot;: &quot;fa&quot;,
  &quot;extraClasses&quot;: &quot;fa-rotate-0&quot;,
}
            );


        var popup_35280f9260e33502d82d7ab6a65b727b = L.popup({
  &quot;maxWidth&quot;: &quot;100%&quot;,
});



                var html_39aee4535dfd9c10e4884dbfeb882724 = $(`&lt;div id=&quot;html_39aee4535dfd9c10e4884dbfeb882724&quot; style=&quot;width: 100.0%; height: 100.0%;&quot;&gt;Karis House&lt;/div&gt;`)[0];
                popup_35280f9260e33502d82d7ab6a65b727b.setContent(html_39aee4535dfd9c10e4884dbfeb882724);



        marker_35f9347c6644d0c4393aa39522662ec8.bindPopup(popup_35280f9260e33502d82d7ab6a65b727b)
        ;




                marker_35f9347c6644d0c4393aa39522662ec8.setIcon(icon_e8dcd569e1be9e3772ea79d4c0d85536);


            var marker_2a0e5fa213f5f0d036bc82d347f5666b = L.marker(
                [39.29370441331408, -76.606262560335],
                {
}
            ).addTo(feature_group_e075b0a152a3f204744a99a04ff0e888);


            var icon_72eb3336b62f9cb047f50f7e103b9bad = L.AwesomeMarkers.icon(
                {
  &quot;markerColor&quot;: &quot;red&quot;,
  &quot;iconColor&quot;: &quot;white&quot;,
  &quot;icon&quot;: &quot;bed&quot;,
  &quot;prefix&quot;: &quot;fa&quot;,
  &quot;extraClasses&quot;: &quot;fa-rotate-0&quot;,
}
            );


        var popup_ced8a0d67f565c291dec3115167ae513 = L.popup({
  &quot;maxWidth&quot;: &quot;100%&quot;,
});



                var html_2253b037eecfef06c0a2c8f156cc2e76 = $(`&lt;div id=&quot;html_2253b037eecfef06c0a2c8f156cc2e76&quot; style=&quot;width: 100.0%; height: 100.0%;&quot;&gt;MCVET ES&lt;/div&gt;`)[0];
                popup_ced8a0d67f565c291dec3115167ae513.setContent(html_2253b037eecfef06c0a2c8f156cc2e76);



        marker_2a0e5fa213f5f0d036bc82d347f5666b.bindPopup(popup_ced8a0d67f565c291dec3115167ae513)
        ;




                marker_2a0e5fa213f5f0d036bc82d347f5666b.setIcon(icon_72eb3336b62f9cb047f50f7e103b9bad);


            var marker_8429012b503e09d1e354493c2a9d69f2 = L.marker(
                [39.31830481986578, -76.613995968731],
                {
}
            ).addTo(feature_group_e075b0a152a3f204744a99a04ff0e888);


            var icon_6bcf299fa7210656260a62481d9ba676 = L.AwesomeMarkers.icon(
                {
  &quot;markerColor&quot;: &quot;red&quot;,
  &quot;iconColor&quot;: &quot;white&quot;,
  &quot;icon&quot;: &quot;bed&quot;,
  &quot;prefix&quot;: &quot;fa&quot;,
  &quot;extraClasses&quot;: &quot;fa-rotate-0&quot;,
}
            );


        var popup_97ffd296de32f9dbe7cde089a7c54c2d = L.popup({
  &quot;maxWidth&quot;: &quot;100%&quot;,
});



                var html_bfd713d606196a3a61d3389f088d0b49 = $(`&lt;div id=&quot;html_bfd713d606196a3a61d3389f088d0b49&quot; style=&quot;width: 100.0%; height: 100.0%;&quot;&gt;Prisoners AID ES&lt;/div&gt;`)[0];
                popup_97ffd296de32f9dbe7cde089a7c54c2d.setContent(html_bfd713d606196a3a61d3389f088d0b49);



        marker_8429012b503e09d1e354493c2a9d69f2.bindPopup(popup_97ffd296de32f9dbe7cde089a7c54c2d)
        ;




                marker_8429012b503e09d1e354493c2a9d69f2.setIcon(icon_6bcf299fa7210656260a62481d9ba676);


            var marker_9c9ea96ca729aeac59f9471e8ed4efc0 = L.marker(
                [39.31084925041601, -76.61405196324532],
                {
}
            ).addTo(feature_group_e075b0a152a3f204744a99a04ff0e888);


            var icon_9d554022608efae699fae6bb45d87f5b = L.AwesomeMarkers.icon(
                {
  &quot;markerColor&quot;: &quot;red&quot;,
  &quot;iconColor&quot;: &quot;white&quot;,
  &quot;icon&quot;: &quot;bed&quot;,
  &quot;prefix&quot;: &quot;fa&quot;,
  &quot;extraClasses&quot;: &quot;fa-rotate-0&quot;,
}
            );


        var popup_f36c9a5d990795a148d6e98fdfca562a = L.popup({
  &quot;maxWidth&quot;: &quot;100%&quot;,
});



                var html_3add20c88e81af5cf47201428ad19e4a = $(`&lt;div id=&quot;html_3add20c88e81af5cf47201428ad19e4a&quot; style=&quot;width: 100.0%; height: 100.0%;&quot;&gt;Project PLASE ES&lt;/div&gt;`)[0];
                popup_f36c9a5d990795a148d6e98fdfca562a.setContent(html_3add20c88e81af5cf47201428ad19e4a);



        marker_9c9ea96ca729aeac59f9471e8ed4efc0.bindPopup(popup_f36c9a5d990795a148d6e98fdfca562a)
        ;




                marker_9c9ea96ca729aeac59f9471e8ed4efc0.setIcon(icon_9d554022608efae699fae6bb45d87f5b);


            var marker_01db665b233ae8cf77dc6017a1047d98 = L.marker(
                [39.30298833545443, -76.61357192318277],
                {
}
            ).addTo(feature_group_e075b0a152a3f204744a99a04ff0e888);


            var icon_4e16cfaaaf8831d35a1cdb329f5d4c12 = L.AwesomeMarkers.icon(
                {
  &quot;markerColor&quot;: &quot;red&quot;,
  &quot;iconColor&quot;: &quot;white&quot;,
  &quot;icon&quot;: &quot;bed&quot;,
  &quot;prefix&quot;: &quot;fa&quot;,
  &quot;extraClasses&quot;: &quot;fa-rotate-0&quot;,
}
            );


        var popup_8f8f7134042986613cb92eb9a1c87554 = L.popup({
  &quot;maxWidth&quot;: &quot;100%&quot;,
});



                var html_971121423d6579e0b253850acff69187 = $(`&lt;div id=&quot;html_971121423d6579e0b253850acff69187&quot; style=&quot;width: 100.0%; height: 100.0%;&quot;&gt;Salvation Army/Booth House&lt;/div&gt;`)[0];
                popup_8f8f7134042986613cb92eb9a1c87554.setContent(html_971121423d6579e0b253850acff69187);



        marker_01db665b233ae8cf77dc6017a1047d98.bindPopup(popup_8f8f7134042986613cb92eb9a1c87554)
        ;




                marker_01db665b233ae8cf77dc6017a1047d98.setIcon(icon_4e16cfaaaf8831d35a1cdb329f5d4c12);


            var marker_5d83711d6d12310c2aec092a3a1f1d45 = L.marker(
                [39.29828493326264, -76.6101713910694],
                {
}
            ).addTo(feature_group_e075b0a152a3f204744a99a04ff0e888);


            var icon_cd941a5b7039d597984770f9e871fa6b = L.AwesomeMarkers.icon(
                {
  &quot;markerColor&quot;: &quot;red&quot;,
  &quot;iconColor&quot;: &quot;white&quot;,
  &quot;icon&quot;: &quot;bed&quot;,
  &quot;prefix&quot;: &quot;fa&quot;,
  &quot;extraClasses&quot;: &quot;fa-rotate-0&quot;,
}
            );


        var popup_cd718c7ff3e279082134f9d286a4c0c9 = L.popup({
  &quot;maxWidth&quot;: &quot;100%&quot;,
});



                var html_a9baec7475e549716b86bbf0739fc528 = $(`&lt;div id=&quot;html_a9baec7475e549716b86bbf0739fc528&quot; style=&quot;width: 100.0%; height: 100.0%;&quot;&gt;ACC Christopher Place&lt;/div&gt;`)[0];
                popup_cd718c7ff3e279082134f9d286a4c0c9.setContent(html_a9baec7475e549716b86bbf0739fc528);



        marker_5d83711d6d12310c2aec092a3a1f1d45.bindPopup(popup_cd718c7ff3e279082134f9d286a4c0c9)
        ;




                marker_5d83711d6d12310c2aec092a3a1f1d45.setIcon(icon_cd941a5b7039d597984770f9e871fa6b);


            var marker_1480a616ea2987323ff2af3856e7380f = L.marker(
                [39.293777663886225, -76.6174741908216],
                {
}
            ).addTo(feature_group_e075b0a152a3f204744a99a04ff0e888);


            var icon_ec428b0e26d126b68691f7d490cc2f87 = L.AwesomeMarkers.icon(
                {
  &quot;markerColor&quot;: &quot;red&quot;,
  &quot;iconColor&quot;: &quot;white&quot;,
  &quot;icon&quot;: &quot;bed&quot;,
  &quot;prefix&quot;: &quot;fa&quot;,
  &quot;extraClasses&quot;: &quot;fa-rotate-0&quot;,
}
            );


        var popup_f3969186052af350871c51477c96d732 = L.popup({
  &quot;maxWidth&quot;: &quot;100%&quot;,
});



                var html_487b50d0f4275af78caed75318b2fe0c = $(`&lt;div id=&quot;html_487b50d0f4275af78caed75318b2fe0c&quot; style=&quot;width: 100.0%; height: 100.0%;&quot;&gt;ACC My Sister’s Place Lodge&lt;/div&gt;`)[0];
                popup_f3969186052af350871c51477c96d732.setContent(html_487b50d0f4275af78caed75318b2fe0c);



        marker_1480a616ea2987323ff2af3856e7380f.bindPopup(popup_f3969186052af350871c51477c96d732)
        ;




                marker_1480a616ea2987323ff2af3856e7380f.setIcon(icon_ec428b0e26d126b68691f7d490cc2f87);


            var marker_e31bb071e47c28e17e37f9f4f876e025 = L.marker(
                [39.31564499777156, -76.61650251143895],
                {
}
            ).addTo(feature_group_e075b0a152a3f204744a99a04ff0e888);


            var icon_be5eeb5e1bcca8718ffaf8c26e05b4bf = L.AwesomeMarkers.icon(
                {
  &quot;markerColor&quot;: &quot;red&quot;,
  &quot;iconColor&quot;: &quot;white&quot;,
  &quot;icon&quot;: &quot;bed&quot;,
  &quot;prefix&quot;: &quot;fa&quot;,
  &quot;extraClasses&quot;: &quot;fa-rotate-0&quot;,
}
            );


        var popup_8d0af23c06801d11a70b16b3fbcb62a5 = L.popup({
  &quot;maxWidth&quot;: &quot;100%&quot;,
});



                var html_733c6d360343d96f9dc5067dbb692ce9 = $(`&lt;div id=&quot;html_733c6d360343d96f9dc5067dbb692ce9&quot; style=&quot;width: 100.0%; height: 100.0%;&quot;&gt;ACC Project FRESH Start&lt;/div&gt;`)[0];
                popup_8d0af23c06801d11a70b16b3fbcb62a5.setContent(html_733c6d360343d96f9dc5067dbb692ce9);



        marker_e31bb071e47c28e17e37f9f4f876e025.bindPopup(popup_8d0af23c06801d11a70b16b3fbcb62a5)
        ;




                marker_e31bb071e47c28e17e37f9f4f876e025.setIcon(icon_be5eeb5e1bcca8718ffaf8c26e05b4bf);


            var marker_7f1f5f88f10dc5dd029ec8f940d51fbc = L.marker(
                [39.31769450620435, -76.61196703102117],
                {
}
            ).addTo(feature_group_e075b0a152a3f204744a99a04ff0e888);


            var icon_b7457967856638b318fe34f2c6fb6faf = L.AwesomeMarkers.icon(
                {
  &quot;markerColor&quot;: &quot;red&quot;,
  &quot;iconColor&quot;: &quot;white&quot;,
  &quot;icon&quot;: &quot;bed&quot;,
  &quot;prefix&quot;: &quot;fa&quot;,
  &quot;extraClasses&quot;: &quot;fa-rotate-0&quot;,
}
            );


        var popup_1e1a98ee84fc6d38e671050e35d2cd4e = L.popup({
  &quot;maxWidth&quot;: &quot;100%&quot;,
});



                var html_80586454953b7ea2c7fbf50a1c22d05b = $(`&lt;div id=&quot;html_80586454953b7ea2c7fbf50a1c22d05b&quot; style=&quot;width: 100.0%; height: 100.0%;&quot;&gt;At Jacob’s Well&lt;/div&gt;`)[0];
                popup_1e1a98ee84fc6d38e671050e35d2cd4e.setContent(html_80586454953b7ea2c7fbf50a1c22d05b);



        marker_7f1f5f88f10dc5dd029ec8f940d51fbc.bindPopup(popup_1e1a98ee84fc6d38e671050e35d2cd4e)
        ;




                marker_7f1f5f88f10dc5dd029ec8f940d51fbc.setIcon(icon_b7457967856638b318fe34f2c6fb6faf);


            var marker_b8a88e0dc009ee87c30a15ac8d0e198e = L.marker(
                [39.340193600655745, -76.6072150570671],
                {
}
            ).addTo(feature_group_e075b0a152a3f204744a99a04ff0e888);


            var icon_b520dda87154464c6f05fc7c9010c920 = L.AwesomeMarkers.icon(
                {
  &quot;markerColor&quot;: &quot;red&quot;,
  &quot;iconColor&quot;: &quot;white&quot;,
  &quot;icon&quot;: &quot;bed&quot;,
  &quot;prefix&quot;: &quot;fa&quot;,
  &quot;extraClasses&quot;: &quot;fa-rotate-0&quot;,
}
            );


        var popup_8f803e9f0531d753852bb94897f051d6 = L.popup({
  &quot;maxWidth&quot;: &quot;100%&quot;,
});



                var html_bdc559f0b206b59f65155771a8dd2fbc = $(`&lt;div id=&quot;html_bdc559f0b206b59f65155771a8dd2fbc&quot; style=&quot;width: 100.0%; height: 100.0%;&quot;&gt;BMHS Safe Haven I&lt;/div&gt;`)[0];
                popup_8f803e9f0531d753852bb94897f051d6.setContent(html_bdc559f0b206b59f65155771a8dd2fbc);



        marker_b8a88e0dc009ee87c30a15ac8d0e198e.bindPopup(popup_8f803e9f0531d753852bb94897f051d6)
        ;




                marker_b8a88e0dc009ee87c30a15ac8d0e198e.setIcon(icon_b520dda87154464c6f05fc7c9010c920);


            var marker_c3f0f11cc5abb5bdf4e551bb374b49a4 = L.marker(
                [39.31798560159025, -76.57909920708497],
                {
}
            ).addTo(feature_group_e075b0a152a3f204744a99a04ff0e888);


            var icon_5fdd99c6480980ce99b0420557915d4c = L.AwesomeMarkers.icon(
                {
  &quot;markerColor&quot;: &quot;red&quot;,
  &quot;iconColor&quot;: &quot;white&quot;,
  &quot;icon&quot;: &quot;bed&quot;,
  &quot;prefix&quot;: &quot;fa&quot;,
  &quot;extraClasses&quot;: &quot;fa-rotate-0&quot;,
}
            );


        var popup_843c4033ba2c9423912ebba3c63676cc = L.popup({
  &quot;maxWidth&quot;: &quot;100%&quot;,
});



                var html_8442f6418bd093034c5110fd9fa8ab83 = $(`&lt;div id=&quot;html_8442f6418bd093034c5110fd9fa8ab83&quot; style=&quot;width: 100.0%; height: 100.0%;&quot;&gt;BMHS Ethel Elan Safe Haven II&lt;/div&gt;`)[0];
                popup_843c4033ba2c9423912ebba3c63676cc.setContent(html_8442f6418bd093034c5110fd9fa8ab83);



        marker_c3f0f11cc5abb5bdf4e551bb374b49a4.bindPopup(popup_843c4033ba2c9423912ebba3c63676cc)
        ;




                marker_c3f0f11cc5abb5bdf4e551bb374b49a4.setIcon(icon_5fdd99c6480980ce99b0420557915d4c);


            var marker_5a588151b646890c4013810d7326b644 = L.marker(
                [39.30497316782566, -76.58749425008823],
                {
}
            ).addTo(feature_group_e075b0a152a3f204744a99a04ff0e888);


            var icon_92e4671ea52a5812e5eba5b272622fd7 = L.AwesomeMarkers.icon(
                {
  &quot;markerColor&quot;: &quot;red&quot;,
  &quot;iconColor&quot;: &quot;white&quot;,
  &quot;icon&quot;: &quot;bed&quot;,
  &quot;prefix&quot;: &quot;fa&quot;,
  &quot;extraClasses&quot;: &quot;fa-rotate-0&quot;,
}
            );


        var popup_9dda01b6da28a9850e0226c255b6fa36 = L.popup({
  &quot;maxWidth&quot;: &quot;100%&quot;,
});



                var html_8c77794dd2937301c9b60423fddbb7bd = $(`&lt;div id=&quot;html_8c77794dd2937301c9b60423fddbb7bd&quot; style=&quot;width: 100.0%; height: 100.0%;&quot;&gt;Dayspring Village&lt;/div&gt;`)[0];
                popup_9dda01b6da28a9850e0226c255b6fa36.setContent(html_8c77794dd2937301c9b60423fddbb7bd);



        marker_5a588151b646890c4013810d7326b644.bindPopup(popup_9dda01b6da28a9850e0226c255b6fa36)
        ;




                marker_5a588151b646890c4013810d7326b644.setIcon(icon_92e4671ea52a5812e5eba5b272622fd7);


            var marker_b1d1937c922febb7db988e783c27114e = L.marker(
                [39.29046383527283, -76.6018854041655],
                {
}
            ).addTo(feature_group_e075b0a152a3f204744a99a04ff0e888);


            var icon_ad1a45ef9d860c1f26857e42489b4206 = L.AwesomeMarkers.icon(
                {
  &quot;markerColor&quot;: &quot;red&quot;,
  &quot;iconColor&quot;: &quot;white&quot;,
  &quot;icon&quot;: &quot;bed&quot;,
  &quot;prefix&quot;: &quot;fa&quot;,
  &quot;extraClasses&quot;: &quot;fa-rotate-0&quot;,
}
            );


        var popup_d26c6cfa1cd99b83e57b3e1036fe5d60 = L.popup({
  &quot;maxWidth&quot;: &quot;100%&quot;,
});



                var html_fa9649f95a211ef842e6c347fc45b382 = $(`&lt;div id=&quot;html_fa9649f95a211ef842e6c347fc45b382&quot; style=&quot;width: 100.0%; height: 100.0%;&quot;&gt;Helping Up Mission&lt;/div&gt;`)[0];
                popup_d26c6cfa1cd99b83e57b3e1036fe5d60.setContent(html_fa9649f95a211ef842e6c347fc45b382);



        marker_b1d1937c922febb7db988e783c27114e.bindPopup(popup_d26c6cfa1cd99b83e57b3e1036fe5d60)
        ;




                marker_b1d1937c922febb7db988e783c27114e.setIcon(icon_ad1a45ef9d860c1f26857e42489b4206);


            var marker_13f8b0c4ca9e44378f4203a24b5670a5 = L.marker(
                [39.31121960256198, -76.67631966621622],
                {
}
            ).addTo(feature_group_e075b0a152a3f204744a99a04ff0e888);


            var icon_05e41c44dc63f13ecabdfd54894e660a = L.AwesomeMarkers.icon(
                {
  &quot;markerColor&quot;: &quot;red&quot;,
  &quot;iconColor&quot;: &quot;white&quot;,
  &quot;icon&quot;: &quot;bed&quot;,
  &quot;prefix&quot;: &quot;fa&quot;,
  &quot;extraClasses&quot;: &quot;fa-rotate-0&quot;,
}
            );


        var popup_63ddb2bf90bad499404105eef7948c18 = L.popup({
  &quot;maxWidth&quot;: &quot;100%&quot;,
});



                var html_9cf5623e2b8c4f4911946e8fea7f1d34 = $(`&lt;div id=&quot;html_9cf5623e2b8c4f4911946e8fea7f1d34&quot; style=&quot;width: 100.0%; height: 100.0%;&quot;&gt;J, H &amp; R - Carrington House&lt;/div&gt;`)[0];
                popup_63ddb2bf90bad499404105eef7948c18.setContent(html_9cf5623e2b8c4f4911946e8fea7f1d34);



        marker_13f8b0c4ca9e44378f4203a24b5670a5.bindPopup(popup_63ddb2bf90bad499404105eef7948c18)
        ;




                marker_13f8b0c4ca9e44378f4203a24b5670a5.setIcon(icon_05e41c44dc63f13ecabdfd54894e660a);


            var marker_f82a4187f033ac6f1c530845063fe62c = L.marker(
                [39.31121960256198, -76.67631966621622],
                {
}
            ).addTo(feature_group_e075b0a152a3f204744a99a04ff0e888);


            var icon_89cc2528a8919ccd2387d57f5bb265e6 = L.AwesomeMarkers.icon(
                {
  &quot;markerColor&quot;: &quot;red&quot;,
  &quot;iconColor&quot;: &quot;white&quot;,
  &quot;icon&quot;: &quot;bed&quot;,
  &quot;prefix&quot;: &quot;fa&quot;,
  &quot;extraClasses&quot;: &quot;fa-rotate-0&quot;,
}
            );


        var popup_588b7bb880719ce19f3af86a617ca34c = L.popup({
  &quot;maxWidth&quot;: &quot;100%&quot;,
});



                var html_a51903214c613175661cd7ac06c6710c = $(`&lt;div id=&quot;html_a51903214c613175661cd7ac06c6710c&quot; style=&quot;width: 100.0%; height: 100.0%;&quot;&gt;J, H &amp; R - VA&lt;/div&gt;`)[0];
                popup_588b7bb880719ce19f3af86a617ca34c.setContent(html_a51903214c613175661cd7ac06c6710c);



        marker_f82a4187f033ac6f1c530845063fe62c.bindPopup(popup_588b7bb880719ce19f3af86a617ca34c)
        ;




                marker_f82a4187f033ac6f1c530845063fe62c.setIcon(icon_89cc2528a8919ccd2387d57f5bb265e6);


            var marker_81026c30398112f2e9f3e66576396857 = L.marker(
                [39.32560915784916, -76.60299377730044],
                {
}
            ).addTo(feature_group_e075b0a152a3f204744a99a04ff0e888);


            var icon_5484ff2eaeab02c9e812f07720549e6c = L.AwesomeMarkers.icon(
                {
  &quot;markerColor&quot;: &quot;red&quot;,
  &quot;iconColor&quot;: &quot;white&quot;,
  &quot;icon&quot;: &quot;bed&quot;,
  &quot;prefix&quot;: &quot;fa&quot;,
  &quot;extraClasses&quot;: &quot;fa-rotate-0&quot;,
}
            );


        var popup_bfe537cd7c7c49607ff72c942a774242 = L.popup({
  &quot;maxWidth&quot;: &quot;100%&quot;,
});



                var html_51a132465efcdc8786b646c2c88f5ad2 = $(`&lt;div id=&quot;html_51a132465efcdc8786b646c2c88f5ad2&quot; style=&quot;width: 100.0%; height: 100.0%;&quot;&gt;Marian House&lt;/div&gt;`)[0];
                popup_bfe537cd7c7c49607ff72c942a774242.setContent(html_51a132465efcdc8786b646c2c88f5ad2);



        marker_81026c30398112f2e9f3e66576396857.bindPopup(popup_bfe537cd7c7c49607ff72c942a774242)
        ;




                marker_81026c30398112f2e9f3e66576396857.setIcon(icon_5484ff2eaeab02c9e812f07720549e6c);


            var marker_33f357ee96afabf972169debaeb3b9af = L.marker(
                [39.29370441331408, -76.606262560335],
                {
}
            ).addTo(feature_group_e075b0a152a3f204744a99a04ff0e888);


            var icon_f6f06edb43ba0f58f093aa4186a9f1ac = L.AwesomeMarkers.icon(
                {
  &quot;markerColor&quot;: &quot;red&quot;,
  &quot;iconColor&quot;: &quot;white&quot;,
  &quot;icon&quot;: &quot;bed&quot;,
  &quot;prefix&quot;: &quot;fa&quot;,
  &quot;extraClasses&quot;: &quot;fa-rotate-0&quot;,
}
            );


        var popup_c5c4e0a61738a58b31f25e68f18dfe67 = L.popup({
  &quot;maxWidth&quot;: &quot;100%&quot;,
});



                var html_067ed1be33c0940008aadf2a24119869 = $(`&lt;div id=&quot;html_067ed1be33c0940008aadf2a24119869&quot; style=&quot;width: 100.0%; height: 100.0%;&quot;&gt;Maryland Center for Veterans Education and Training&lt;/div&gt;`)[0];
                popup_c5c4e0a61738a58b31f25e68f18dfe67.setContent(html_067ed1be33c0940008aadf2a24119869);



        marker_33f357ee96afabf972169debaeb3b9af.bindPopup(popup_c5c4e0a61738a58b31f25e68f18dfe67)
        ;




                marker_33f357ee96afabf972169debaeb3b9af.setIcon(icon_f6f06edb43ba0f58f093aa4186a9f1ac);


            var marker_4a597b9f8d631daa9e445afd8c66409d = L.marker(
                [39.29872855966124, -76.61898072865594],
                {
}
            ).addTo(feature_group_e075b0a152a3f204744a99a04ff0e888);


            var icon_4322542e807c6aecd73049c1aa6880c9 = L.AwesomeMarkers.icon(
                {
  &quot;markerColor&quot;: &quot;red&quot;,
  &quot;iconColor&quot;: &quot;white&quot;,
  &quot;icon&quot;: &quot;bed&quot;,
  &quot;prefix&quot;: &quot;fa&quot;,
  &quot;extraClasses&quot;: &quot;fa-rotate-0&quot;,
}
            );


        var popup_c97fc5782878f8b12dc306e44b1111f0 = L.popup({
  &quot;maxWidth&quot;: &quot;100%&quot;,
});



                var html_ce499986a5ad41c6395b3f13baf552ba = $(`&lt;div id=&quot;html_ce499986a5ad41c6395b3f13baf552ba&quot; style=&quot;width: 100.0%; height: 100.0%;&quot;&gt;Patrick Allison House&lt;/div&gt;`)[0];
                popup_c97fc5782878f8b12dc306e44b1111f0.setContent(html_ce499986a5ad41c6395b3f13baf552ba);



        marker_4a597b9f8d631daa9e445afd8c66409d.bindPopup(popup_c97fc5782878f8b12dc306e44b1111f0)
        ;




                marker_4a597b9f8d631daa9e445afd8c66409d.setIcon(icon_4322542e807c6aecd73049c1aa6880c9);


            var marker_3351fcac84fc35807b4b3d5bc377fc04 = L.marker(
                [39.31830481986578, -76.613995968731],
                {
}
            ).addTo(feature_group_e075b0a152a3f204744a99a04ff0e888);


            var icon_34077a4ddd1a044310bd6a488254edb1 = L.AwesomeMarkers.icon(
                {
  &quot;markerColor&quot;: &quot;red&quot;,
  &quot;iconColor&quot;: &quot;white&quot;,
  &quot;icon&quot;: &quot;bed&quot;,
  &quot;prefix&quot;: &quot;fa&quot;,
  &quot;extraClasses&quot;: &quot;fa-rotate-0&quot;,
}
            );


        var popup_1b6beea789085e00080ff2c734b06fde = L.popup({
  &quot;maxWidth&quot;: &quot;100%&quot;,
});



                var html_0c51f51cdaaae2f441a6d8ee22a6bcf4 = $(`&lt;div id=&quot;html_0c51f51cdaaae2f441a6d8ee22a6bcf4&quot; style=&quot;width: 100.0%; height: 100.0%;&quot;&gt;Prisoner’s Aid&lt;/div&gt;`)[0];
                popup_1b6beea789085e00080ff2c734b06fde.setContent(html_0c51f51cdaaae2f441a6d8ee22a6bcf4);



        marker_3351fcac84fc35807b4b3d5bc377fc04.bindPopup(popup_1b6beea789085e00080ff2c734b06fde)
        ;




                marker_3351fcac84fc35807b4b3d5bc377fc04.setIcon(icon_34077a4ddd1a044310bd6a488254edb1);


            var marker_e3b45d2ad6b46b3c7099fb0d97ac0395 = L.marker(
                [39.31017049782587, -76.61822160486902],
                {
}
            ).addTo(feature_group_e075b0a152a3f204744a99a04ff0e888);


            var icon_e23a5a18f1f6239d55ef099e4aeb0548 = L.AwesomeMarkers.icon(
                {
  &quot;markerColor&quot;: &quot;red&quot;,
  &quot;iconColor&quot;: &quot;white&quot;,
  &quot;icon&quot;: &quot;bed&quot;,
  &quot;prefix&quot;: &quot;fa&quot;,
  &quot;extraClasses&quot;: &quot;fa-rotate-0&quot;,
}
            );


        var popup_522aa0feb30aac92b4b7fccedd78c3d3 = L.popup({
  &quot;maxWidth&quot;: &quot;100%&quot;,
});



                var html_ca594502686046a7370d97b9f36eae44 = $(`&lt;div id=&quot;html_ca594502686046a7370d97b9f36eae44&quot; style=&quot;width: 100.0%; height: 100.0%;&quot;&gt;Project PLASE&lt;/div&gt;`)[0];
                popup_522aa0feb30aac92b4b7fccedd78c3d3.setContent(html_ca594502686046a7370d97b9f36eae44);



        marker_e3b45d2ad6b46b3c7099fb0d97ac0395.bindPopup(popup_522aa0feb30aac92b4b7fccedd78c3d3)
        ;




                marker_e3b45d2ad6b46b3c7099fb0d97ac0395.setIcon(icon_e23a5a18f1f6239d55ef099e4aeb0548);


            var marker_823d5d056ca4fb14e3cbc2f3407fccf3 = L.marker(
                [39.30298833545443, -76.61357192318277],
                {
}
            ).addTo(feature_group_e075b0a152a3f204744a99a04ff0e888);


            var icon_80a36d3d673f443cb94872f41db9d0f8 = L.AwesomeMarkers.icon(
                {
  &quot;markerColor&quot;: &quot;red&quot;,
  &quot;iconColor&quot;: &quot;white&quot;,
  &quot;icon&quot;: &quot;bed&quot;,
  &quot;prefix&quot;: &quot;fa&quot;,
  &quot;extraClasses&quot;: &quot;fa-rotate-0&quot;,
}
            );


        var popup_0314a25ff27aa73875a97c3ea0e6a5de = L.popup({
  &quot;maxWidth&quot;: &quot;100%&quot;,
});



                var html_233e4d8be46331b778c36815d7a2576e = $(`&lt;div id=&quot;html_233e4d8be46331b778c36815d7a2576e&quot; style=&quot;width: 100.0%; height: 100.0%;&quot;&gt;Salvation Army&lt;/div&gt;`)[0];
                popup_0314a25ff27aa73875a97c3ea0e6a5de.setContent(html_233e4d8be46331b778c36815d7a2576e);



        marker_823d5d056ca4fb14e3cbc2f3407fccf3.bindPopup(popup_0314a25ff27aa73875a97c3ea0e6a5de)
        ;




                marker_823d5d056ca4fb14e3cbc2f3407fccf3.setIcon(icon_80a36d3d673f443cb94872f41db9d0f8);


            var marker_07097bf502a171b9f65d5163a0302313 = L.marker(
                [39.2969189028437, -76.62156707390085],
                {
}
            ).addTo(feature_group_e075b0a152a3f204744a99a04ff0e888);


            var icon_5b426586fa4a0ad676db5544b149175c = L.AwesomeMarkers.icon(
                {
  &quot;markerColor&quot;: &quot;red&quot;,
  &quot;iconColor&quot;: &quot;white&quot;,
  &quot;icon&quot;: &quot;bed&quot;,
  &quot;prefix&quot;: &quot;fa&quot;,
  &quot;extraClasses&quot;: &quot;fa-rotate-0&quot;,
}
            );


        var popup_68fb7c5297ce9ad51458ff5ae58092b3 = L.popup({
  &quot;maxWidth&quot;: &quot;100%&quot;,
});



                var html_ff6acb797f168c236d7e3a2d727f2236 = $(`&lt;div id=&quot;html_ff6acb797f168c236d7e3a2d727f2236&quot; style=&quot;width: 100.0%; height: 100.0%;&quot;&gt;Seton Station&lt;/div&gt;`)[0];
                popup_68fb7c5297ce9ad51458ff5ae58092b3.setContent(html_ff6acb797f168c236d7e3a2d727f2236);



        marker_07097bf502a171b9f65d5163a0302313.bindPopup(popup_68fb7c5297ce9ad51458ff5ae58092b3)
        ;




                marker_07097bf502a171b9f65d5163a0302313.setIcon(icon_5b426586fa4a0ad676db5544b149175c);


            var marker_926bc456bc42f79dcd132e9a7a87ac1d = L.marker(
                [39.30912895418698, -76.59344372933701],
                {
}
            ).addTo(feature_group_e075b0a152a3f204744a99a04ff0e888);


            var icon_79ddd1ac8ed48de7c21971b556afb445 = L.AwesomeMarkers.icon(
                {
  &quot;markerColor&quot;: &quot;red&quot;,
  &quot;iconColor&quot;: &quot;white&quot;,
  &quot;icon&quot;: &quot;bed&quot;,
  &quot;prefix&quot;: &quot;fa&quot;,
  &quot;extraClasses&quot;: &quot;fa-rotate-0&quot;,
}
            );


        var popup_972d9227ef2a24999310dff92ddb0710 = L.popup({
  &quot;maxWidth&quot;: &quot;100%&quot;,
});



                var html_86736cc611a51244da631baf0a22e26d = $(`&lt;div id=&quot;html_86736cc611a51244da631baf0a22e26d&quot; style=&quot;width: 100.0%; height: 100.0%;&quot;&gt;Supportive Housing Group/Lanvale Institute&lt;/div&gt;`)[0];
                popup_972d9227ef2a24999310dff92ddb0710.setContent(html_86736cc611a51244da631baf0a22e26d);



        marker_926bc456bc42f79dcd132e9a7a87ac1d.bindPopup(popup_972d9227ef2a24999310dff92ddb0710)
        ;




                marker_926bc456bc42f79dcd132e9a7a87ac1d.setIcon(icon_79ddd1ac8ed48de7c21971b556afb445);


            var marker_85b53e5dedae1f7f70494e5e8466e054 = L.marker(
                [39.2761795775261, -76.61840513073078],
                {
}
            ).addTo(feature_group_e075b0a152a3f204744a99a04ff0e888);


            var icon_22c4ce578757b4885f96e86397a883b0 = L.AwesomeMarkers.icon(
                {
  &quot;markerColor&quot;: &quot;red&quot;,
  &quot;iconColor&quot;: &quot;white&quot;,
  &quot;icon&quot;: &quot;bed&quot;,
  &quot;prefix&quot;: &quot;fa&quot;,
  &quot;extraClasses&quot;: &quot;fa-rotate-0&quot;,
}
            );


        var popup_5f0f8e9e27ec76803101416e80e24aca = L.popup({
  &quot;maxWidth&quot;: &quot;100%&quot;,
});



                var html_edf7ae15659ad752810e8e148cad7672 = $(`&lt;div id=&quot;html_edf7ae15659ad752810e8e148cad7672&quot; style=&quot;width: 100.0%; height: 100.0%;&quot;&gt;The Baltimore Station&lt;/div&gt;`)[0];
                popup_5f0f8e9e27ec76803101416e80e24aca.setContent(html_edf7ae15659ad752810e8e148cad7672);



        marker_85b53e5dedae1f7f70494e5e8466e054.bindPopup(popup_5f0f8e9e27ec76803101416e80e24aca)
        ;




                marker_85b53e5dedae1f7f70494e5e8466e054.setIcon(icon_22c4ce578757b4885f96e86397a883b0);


            var marker_8752296c68cda17a82872911d9ccf5e5 = L.marker(
                [39.3295308350478, -76.65823143421247],
                {
}
            ).addTo(feature_group_e075b0a152a3f204744a99a04ff0e888);


            var icon_a587daa03eaf09c32458470cf8bf9762 = L.AwesomeMarkers.icon(
                {
  &quot;markerColor&quot;: &quot;red&quot;,
  &quot;iconColor&quot;: &quot;white&quot;,
  &quot;icon&quot;: &quot;bed&quot;,
  &quot;prefix&quot;: &quot;fa&quot;,
  &quot;extraClasses&quot;: &quot;fa-rotate-0&quot;,
}
            );


        var popup_ba459fb84c74c88eb09509358674f98a = L.popup({
  &quot;maxWidth&quot;: &quot;100%&quot;,
});



                var html_547a7c48bc5d93f8a9aa106158b86808 = $(`&lt;div id=&quot;html_547a7c48bc5d93f8a9aa106158b86808&quot; style=&quot;width: 100.0%; height: 100.0%;&quot;&gt;SVdP Cottage Avenue&lt;/div&gt;`)[0];
                popup_ba459fb84c74c88eb09509358674f98a.setContent(html_547a7c48bc5d93f8a9aa106158b86808);



        marker_8752296c68cda17a82872911d9ccf5e5.bindPopup(popup_ba459fb84c74c88eb09509358674f98a)
        ;




                marker_8752296c68cda17a82872911d9ccf5e5.setIcon(icon_a587daa03eaf09c32458470cf8bf9762);


            var marker_94b901f1c3cb8aebece015cd31d8b472 = L.marker(
                [39.28628342691768, -76.59563273804784],
                {
}
            ).addTo(feature_group_e075b0a152a3f204744a99a04ff0e888);


            var icon_4b7ab9df2e4265bed0430f1ae8d1d6df = L.AwesomeMarkers.icon(
                {
  &quot;markerColor&quot;: &quot;red&quot;,
  &quot;iconColor&quot;: &quot;white&quot;,
  &quot;icon&quot;: &quot;bed&quot;,
  &quot;prefix&quot;: &quot;fa&quot;,
  &quot;extraClasses&quot;: &quot;fa-rotate-0&quot;,
}
            );


        var popup_70adb7d8bfe5c7697f0086240f484bc3 = L.popup({
  &quot;maxWidth&quot;: &quot;100%&quot;,
});



                var html_f03c2003ecec2e803873a5363f3a9afc = $(`&lt;div id=&quot;html_f03c2003ecec2e803873a5363f3a9afc&quot; style=&quot;width: 100.0%; height: 100.0%;&quot;&gt;SVdP Ozanam House&lt;/div&gt;`)[0];
                popup_70adb7d8bfe5c7697f0086240f484bc3.setContent(html_f03c2003ecec2e803873a5363f3a9afc);



        marker_94b901f1c3cb8aebece015cd31d8b472.bindPopup(popup_70adb7d8bfe5c7697f0086240f484bc3)
        ;




                marker_94b901f1c3cb8aebece015cd31d8b472.setIcon(icon_4b7ab9df2e4265bed0430f1ae8d1d6df);


            var marker_be7cf0c7a6a4eddd4b343ae0a82e864e = L.marker(
                [39.29042689709753, -76.59843701536778],
                {
}
            ).addTo(feature_group_e075b0a152a3f204744a99a04ff0e888);


            var icon_f46fa1e5f87401adae6a30ef7edcf07e = L.AwesomeMarkers.icon(
                {
  &quot;markerColor&quot;: &quot;red&quot;,
  &quot;iconColor&quot;: &quot;white&quot;,
  &quot;icon&quot;: &quot;bed&quot;,
  &quot;prefix&quot;: &quot;fa&quot;,
  &quot;extraClasses&quot;: &quot;fa-rotate-0&quot;,
}
            );


        var popup_2163ff1e701bb1e4bce983db245a277d = L.popup({
  &quot;maxWidth&quot;: &quot;100%&quot;,
});



                var html_e6c53cce188ed2f58817c6f0c5460d81 = $(`&lt;div id=&quot;html_e6c53cce188ed2f58817c6f0c5460d81&quot; style=&quot;width: 100.0%; height: 100.0%;&quot;&gt;Earl’s Place&lt;/div&gt;`)[0];
                popup_2163ff1e701bb1e4bce983db245a277d.setContent(html_e6c53cce188ed2f58817c6f0c5460d81);



        marker_be7cf0c7a6a4eddd4b343ae0a82e864e.bindPopup(popup_2163ff1e701bb1e4bce983db245a277d)
        ;




                marker_be7cf0c7a6a4eddd4b343ae0a82e864e.setIcon(icon_f46fa1e5f87401adae6a30ef7edcf07e);


            var marker_3c7787b56f81a14267a4f15efe65d798 = L.marker(
                [39.28497002521616, -76.64384996790723],
                {
}
            ).addTo(feature_group_e075b0a152a3f204744a99a04ff0e888);


            var icon_becf664adb3b9fb1de70197ab177aed5 = L.AwesomeMarkers.icon(
                {
  &quot;markerColor&quot;: &quot;red&quot;,
  &quot;iconColor&quot;: &quot;white&quot;,
  &quot;icon&quot;: &quot;bed&quot;,
  &quot;prefix&quot;: &quot;fa&quot;,
  &quot;extraClasses&quot;: &quot;fa-rotate-0&quot;,
}
            );


        var popup_2476ed26f219feb60cb0351741b07ad7 = L.popup({
  &quot;maxWidth&quot;: &quot;100%&quot;,
});



                var html_f14db983470a8271c9b498c1e10a5ef8 = $(`&lt;div id=&quot;html_f14db983470a8271c9b498c1e10a5ef8&quot; style=&quot;width: 100.0%; height: 100.0%;&quot;&gt;VOA Pratt House&lt;/div&gt;`)[0];
                popup_2476ed26f219feb60cb0351741b07ad7.setContent(html_f14db983470a8271c9b498c1e10a5ef8);



        marker_3c7787b56f81a14267a4f15efe65d798.bindPopup(popup_2476ed26f219feb60cb0351741b07ad7)
        ;




                marker_3c7787b56f81a14267a4f15efe65d798.setIcon(icon_becf664adb3b9fb1de70197ab177aed5);


            var marker_249345881692d0ea277c4c15bfa5c274 = L.marker(
                [39.305920480629375, -76.63163267318743],
                {
}
            ).addTo(feature_group_e075b0a152a3f204744a99a04ff0e888);


            var icon_3c3701228dc161cfe203210256041cbb = L.AwesomeMarkers.icon(
                {
  &quot;markerColor&quot;: &quot;red&quot;,
  &quot;iconColor&quot;: &quot;white&quot;,
  &quot;icon&quot;: &quot;bed&quot;,
  &quot;prefix&quot;: &quot;fa&quot;,
  &quot;extraClasses&quot;: &quot;fa-rotate-0&quot;,
}
            );


        var popup_9fb3951da1cf4ef74dd5f0e2e3f03be2 = L.popup({
  &quot;maxWidth&quot;: &quot;100%&quot;,
});



                var html_81cc600489c8131693b167374940af5d = $(`&lt;div id=&quot;html_81cc600489c8131693b167374940af5d&quot; style=&quot;width: 100.0%; height: 100.0%;&quot;&gt;YWCA - Druid House&lt;/div&gt;`)[0];
                popup_9fb3951da1cf4ef74dd5f0e2e3f03be2.setContent(html_81cc600489c8131693b167374940af5d);



        marker_249345881692d0ea277c4c15bfa5c274.bindPopup(popup_9fb3951da1cf4ef74dd5f0e2e3f03be2)
        ;




                marker_249345881692d0ea277c4c15bfa5c274.setIcon(icon_3c3701228dc161cfe203210256041cbb);


            var marker_a6379575e6a26c480e61aa8f26faa747 = L.marker(
                [39.27363985230644, -76.61476039219282],
                {
}
            ).addTo(feature_group_e075b0a152a3f204744a99a04ff0e888);


            var icon_bf36dc710b87e024359c7784fa21d7d1 = L.AwesomeMarkers.icon(
                {
  &quot;markerColor&quot;: &quot;red&quot;,
  &quot;iconColor&quot;: &quot;white&quot;,
  &quot;icon&quot;: &quot;bed&quot;,
  &quot;prefix&quot;: &quot;fa&quot;,
  &quot;extraClasses&quot;: &quot;fa-rotate-0&quot;,
}
            );


        var popup_11fa9d2bbaed28b3f90148b786795c93 = L.popup({
  &quot;maxWidth&quot;: &quot;100%&quot;,
});



                var html_d0557ddc0162d8123b29a0f99674012e = $(`&lt;div id=&quot;html_d0557ddc0162d8123b29a0f99674012e&quot; style=&quot;width: 100.0%; height: 100.0%;&quot;&gt;American Rescue Workers&lt;/div&gt;`)[0];
                popup_11fa9d2bbaed28b3f90148b786795c93.setContent(html_d0557ddc0162d8123b29a0f99674012e);



        marker_a6379575e6a26c480e61aa8f26faa747.bindPopup(popup_11fa9d2bbaed28b3f90148b786795c93)
        ;




                marker_a6379575e6a26c480e61aa8f26faa747.setIcon(icon_bf36dc710b87e024359c7784fa21d7d1);


            feature_group_e075b0a152a3f204744a99a04ff0e888.addTo(map_0726b6ba9a4d2cc5792f26bf69fef3e1);


        function geo_json_22b0e65d633ec17331051cd30685de58_styler(feature) {
            switch(feature.id) {
                default:
                    return {&quot;color&quot;: &quot;green&quot;, &quot;fillColor&quot;: &quot;green&quot;, &quot;fillOpacity&quot;: 0.1};
            }
        }

        function geo_json_22b0e65d633ec17331051cd30685de58_onEachFeature(feature, layer) {
            layer.on({
            });
        };
        var geo_json_22b0e65d633ec17331051cd30685de58 = L.geoJson(null, {
                onEachFeature: geo_json_22b0e65d633ec17331051cd30685de58_onEachFeature,

                style: geo_json_22b0e65d633ec17331051cd30685de58_styler,
            ...{
}
        });

        function geo_json_22b0e65d633ec17331051cd30685de58_add (data) {
            geo_json_22b0e65d633ec17331051cd30685de58
                .addData(data);
        }
            geo_json_22b0e65d633ec17331051cd30685de58_add({&quot;bbox&quot;: [39.28173894494271, -76.62750822521143, 39.287861055561315, -76.62609171418707], &quot;features&quot;: [{&quot;bbox&quot;: [39.28173894494271, -76.62750822521143, 39.287861055561315, -76.62609171418707], &quot;geometry&quot;: {&quot;coordinates&quot;: [[[39.28569574154672, -76.6274772267344], [39.28540453443912, -76.62749427863706], [39.28510750590661, -76.62750464472808], [39.28480751621959, -76.62750822521143], [39.2845074541545, -76.62750498561736], [39.28421020918464, -76.62749495713385], [39.283918643663895, -76.62747823630669], [39.283635565271446, -76.62745498411097], [39.2833636999821, -76.627425424403], [39.283105665822305, -76.62738984176728], [39.28286394766427, -76.62734857877933], [39.28264087330044, -76.62730203271055], [39.28243859102913, -76.62725065170663], [39.282259048966125, -76.62719493047618], [39.282103976282386, -76.62713540553104], [39.28197486654788, -76.62707265002366], [39.28187296334264, -76.62700726823154], [39.28179924827329, -76.62693988974104], [39.28175443151107, -76.62687116338724], [39.28173894494271, -76.62680175100736], [39.28175293800055, -76.62673232106832], [39.28179627621228, -76.6266635422294], [39.281868542484915, -76.62659607690246], [39.28196904111097, -76.62653057487104], [39.28209680445857, -76.62646766703058], [39.28225060228182, -76.62640795930965], [39.28242895356146, -76.62635202683094], [39.282630140762656, -76.62630040836856], [39.28285222637251, -76.62625360115494], [39.28309307155826, -76.62621205608772], [39.283350356766334, -76.62617617338266], [39.28362160406421, -76.62614629871514], [39.283904201009456, -76.62612271988695], [39.28419542581585, -76.6261056640511], [39.28449247357453, -76.62609529552121], [39.28479248327628, -76.62609171418707], [39.28509256537544, -76.6260949545511], [39.285389829628855, -76.62610498539576], [39.28568141294157, -76.62612171008452], [39.28596450695068, -76.62614496749386], [39.286236385081196, -76.62617453356715], [39.28649442881326, -76.62621012347523], [39.28673615290678, -76.62625139436315], [39.28695922934114, -76.62629794865589], [39.287161509738574, -76.62634933789174], [39.28734104605525, -76.62640506704572], [39.28749610934104, -76.62646459930153], [39.28762520638712, -76.62652736122573], [39.287727094101065, -76.62659274829426], [39.28780079147163, -76.62666013071794], [39.287845589007674, -76.6267288595106], [39.287861055561315, -76.62679827274127], [39.28784704246939, -76.62686770191036], [39.2878036849741, -76.6269364783881], [39.2877314009095, -76.62700393985348], [39.28763088666682, -76.62706943667126], [39.287503110478, -76.62713233814641], [39.2873493030825, -76.62719203859497], [39.287170945867416, -76.6272479631737], [39.286969756595624, -76.62729957341213], [39.28674767285965, -76.6273463723938], [39.28650683342025, -76.6273879095375], [39.286249557610155, -76.6274237849319], [39.285978323000684, -76.6274536531826], [39.28569574154672, -76.6274772267344]]], &quot;type&quot;: &quot;Polygon&quot;}, &quot;id&quot;: &quot;0&quot;, &quot;properties&quot;: {}, &quot;type&quot;: &quot;Feature&quot;}], &quot;type&quot;: &quot;FeatureCollection&quot;});



            geo_json_22b0e65d633ec17331051cd30685de58.addTo(map_0726b6ba9a4d2cc5792f26bf69fef3e1);


        function geo_json_1b4f50289d4fcb649fa8b1506c7c2fde_styler(feature) {
            switch(feature.id) {
                default:
                    return {&quot;color&quot;: &quot;yellow&quot;, &quot;fillColor&quot;: &quot;yellow&quot;, &quot;fillOpacity&quot;: 0.1};
            }
        }

        function geo_json_1b4f50289d4fcb649fa8b1506c7c2fde_onEachFeature(feature, layer) {
            layer.on({
            });
        };
        var geo_json_1b4f50289d4fcb649fa8b1506c7c2fde = L.geoJson(null, {
                onEachFeature: geo_json_1b4f50289d4fcb649fa8b1506c7c2fde_onEachFeature,

                style: geo_json_1b4f50289d4fcb649fa8b1506c7c2fde_styler,
            ...{
}
        });

        function geo_json_1b4f50289d4fcb649fa8b1506c7c2fde_add (data) {
            geo_json_1b4f50289d4fcb649fa8b1506c7c2fde
                .addData(data);
        }
            geo_json_1b4f50289d4fcb649fa8b1506c7c2fde_add({&quot;bbox&quot;: [39.27561683636795, -76.62892449385814, 39.29398316816871, -76.62467496072814], &quot;features&quot;: [{&quot;bbox&quot;: [39.27561683636795, -76.62892449385814, 39.29398316816871, -76.62467496072814], &quot;geometry&quot;: {&quot;coordinates&quot;: [[[39.28748705233033, -76.62883152008924], [39.286613484097934, -76.62888266400101], [39.28572245617173, -76.62891375495967], [39.284822547146774, -76.62892449385814], [39.28392242104577, -76.62891477738515], [39.28303074397951, -76.62888469901598], [39.28215610078716, -76.62883454811588], [39.28130691245558, -76.62876480716527], [39.28049135510908, -76.6286761471323], [39.27971728134679, -76.62856942103703], [39.27899214468167, -76.62844565576803], [39.27832292780675, -76.62830604222907], [39.277716075377604, -76.62815192390941], [39.277177431957206, -76.6279847839861], [39.27671218572011, -76.62780623108127], [39.276324818459216, -76.62761798380957], [39.276019062376626, -76.62742185426352], [39.27579786407761, -76.62721973059381], [39.275663356115885, -76.62701355885187], [39.27561683636795, -76.62680532426786], [39.275658755438144, -76.62659703214403], [39.27578871221927, -76.62639068854678], [39.276005457655884, -76.62618828098336], [39.27630690667712, -76.62599175924943], [39.27669015818855, -76.62580301663218], [39.27715152293346, -76.62562387165099], [39.277686558958386, -76.62545605051243], [39.27829011434373, -76.62530117044975], [39.27895637679014, -76.62516072410861], [39.279678929583575, -76.62503606513121], [39.280450813401195, -76.62492839507887], [39.28126459336202, -76.62483875182085], [39.282112430675646, -76.62476799950272], [39.28298615819729, -76.62471682019209], [39.28387735915912, -76.62468570728372], [39.284777448316454, -76.62467496072814], [39.28567775472417, -76.62468468413088], [39.28656960534241, -76.6247147837504], [39.28744440866251, -76.62476496940478], [39.288293737543945, -76.62483475727802], [39.289109410461194, -76.62492347459875], [39.28988357037376, -76.62503026614537], [39.29060876045803, -76.62515410251406], [39.29127799596812, -76.62529379006874], [39.291884831532236, -76.62544798247578], [39.292423423235626, -76.62561519371094], [39.29288858489106, -76.62579381241166], [39.293275837956195, -76.62598211743475], [39.29358145461729, -76.6261782944684], [39.293802493627055, -76.62638045353695], [39.29393682855315, -76.62658664722862], [39.29398316816871, -76.62679488946974], [39.29394106879183, -76.62700317466381], [39.2938109384587, -76.62720949701072], [39.29359403289436, -76.62741186981985], [39.29329244332327, -76.62760834463117], [39.29290907624109, -76.62779702996066], [39.29244762534587, -76.62797610949049], [39.2919125359018, -76.62814385952936], [39.29130896188128, -76.62829866557632], [39.2906427162999, -76.62843903783009], [39.2899202152235, -76.62856362549563], [39.28914841598752, -76.62867122975209], [39.28833475022246, -76.62876081525869], [39.28748705233033, -76.62883152008924]]], &quot;type&quot;: &quot;Polygon&quot;}, &quot;id&quot;: &quot;0&quot;, &quot;properties&quot;: {}, &quot;type&quot;: &quot;Feature&quot;}], &quot;type&quot;: &quot;FeatureCollection&quot;});



            geo_json_1b4f50289d4fcb649fa8b1506c7c2fde.addTo(map_0726b6ba9a4d2cc5792f26bf69fef3e1);


            var circle_55b9da4a2ef9e09997826d2877c891ff = L.circle(
                [39.2848, -76.6268],
                {&quot;bubblingMouseEvents&quot;: true, &quot;color&quot;: &quot;blue&quot;, &quot;dashArray&quot;: null, &quot;dashOffset&quot;: null, &quot;fill&quot;: false, &quot;fillColor&quot;: &quot;blue&quot;, &quot;fillOpacity&quot;: 0.2, &quot;fillRule&quot;: &quot;evenodd&quot;, &quot;lineCap&quot;: &quot;round&quot;, &quot;lineJoin&quot;: &quot;round&quot;, &quot;opacity&quot;: 1.0, &quot;radius&quot;: 1609.34, &quot;stroke&quot;: true, &quot;weight&quot;: 2}
            ).addTo(map_0726b6ba9a4d2cc5792f26bf69fef3e1);


            var circle_8d1384b3ca788b073f752efb7871f5f6 = L.circle(
                [39.2848, -76.6268],
                {&quot;bubblingMouseEvents&quot;: true, &quot;color&quot;: &quot;red&quot;, &quot;dashArray&quot;: null, &quot;dashOffset&quot;: null, &quot;fill&quot;: false, &quot;fillColor&quot;: &quot;red&quot;, &quot;fillOpacity&quot;: 0.2, &quot;fillRule&quot;: &quot;evenodd&quot;, &quot;lineCap&quot;: &quot;round&quot;, &quot;lineJoin&quot;: &quot;round&quot;, &quot;opacity&quot;: 1.0, &quot;radius&quot;: 4828.02, &quot;stroke&quot;: true, &quot;weight&quot;: 2}
            ).addTo(map_0726b6ba9a4d2cc5792f26bf69fef3e1);


            var heat_map_f7c9e5515529539ddfe44ce0b0489bb7 = L.heatLayer(
                [[39.27363985230644, -76.61476039219282], [39.29786541144464, -76.60326105915476], [39.29167942657049, -76.59996552078663], [39.28075133239391, -76.61406447836761], [39.29105020437977, -76.60293248355717], [39.304957344305144, -76.58796562511974], [39.29046383527283, -76.6018854041655], [39.30174696401811, -76.64517677848862], [39.291612593818414, -76.60022671646288], [39.29370441331408, -76.606262560335], [39.31830481986578, -76.613995968731], [39.31084925041601, -76.61405196324532], [39.30298833545443, -76.61357192318277], [39.29828493326264, -76.6101713910694], [39.293777663886225, -76.6174741908216], [39.31564499777156, -76.61650251143895], [39.31769450620435, -76.61196703102117], [39.340193600655745, -76.6072150570671], [39.31798560159025, -76.57909920708497], [39.30497316782566, -76.58749425008823], [39.29046383527283, -76.6018854041655], [39.31121960256198, -76.67631966621622], [39.31121960256198, -76.67631966621622], [39.32560915784916, -76.60299377730044], [39.29370441331408, -76.606262560335], [39.29872855966124, -76.61898072865594], [39.31830481986578, -76.613995968731], [39.31017049782587, -76.61822160486902], [39.30298833545443, -76.61357192318277], [39.2969189028437, -76.62156707390085], [39.30912895418698, -76.59344372933701], [39.2761795775261, -76.61840513073078], [39.3295308350478, -76.65823143421247], [39.28628342691768, -76.59563273804784], [39.29042689709753, -76.59843701536778], [39.28497002521616, -76.64384996790723], [39.305920480629375, -76.63163267318743], [39.27363985230644, -76.61476039219282], [39.3101783, -76.6180097], [39.29734385, -76.61062928705718], [39.3151035, -76.6187862], [39.2949306, -76.6166875], [39.2819967, -76.6328189], [39.35537105, -76.55801906929733], [39.319821899999994, -76.62158813219636], [39.3258218, -76.6095485], [39.3100474, -76.6380548], [39.327006299999994, -76.59390961327433], [39.3101783, -76.6180097], [39.22421115, -76.68556148122171], [39.29378545, -76.6093553524615], [39.3019953, -76.60288246925838], [39.3480927, -76.6767704], [39.3025844, -76.6157514], [39.23793125, -76.6066009837206], [39.2862365, -76.5667566446291], [39.2505284, -76.6223205], [39.3098552, -76.6418471231494], [39.2946388, -76.59930889030557], [39.35537105, -76.55801906929733], [39.288986, -76.64942222258527], [39.35289645, -76.66228716247511], [39.35412555, -76.66539357378952], [39.29121285, -76.5490695530589], [39.296439199999995, -76.59239403267401], [39.2998731, -76.59327771844121], [39.3575548, -76.58584727178213], [39.25184350000001, -76.61403126369895], [39.3295258, -76.61483957750231], [39.2932695, -76.61280493859464], [39.3630752, -76.65282334820517], [39.28827365, -76.62439437177966], [39.3138027, -76.6164449], [39.3156377, -76.6165108], [39.3158175, -76.6167198], [39.3169269, -76.6538936], [39.2997678, -76.5751002691712], [39.2819967, -76.6328189], [39.3256619, -76.6030961], [39.3179037, -76.61004974068197], [39.2914486, -76.6088784]],
                {
  &quot;minOpacity&quot;: 0.5,
  &quot;maxZoom&quot;: 18,
  &quot;radius&quot;: 40,
  &quot;blur&quot;: 30,
}
            );


            heat_map_f7c9e5515529539ddfe44ce0b0489bb7.addTo(map_0726b6ba9a4d2cc5792f26bf69fef3e1);


            var layer_control_24489d86c35c38949b5f867a56a7d32f_layers = {
                base_layers : {
                    &quot;openstreetmap&quot; : tile_layer_f97ed5ae929afc9075704ec97a8ffe2a,
                },
                overlays :  {
                    &quot;\ud83c\udfe5 Social Shelters&quot; : feature_group_84842d008a6f773fe6b54c40c9a5e774,
                    &quot;\ud83d\udc65 Homeless Shelters&quot; : feature_group_e075b0a152a3f204744a99a04ff0e888,
                    &quot;macro_element_22b0e65d633ec17331051cd30685de58&quot; : geo_json_22b0e65d633ec17331051cd30685de58,
                    &quot;macro_element_1b4f50289d4fcb649fa8b1506c7c2fde&quot; : geo_json_1b4f50289d4fcb649fa8b1506c7c2fde,
                    &quot;macro_element_f7c9e5515529539ddfe44ce0b0489bb7&quot; : heat_map_f7c9e5515529539ddfe44ce0b0489bb7,
                },
            };
            let layer_control_24489d86c35c38949b5f867a56a7d32f = L.control.layers(
                layer_control_24489d86c35c38949b5f867a56a7d32f_layers.base_layers,
                layer_control_24489d86c35c38949b5f867a56a7d32f_layers.overlays,
                {
  &quot;position&quot;: &quot;topright&quot;,
  &quot;collapsed&quot;: false,
  &quot;autoZIndex&quot;: true,
}
            ).addTo(map_0726b6ba9a4d2cc5792f26bf69fef3e1);



            tile_layer_f97ed5ae929afc9075704ec97a8ffe2a.addTo(map_0726b6ba9a4d2cc5792f26bf69fef3e1);


                marker_056bf807e92f1f878d89572dd2a5f1a8.setIcon(icon_fec6b7031614eda68fe8824571217136);


                marker_45567c63a8a6f559459a2b6b30deb8bf.setIcon(icon_bcbf4f7b601eaaab7962814fcfd9031a);


                marker_a99f7cebb2cbcad8535ff66fea2516fd.setIcon(icon_f411f58c4e7340021c7686b9bd709160);


                marker_fcd71c4c85a58a47dd4adca3adb308c5.setIcon(icon_e1d9c643c773405ee5a85febb8f105c9);


                marker_47e789a43e8244ca0e3d5dc651d30578.setIcon(icon_fb96c14c3051a30cff6f0c4568a77789);


                marker_a69a3534dacacf0e33da58efe02aa40b.setIcon(icon_91d81bd42e7cfbbbb4bf4d561dcc15c8);


                marker_3319c4d339e2a24b4dd34367087deb74.setIcon(icon_c29a55d5f2a18dc5327ef9eeb160d49d);


                marker_1d5e33e2b5837ca78deb9ebc90c0c3dd.setIcon(icon_de7c4653f4f57ffa692859d28cc8d702);


                marker_50df6c3c0e7d3e7197dfb0c730930533.setIcon(icon_f2e567cabd053ecd1ea382a21d49cb1f);


                marker_610e1b5a686fdfbf3c3ebc5f9897bc1a.setIcon(icon_eda59542deef7cc221c978ad76a572f1);


                marker_fcd57ed754f442ee0d1799293eb50d6e.setIcon(icon_85ec31f1fcd60d8ca8f86199828665b9);


                marker_3210a12b75cbf20ccdc62775f986a755.setIcon(icon_595bcd8cf25fbd619e63b126f0b4cca3);


                marker_54fcbe0cd2e59d91c459663369a69b31.setIcon(icon_dbb642c7df79c92e01a22642db26965d);


                marker_aaed9179e24c8899e5817f61f345c6d6.setIcon(icon_fd1cd18e136bc8320667b057d5b5faac);


                marker_8b19276a858962a1818b61f0bbdde53d.setIcon(icon_76e914e088b4b32d1a41709680d93439);


                marker_a3b1a2811b0837a2f2945c8c359407b3.setIcon(icon_e0470d21ac5d7e3270aac635353b9a6d);


                marker_5a81ae604b9ac6974c42e4faf5169388.setIcon(icon_1fe2e32c3021352b48f2f158a4d90ce1);


                marker_60490a8e03487908ff30504293bc39b6.setIcon(icon_1da7efd1d8f473e5840c9d23b37b01f4);


                marker_c14588d7be5109443747195d50c94395.setIcon(icon_9b3cba67c01ced5c8667e6e2a0ec9f4f);


                marker_f4d14b57c2af6f5cc10ff67a0e437c30.setIcon(icon_cbba7938664758fdd7f6caa58981b073);


                marker_fec49ab2cbf86bdd74948bc4c496a229.setIcon(icon_4c7f6de5c0e9a94f1031f9e5261b53a6);


                marker_7b3dc05c43e2411730d30ba1b8867e76.setIcon(icon_666200e2057b717fb429b0d7c16223ae);


                marker_392997f081affa42ab054aba7523761b.setIcon(icon_00dba36270ad546a5763c89cc20a6190);


                marker_7d0807e76fc03e424dc69da21a9412ef.setIcon(icon_3faffe95ed5f4cc79dd8c0997ae9be3a);


                marker_cd663ba45f389209fb2045ea36b0c42c.setIcon(icon_c5aee8dae264b32788b7c8164b47c246);


                marker_591dd0b4c1d056fe13d8b144510b0a05.setIcon(icon_af569e678718787ce75825691641c284);


                marker_02c712e089d6f2acf6bedc8a00d46782.setIcon(icon_8e6d3740c194d8f3d7755d0c9116a5e0);


                marker_a8d20e14d0ab8d212ffc498da1d7e85e.setIcon(icon_3d0e1708d86873d4e5dd17f887d96b23);


                marker_0353bd96dcb5205df09c9eaff2a7279f.setIcon(icon_12e64f66cd39f92d396bee0fe904ec22);


                marker_7133d7d057d83f76018ab195703eaad6.setIcon(icon_f099f6af2e0dabbbf7bd6e8affbf0916);


                marker_04ede81bbdf5b02bc182bfa22eb811a4.setIcon(icon_adc05abbed0c1c302c0f08335df1586f);


                marker_57837dd4b57d235025759dd46bb8f23e.setIcon(icon_da0094eb6ad4708cd10d766bbbaf9cfc);


                marker_ad904f0e1aeb1be89b1226e6da4c7764.setIcon(icon_0eb52fda30b240724c17cdd9a5e9519b);


                marker_dd16d89e3aada594ec7af99ce0208f62.setIcon(icon_d7a3eaa73c63a5cbcb0ac881db046276);


                marker_12469d37f268146b7463b0fa728c678e.setIcon(icon_2eef4dbf3566885eebe0252bb04cbecd);


                marker_9e33b5f9cca7381ce165a05a122373a3.setIcon(icon_1e22d21cc0ad21cbbace9326897bb319);


                marker_c9f62831a0f3a9818da2a7dd934ae5f6.setIcon(icon_c1bd635a1837ed4213e0f38e7ee24536);


                marker_7a35fdfff5ec8f0b60724b1bde1ce6e3.setIcon(icon_fcea37cc59bd89bb45999e5b865a52cd);


                marker_cf3c8ce99410a9abf98444b6f57deae2.setIcon(icon_cb7780e2b3540cf703e11019e64633a2);


                marker_6bb0e0a9a5dbc8cb8e3498fa7e67fc44.setIcon(icon_83b3598f7bab767f95a6916735341ae5);


                marker_a735c89d37515c04bd4f576d556d617d.setIcon(icon_3cade699029dff0b5872fb5571dd7e97);


                marker_c2db52b96962f5f5f082982f0b6f940f.setIcon(icon_719897d215f2e3adab7f0c35d0fc59a9);


                marker_6bc58f9094901ed85fa1bbe202eb10e0.setIcon(icon_5d79953805491d695da848065985bd8d);


            feature_group_84842d008a6f773fe6b54c40c9a5e774.addTo(map_0726b6ba9a4d2cc5792f26bf69fef3e1);


                marker_46a7fd6934fe47089f7f8faa518efc31.setIcon(icon_2240a8ab507e6c1df85421d9884a7fb0);


                marker_013feea8e3f7640210fcff6a9392e63d.setIcon(icon_ce0fafb684116ca58f3856069c1f3c26);


                marker_5d9d9e57ea5486034c7f6fbdd0134944.setIcon(icon_61dbebd6790b397f0952fa4d703ec8f4);


                marker_468125698b2c91f5ece84b5860967c6f.setIcon(icon_35f13e43c102f1f180bc0a1741cf01d8);


                marker_9deefa6758449a651b2f3922dd4b5488.setIcon(icon_aa9bee276a0061cd3180280448403842);


                marker_25ff97f2fc84735d250db998144f0e89.setIcon(icon_ac025335c62b0982406db40c87eae943);


                marker_3bf5b6a8911b59db0be265f95d60c075.setIcon(icon_48a5340a2a6a72e7913aa586309ed3b8);


                marker_e0ed71dcabdf91327069b764e0a16982.setIcon(icon_2ad31ba0a49c11ff696403bc1f0e514d);


                marker_35f9347c6644d0c4393aa39522662ec8.setIcon(icon_e8dcd569e1be9e3772ea79d4c0d85536);


                marker_2a0e5fa213f5f0d036bc82d347f5666b.setIcon(icon_72eb3336b62f9cb047f50f7e103b9bad);


                marker_8429012b503e09d1e354493c2a9d69f2.setIcon(icon_6bcf299fa7210656260a62481d9ba676);


                marker_9c9ea96ca729aeac59f9471e8ed4efc0.setIcon(icon_9d554022608efae699fae6bb45d87f5b);


                marker_01db665b233ae8cf77dc6017a1047d98.setIcon(icon_4e16cfaaaf8831d35a1cdb329f5d4c12);


                marker_5d83711d6d12310c2aec092a3a1f1d45.setIcon(icon_cd941a5b7039d597984770f9e871fa6b);


                marker_1480a616ea2987323ff2af3856e7380f.setIcon(icon_ec428b0e26d126b68691f7d490cc2f87);


                marker_e31bb071e47c28e17e37f9f4f876e025.setIcon(icon_be5eeb5e1bcca8718ffaf8c26e05b4bf);


                marker_7f1f5f88f10dc5dd029ec8f940d51fbc.setIcon(icon_b7457967856638b318fe34f2c6fb6faf);


                marker_b8a88e0dc009ee87c30a15ac8d0e198e.setIcon(icon_b520dda87154464c6f05fc7c9010c920);


                marker_c3f0f11cc5abb5bdf4e551bb374b49a4.setIcon(icon_5fdd99c6480980ce99b0420557915d4c);


                marker_5a588151b646890c4013810d7326b644.setIcon(icon_92e4671ea52a5812e5eba5b272622fd7);


                marker_b1d1937c922febb7db988e783c27114e.setIcon(icon_ad1a45ef9d860c1f26857e42489b4206);


                marker_13f8b0c4ca9e44378f4203a24b5670a5.setIcon(icon_05e41c44dc63f13ecabdfd54894e660a);


                marker_f82a4187f033ac6f1c530845063fe62c.setIcon(icon_89cc2528a8919ccd2387d57f5bb265e6);


                marker_81026c30398112f2e9f3e66576396857.setIcon(icon_5484ff2eaeab02c9e812f07720549e6c);


                marker_33f357ee96afabf972169debaeb3b9af.setIcon(icon_f6f06edb43ba0f58f093aa4186a9f1ac);


                marker_4a597b9f8d631daa9e445afd8c66409d.setIcon(icon_4322542e807c6aecd73049c1aa6880c9);


                marker_3351fcac84fc35807b4b3d5bc377fc04.setIcon(icon_34077a4ddd1a044310bd6a488254edb1);


                marker_e3b45d2ad6b46b3c7099fb0d97ac0395.setIcon(icon_e23a5a18f1f6239d55ef099e4aeb0548);


                marker_823d5d056ca4fb14e3cbc2f3407fccf3.setIcon(icon_80a36d3d673f443cb94872f41db9d0f8);


                marker_07097bf502a171b9f65d5163a0302313.setIcon(icon_5b426586fa4a0ad676db5544b149175c);


                marker_926bc456bc42f79dcd132e9a7a87ac1d.setIcon(icon_79ddd1ac8ed48de7c21971b556afb445);


                marker_85b53e5dedae1f7f70494e5e8466e054.setIcon(icon_22c4ce578757b4885f96e86397a883b0);


                marker_8752296c68cda17a82872911d9ccf5e5.setIcon(icon_a587daa03eaf09c32458470cf8bf9762);


                marker_94b901f1c3cb8aebece015cd31d8b472.setIcon(icon_4b7ab9df2e4265bed0430f1ae8d1d6df);


                marker_be7cf0c7a6a4eddd4b343ae0a82e864e.setIcon(icon_f46fa1e5f87401adae6a30ef7edcf07e);


                marker_3c7787b56f81a14267a4f15efe65d798.setIcon(icon_becf664adb3b9fb1de70197ab177aed5);


                marker_249345881692d0ea277c4c15bfa5c274.setIcon(icon_3c3701228dc161cfe203210256041cbb);


                marker_a6379575e6a26c480e61aa8f26faa747.setIcon(icon_bf36dc710b87e024359c7784fa21d7d1);


            feature_group_e075b0a152a3f204744a99a04ff0e888.addTo(map_0726b6ba9a4d2cc5792f26bf69fef3e1);


            geo_json_22b0e65d633ec17331051cd30685de58.addTo(map_0726b6ba9a4d2cc5792f26bf69fef3e1);


            geo_json_1b4f50289d4fcb649fa8b1506c7c2fde.addTo(map_0726b6ba9a4d2cc5792f26bf69fef3e1);


            heat_map_f7c9e5515529539ddfe44ce0b0489bb7.addTo(map_0726b6ba9a4d2cc5792f26bf69fef3e1);

&lt;/script&gt;
&lt;/html&gt;" style="position:absolute;width:100%;height:100%;left:0;top:0;border:none !important;" allowfullscreen webkitallowfullscreen mozallowfullscreen></iframe></div></div>


# Conclusion

Our project was about figuring out which parts of Southwest Baltimore are struggling the most when it comes to health and access to services. We noticed a significant area around Pauls Place is lacking health or social welfare facilities which is possibly overburdening the two health facilities and three social welfare centers in Southwest Baltimore. We found that half the deaths in that area could’ve been prevented if people had better access to healthcare. There are more liquor stores in these neighborhoods than in other parts of the city, and fewer grorcery stores or places to get healthy food.  
This work matters because it helps places like Paul’s Place and others know where to focus their time and energy. With limited resources, it’s important to know which neighborhoods need the most help. In addition, the predictive model we are creating can be used in the future to keep track of how things change over time. 

Some of the challenges we had was that we didn’t have a lot of data directly from Paul’s Place, so we had to depend on public data like such as US Census Bureau, Open Baltimore, and health department dataset. Another challenge was the access to resources Paul's Place has which makes it harder to determine how we’d want to gather new patient information and store this data. 
Moving forward, it would help a lot if more data was collected internally. Things like online forms or surveys could really make a difference. Being able to collate and store current user data would help get insights on the people going to Paul’s Place and the community. 

# References.

-American Community Survey (ACS) from the U.S. Census Bureau.https://www.census.gov/programs-surveys/acs/news/data-releases/2023.html.

-Hospitals. (n.d.). https://data.baltimorecity.gov/datasets/e37ce649df4344dab174b34593b1c4b6_0/explore?location=39.307459%2C-76.628697%2C11.39&showTable=true.

-Homeless shelters. (n.d.). https://data.baltimorecity.gov/datasets/710b935a4e864284ad5da9019fe5fca2_0/explore?location=39.306731%2C-76.588463%2C11.39&showTable=true.

-Southwest Baltimore neighborhood in Baltimore, Maryland (MD), 21223, 21229, 21230, 21216, 21207 subdivision profile - real estate, apartments, condos, homes, community, population, jobs, income, streets. (n.d.). https://www.city-data.com/neighborhood/Southwest-Baltimore-Baltimore-MD.html.


```python

```
