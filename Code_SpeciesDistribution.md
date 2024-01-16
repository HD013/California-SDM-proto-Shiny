
## MAXENT FUTURE ANALYSIS OF RANGE SIZE FOR ABIES CONCOLOR ###
#### David Henderson 2023-2024


### CONTENTS ###
#### Setup ###
#### Maxent SDM for precise & total, present & future ###
#### Estimate Areas of Occurrence ###
#### Estimate Climate Ranges ###
#### Predict County Occurrences ###
#### Create Plot Outputs ###
#### Save as List and Export for Shiny ###


### References 
 [Original Hijmans playbook](https://cran.r-hub.io/web/packages/dismo/vignettes/sdm.pdf)

 EdsmX by [Adamlillith](https://github.com/adamlilith/enmSdmX)


<br>
### setup ###
<br>


```
rm(list=objects())

# Load Libraries
library(sf)
library(rgdal)
library(ggplot2)
library(raster)
library(dismo)

path.to.files <-

# Load maps:
# State Level
usa1 <- readOGR(paste0(path.to.files, "gadm41_USA_shp/gadm41_USA_1.shp"))
ca1 <- usa1[which(usa1$NAME_1 == "California"), ]

# County Level 
usa2 <- readOGR(paste0(path.to.files, "gadm41_USA_shp/gadm41_USA_2.shp"))
ca2 <- usa2[which(usa2$NAME_1 == "California"), ]

# Present Climate Data
bio_pres <- getData('worldclim', download=F, path="C:/Users/marvi/OneDrive/Documents/DataAnalysis/AsclepiasCode/Climate_Data/",
                    var='bio', res=2.5, lon=c(-136,-110), lat=c(30,44))
# Future Climate Data
bio_fut <- getData('CMIP5', download=F, path="C:/Users/marvi/OneDrive/Documents/DataAnalysis/AsclepiasCode/Climate_Data/",
                   var='bio', res=2.5, lon=c(-114,-126), lat=c(32,42),
                   rcp=45, model='NO', year=50)
names(bio_pres)
names(bio_fut) <- names(bio_pres)

# Load occurence Data
records <- read.csv('Aibes_concolor_occ_clean_20231229.CSV', 
                    header = TRUE, sep = ",")

# Precise records
accs <- records[which(records$coordinateUncertaintyInMeters < 5000), ]
# Total ('inaccurate' + accurate) records
inaccs <- records


```
