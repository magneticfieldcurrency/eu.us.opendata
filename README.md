# EU.US.OPENDATA 
## A R Library developed through an EU-US Transatlantic Open Data Partnership
The United States' Department of Commerce and Bureau of Economic Analysis in partnership with the European Commission's DG CONNECT and Eurostat have established a Transatlantic Open Data Partnership focused on economic data. The eu.us.opendata R library is the direct result of this collaborative effort, enabling easy access to comparable datasets from the Eurostat API and BEA API. 

## Installation
As the library is currently available only via Github repository, installation requires a couple additional lines of code:

```{r install} 
#Install packages if needed
install.packages(c('devtools', 'httr'));

library(devtools);
library(httr);

httr::set_config( config( ssl_verifypeer = 0L ));

devtools::install_github('CommerceDataService/eu.us.opendata') 

library(eu.us.opendata)

#Assign your API Key 
myKey <- 'Your 36-digit BEA API key here'

```

## getRel
Using [your BEA API key](http://www.bea.gov/API/signup/index.cfm), set as "myKey", get the data as a relationship table:
```{r getRel}
getRel('gross domestic product', lucky = T, beaKey = myKey)
getRel('gdp', lucky = T, beaKey = myKey)
```

## searchRel
The library supports a free text search for data series. Note that regular expressions are not supported in this version. For a list of all comparable datasets, enter a wild card search ("*")

```{r searchRel}
searchRel('gross domestic product')

```

## describeRel
Using a relationship ID, return a description of that relationship as a table:
```{r describeRel}
describeRel('<JOINT#GDP_A_2>', asHtml = TRUE)
```
 
## listRel
 List the relationships available using a direct SPARQL query of the (online) metadata store. 
```{r listRel}
listRel(asHtml = FALSE)
```

 
## geoMap
 Using the retrieved dataset from getRel(), returns either (1) a harmonized shapefile of EU and US geographies with the dataset joined for a selected year, (2) an interactive web-enabled leaflet map.
```{r geoMap}
## Look at state/NUTS2 level data:
dataset <- getRel('<JOINT#GDP_A_2>', lucky = F, beaKey = myKey)

geoMap(dataset, 2014) ## As leaflet map

geoMap(dataset, 2014, asSHP = TRUE) ## As shapefile

## Look at metro level data:
dataset = getRel('gdp', lucky = T, beaKey = myKey)

geoMap(dataset, 2012) ## As leaflet map

geoMap(dataset, "all") ##As a shapefile with all years in the data

```

##timeSync
Provides user easier access to extracts the latest overlapping year of data or all overlapping years.
```{r timeSync}
timeSync(dataset, 1) #latest
timeSync(dataset, 2) #all overlapping years
```
