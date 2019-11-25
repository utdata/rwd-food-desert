# Food desert data

This is a project to prepare Food Access data to join with spatial files to put in Tableau. We're basically trying to make a version of this map, you can [build here](https://www.ers.usda.gov/data-products/food-access-research-atlas/go-to-the-atlas/).

![food-atlas](img/food-atlas-map.png)

Unfortunatly, in the end Tableau isn't as cool as the atlas above (which I think is ArcGIS online.)


## Getting the data

Data file _Food Access Research Atlas Data Download 2015_ downloaded from [United States Department of Agriculture](https://www.ers.usda.gov/data-products/food-access-research-atlas/download-the-data/). It is downloaded as `data-raw/DataDownload2015.xlsx`. This has three tabs. I saved the tab **Variables Lookup** as it's own file `data-dictionary.xlsx` for reference in a smaller file. There is  [documentation](https://www.ers.usda.gov/data-products/food-access-research-atlas/documentation/) for the food access varialbles.

## 01-import

[01-import](https://utdata.github.io/rwd-food-desert/01-import.html) imports the file from Excel, selects the columns we need and filters it to Travis County, Texas. There are 2018 rows.

## 02-data-wrangle

[02-data-wrangle](https://utdata.github.io/rwd-food-desert/02-data-wrangle.html) does some data checking and creates columns that can more easily show the food access level of each census tract.

Exports from this notebook will be used to join datain QGIS.

## 03-mapping

[03-mapping](https://utdata.github.io/rwd-food-desert/03-mapping.html) maps the "Limited Access" and "Limited Income and Limited Access" columns using ggplot and tigris.

## QGIS & Tableau

The `qgis` folder has a project `food_access.qgz` that has Travis Census tracts and joins with the data wrangled from 02-data-wrangle as `data-processed/tfa.csv`.

- `tl_2019_48_tracts/travis_tract.gpkg` is the travis shapefile, filtered from the original, also there.
- That was opened and joined with `data-processed/tfa.csv`.
- The result was exported as `travis_food_access` shapefile called `tfa.shp` (and companion files). A compressed zip is also available.

In the `tableau` folder is a workbook `FoodDesert.twb` that imports the `tfa.shp` file as a Spatial file and then creates the two interactive maps. I didn't finish or publish that because it is the answer to a class assignment for a couple of students.

[This Tableau help file](https://help.tableau.com/current/pro/desktop/en-us/maps_shapefiles.htm) was useful in dealing with the shapefiles. Of importance: add the Geoid to detial to split the individual tracts.

## csvkit

The folder [csvkit](csvkit) has some earlier work to convert the Excel file to csv. It's moot at this point, but I'm keeping it as a learning experience.

## Workbench & Google Sheets

I started working on this project in R because I thought I would have to do a complicated reshape and filter to build the food access columns for "Limited Access" and "Limited Income Limited Access", but once I figured out the logic of the columns I found I only needed two of them for our county, so a nested IF would suffice.

I tried later to go back to [Workbench](https://app.workbenchdata.com/workflows/40486/) (the platform where I started this project) to do the IF, but I found their Excel formulas do not support the `AND` function, at least not within and `IF`.

I was able to build the columns in a Google sheet [Food access formula](https://docs.google.com/spreadsheets/d/13QZmczHLAL3_DIIQ6h7Tjgzty-GSPh1OtAr94K7wiUQ/edit#gid=259790322). That file was started from the end of the "Import" sheet in the Workbench workbook. It wasn't used ... I just wanted to figure out the formula.

