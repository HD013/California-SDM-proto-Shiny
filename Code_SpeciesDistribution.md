
## MAXENT FUTURE ANALYSIS OF RANGE SIZE FOR ABIES CONCOLOR ###
#### David Henderson 2023-2024

<br>
<br>


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
### Setup ###
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
bio_pres <- getData('worldclim', download=F, path="Climate_Data/",
                    var='bio', res=2.5, lon=c(-136,-110), lat=c(30,44))
# Future Climate Data
bio_fut <- getData('CMIP5', download=F, path="Climate_Data/",
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

<br>

#### Prepare the climate data by extracting and droping variables.

<br>

```
#The spatial extent all around CA in Lon/Lat coordinates is roughly:
ca_ext <- c(-130, -110, 30, 44)

# Crop the climate layers to the extent of California
bio_pres <- crop(bio_pres, ca_ext)
bio_fut <- crop(bio_fut, ca_ext)

# Use selected variables
bio_pres <- dropLayer(bio_pres, c("bio3", "bio5", "bio6", "bio7", "bio8",
                                      "bio9", "bio10", "bio11", "bio13", "bio14", "bio15", "bio16", "bio17", "bio18", "bio19"))
bio_fut <- dropLayer(bio_fut, c("bio3", "bio5", "bio6", "bio7", "bio8",
                                    "bio9", "bio10", "bio11", "bio13", "bio14", "bio15", "bio16", "bio17", "bio18", "bio19"))

# Fix temperature
bio_pres[[1]] <- bio_pres[[1]]/10
bio_fut[[1]] <- bio_fut[[1]]/10

# Create training and testing set:
occ_accs=cbind.data.frame(accs$decimalLongitude,accs$decimalLatitude)
set.seed(123)
fold <- kfold(occ_accs, 5)
pres_test <- occ_accs[fold == 1, ]
pres_train <- occ_accs[fold != 1, ]

# inaccurate + accurate records
occ_inaccs=cbind.data.frame(inaccs$decimalLongitude,inaccs$decimalLatitude)
set.seed(123)
fold_icc <- kfold(occ_inaccs, 5)
pres_test_icc <- occ_inaccs[fold_icc == 1, ]
pres_train_icc <- occ_inaccs[fold_icc != 1, ]

# background points 
set.seed(123)
bg <- randomPoints(bio_pres, n=5000, ext=ca_ext, extf = 1.25)
colnames(bg) = c('lon', 'lat')
fold_bg <- kfold(bg, 5)
backg_test <- bg[fold_bg == 1, ]
backg_train <- bg[fold_bg != 1, ]

# Test plot
ext <- extent(-123, -121, 37, 39) # Bay Area Baby!

r <- raster(bio_pres, 1)

plot(!is.na(r), col=c('white', 'light grey'), legend=FALSE)
plot(ext, add=TRUE, col='red', lwd=2)
points(backg_train, pch='-', cex=0.5, col='yellow')
points(backg_test, pch='-',  cex=0.5, col='black')
points(pres_train, pch= '+', col='green')
points(pres_test, pch='+', col='blue')

# Test plot: Total records
plot(!is.na(r), col=c('white', 'light grey'), legend=FALSE)
plot(ext, add=TRUE, col='red', lwd=2)
points(backg_train, pch='-', cex=0.5, col='yellow')
points(backg_test, pch='-',  cex=0.5, col='black')
points(pres_train_icc, pch= '+', col='green')
points(pres_test_icc, pch='+', col='blue')
```

<br>

### Maxent ###

<br>

```
maxent()

## Present
xm <- maxent(bio_pres, pres_train)
xmi <- maxent(bio_pres, pres_train_icc)

# Check the results
# Detailed results
xm@results
#xmi@results

# Test plot
plot(xm)
#plot(xmi)

# Response plot
response(xm)
#response(xmi)

# Evaluate
e_xm <- evaluate(pres_test, backg_test, xm, bio_pres)
e_xmi <- evaluate(pres_test_icc, backg_test, xmi, bio_pres)

# Threshold
tr_xm <- threshold(e_xm, 'spec_sens')
tr_xmi <- threshold(e_xmi, 'spec_sens')

# Predict
px <- predict(bio_pres, xm, ext=ca_ext, progress='')
pxi <- predict(bio_pres, xmi, ext=ca_ext, progress='')


## Future 
pxf <- predict(bio_fut, xm, ext=ca_ext, progress='')
pxfi <- predict(bio_fut, xmi, ext=ca_ext, progress='')

```
<br>
### Estimate Areas of Occurrence ###

<br>

```
# Precise & Present
accs_pres_sp <- rasterToPolygons(px>tr_xm,function(x) x == 1,dissolve=T)
raster::area(accs_pres_sp) / 1000000
# 120353 km2

# Total & Present
inaccs_pres_sp <- rasterToPolygons(pxi>tr_xmi,function(x) x == 1,dissolve=T)
raster::area(inaccs_pres_sp) / 1000000
# 189930.2 km2

# Precise & Future
accs_fut_sp <- rasterToPolygons(pxf>tr_xm,function(x) x == 1,dissolve=T)
raster::area(accs_fut_sp) / 1000000
# 77058.58 km2

# Total & Future
inaccs_fut_sp <- rasterToPolygons(pxfi>tr_xmi,function(x) x == 1,dissolve=T)
raster::area(inaccs_fut_sp) / 1000000
# 114868.1 km2

```

<br>

### Estimate Climate Ranges ###
<br>

```
## Present
accsPresRange <- extract(bio_pres, accs_pres_sp)

inaccsPresRange <- extract(bio_pres, inaccs_pres_sp)

# Temperature
range(accsPresRange[[1]][,1]) 
# -2.1 14.6 degrees Celsius 

range(inaccsPresRange[[1]][,1]) 
# -2.1 15.6 degrees Celsius 

# Precipitation
range(accsPresRange[[1]][,4]) 
# 334 1848 mm

range(inaccsPresRange[[1]][,4]) 
# 283 1848 mm

## Future
accsFutRange <- extract(bio_fut, accs_fut_sp)
inaccsFutRange <- extract(bio_fut, inaccs_fut_sp)


# Temperature
range(accsFutRange[[1]][,1]) 
# 0.2 14.1 degrees Celsius

range(inaccsFutRange[[1]][,1]) 
# 0.2 15.3 degrees Celsius

# Precipitation
range(accsFutRange[[1]][,4]) 
# 310 1720 mm

range(inaccsFutRange[[1]][,4]) 
# 270 1720 mm

```

<br>

### Predict County Occurrences ###

<br>

```
gadmCounty <- paste(ca2$NAME_2)

# Precise & Present
recordsCounty <- paste(accs$county)

accspresCoo <- ca2[which(gadmCounty %in% recordsCounty), ]
accspresCoo <- as(accspresCoo, 'SpatialPolygons')

# Total & Present
recordsCounty_icc <- paste(inaccs$county)

inaccspresCoo <- ca2[which(gadmCounty %in% recordsCounty_icc), ]
inaccspresCoo <- as(inaccspresCoo, 'SpatialPolygons')

# Precise & Future
accsFutint <- raster::intersect(accs_fut_sp, ca2)
recordsCounty_acfut <- paste(accsFutint$NAME_2)

accsFutCoo <- ca2[which(gadmCounty %in% recordsCounty_acfut), ]
accsFutCoo <- as(accsFutCoo, 'SpatialPolygons')

# Total & Future
inaccsFutint <- raster::intersect(inaccs_fut_sp, ca2)
recordsCounty_inacfut <- paste(inaccsFutint$NAME_2)

inaccsFutCoo <- ca2[which(gadmCounty %in% recordsCounty_inacfut), ]
inaccsFutCoo <- as(inaccsFutCoo, 'SpatialPolygons')

```
