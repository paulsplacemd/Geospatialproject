## Overview

Our project highlights healthcare accessibility challenges within Southwest Baltimore, focusing on identifying health service gaps and resource distribution. Residents face difficulties accessing healthcare due to distance, poverty, and transportation limitations, this initiative aims to leverage data science to pinpoint these areas.

In collaboration with Dr. Megan, Program Lead for Health and Wellness at Paul’s Place, our team is exploring the spatial relationships between community demographics, environmental factors, and health outcomes. By applying analytical and geospatial techniques, we aim to uncover healthcare deserts and areas vulnerable to future health concerns.

This specific aspect of the project focuses on creating a map highlighting healthcare service deserts in the Southwest Baltimore community and a geospatial locator for homeless shelters and social welfare centers within a commutable distance of Paul's Place in Southwest Baltimore. The resulting maps visualize these, aiding community outreach efforts by Paul's Place and suggesting potential health service deserts within the region.

## Key Features

-  Visualization of homeless shelters and social welfare centers near Paul's Place in Southwest Baltimore.
-  Aims to highlight areas with limited access to health and social welfare resources.
-  Aims to support data-driven decisions for resource allocation and service delivery.


## Getting Started

### Installation

- Ensure you have the necessary Python libraries installed in your environment:
    ```
    pip install pandas geopandas folium pyproj shapely osmnx
    ```

### Data Sources

This project utilizes data from the American Community Survey (ACS) for economic demographics and publicly available datasets for homeless and social welfare shelter locations. The specific URLs for the shelter data are:

-   Homeless Shelters: [https://raw.githubusercontent.com/paulsplacemd/paulsplacelocator/main/Homeless_Shelters.csv](https://raw.githubusercontent.com/paulsplacemd/paulsplacelocator/main/Homeless_Shelters.csv)
-   Social Welfare Shelters: [https://raw.githubusercontent.com/paulsplacemd/paulsplacelocator/main/baltimore_help_social_health_welfare_shelters_locations.csv](https://raw.githubusercontent.com/paulsplacemd/paulsplacelocator/main/baltimore_help_social_health_welfare_shelters_locations.csv)


## Exploratory Data Analysis and Visualization

An HTML-based interactive map was created to visualize healthcare service deserts in the Southwest Baltimore community and the locations of homeless shelters and social welfare centers within Southwest Baltimore and surrounding areas. This visualization aims to highlight potential health and wellness service deserts, providing insights for Paul's Place and other organizations to strategically focus their health outreach efforts. The map can be accessed [here](link here) and [here](link here).

## Conclusion

This project sheds light on the potential lack of health and social welfare resources in the vicinity of Paul's Place within Southwest Baltimore. The analysis suggests a possible overburdening of the existing facilities, which can contribute to adverse health outcomes for the community.
This work is crucial for informing organizations like Paul's Place about areas needing the most support and for making strategic decisions regarding resource allocation. The geospatial mapping and the developed data processing techniques can be valuable tools for ongoing monitoring and adaptation of community outreach efforts.

### Challenges

- **Limited Direct Data:** We relied heavily on publicly available datasets, which did not fully capture the specific needs and challenges of the community served by Paul's Place.
- **Resource Constraints:** Understanding the specific resource access limitations faced by Paul's Place was a challenge in tailoring data collection strategies.

### Future Directions

- **Internal Data Collection:** Implementing methods for collating data directly from individuals accessing Paul's Place services (e.g., online forms, surveys) could provide targeted insights.
- **Enhanced Data Integration:** Combining internal data with public datasets could lead to a robust understanding of community health needs.
- **Predictive Modeling:** Further development of our predictive models could help anticipate future health risks and inform proactive interventions.
- 

## Contact

For any inquiries or further information, please feel free to reach out.

[Team Name]
[Email Address/Contact Information]
