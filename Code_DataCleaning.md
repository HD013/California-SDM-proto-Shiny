
## Here we go through an abrriviated number of steps to be sure that our data (including and especially the less precise records) are useable in the following analyses. 

<br>

```
# DATA CLEANING

path.to.files <-

records <- read.csv('Aibes_concolor_occ.CSV', 
                           header = TRUE, sep = ",")

# Subset for records in California
records <- records[which(records$stateProvince == "California"), ]

# Change missing data to NA's
records$species[records$species == ''] <- NA
records$country[records$country == ''] <- NA
records$stateProvince[records$stateProvince == ''] <- NA
records$county[records$county == ''] <- NA


# Check for suspect records and remove 
bads <- which(is.na(records$specificEpithet) | records$specificEpithet == '')

# flag records based on observations 
unique(records$basisOfRecord)
# "HUMAN_OBSERVATION"  "PRESERVED_SPECIMEN" "LIVING_SPECIMEN"    "MATERIAL_SAMPLE"    "FOSSIL_SPECIMEN"  

which(records$basisOfRecord %in% 'HUMAN_OBSERVATION')
# 1707 entries

records <-  records[-which(records$basisOfRecord %in% 'HUMAN_OBSERVATION'),]

### flag cultivated records ###
# Habitat
unique(records$habitat)
habitat <- tolower(records$habitat)

# Locality
unique(records$locality)
locality <- tolower(records$locality)

unique(records$verbatimLocality)
verbatimLocality <- tolower(records$verbatimLocality)


flags <- c('cultivat', 'grow', 'garden', 'experiment', 'captiv', 'greenhouse', 'arboretum', 'hothouse', 'residential', 'nursery', 'jardin')
grownIndex <- integer()
for (flag in flags) {
  grownIndex <- c(grownIndex, which(grepl(flag, x=habitat)))
  grownIndex <- c(grownIndex, which(grepl(flag, x=locality)))
  grownIndex <- c(grownIndex, which(grepl(flag, x=verbatimLocality)))
  }
grownIndex <- sort(unique(grownIndex))

grownRecords <- records[grownIndex, c('gbifID', 'habitat', 'locality', 'verbatimLocality')]
rownames(grownRecords) <- grownIndex

# Read notes manually
grownRecords[2]

records[which(records$gbifID == 3059697969
), ]

# Observation to remove
records <- records[-1841,]

# clean county names ###
# removes extraneous text
# # x is a character vector
removeExtraText <- function(x) {

 bads <- c('(', ')', ' county', ' County', ' COUNTY', 'County of', 'Par.', 'Parish', 'Cty.', 'Cty', 'Co.', 'Municipio', 'Municipality', '[', ']', 'Co ')

 for (bad in bads) x <- gsub(x, pattern=bad, replacement='', fixed=TRUE)

 saints <- c('Ste. ', 'Ste ', 'St. ', 'St ')
 for (saint in saints) x <- gsub(x, pattern=saint, replacement='Saint ', fixed=TRUE)

 x <- raster::trim(x)
 x[x == ''] <- NA
 x
 }

records$county <- removeExtraText(records$county)

# what state/province names don't match with GADM?
gadmCombined <- paste(ca2@data$NAME_1, ca2@data$NAME_2)
gadmCombined <- gadmCombined[!duplicated(gadmCombined)]
gadmCombined <- sort(gadmCombined)
gadmCombined <- tolower(gadmCombined)

recordsCombined <- data.frame(
stateProvince = records$stateProvince,
county = records$county
)

recordsCombined$combined <- paste(recordsCombined$stateProvince, recordsCombined$county)
recordsCombined$combined <- tolower(recordsCombined$combined)

recordsCombined <- recordsCombined[!duplicated(recordsCombined$combined), ]
recordsCombined <- recordsCombined[order(recordsCombined$combined), ]

co_bads <- data.frame()
for (i in 1:nrow(recordsCombined)) {
 if (!(recordsCombined$combined[i] %in% gadmCombined)) co_bads <- rbind(co_bads, recordsCombined[i, ])
 }

records <- records[-which(records$county %in% co_bads$county), ]

# RECORDS CLEAN - SPLIT into Precise, Imprecise Records

# minimum coordinate uncertainty in meters to be considered an "precise" record
minCoordUncerOrPrecision_m <- 5000

# Accurate/ precisce
accs <- records[which(records$coordinateUncertaintyInMeters < 5000), ]

accsSp <- SpatialPointsDataFrame(accs[ , llGbif], data=accs)
accsSpEa <- sp::spTransform(accsSp, getCRS('albersNA', TRUE))
ext_acc <- extent(accsSp)

# Test plot: accurate records
# Variables
extSpEa <- as(ext_acc, 'SpatialPolygons')
projection(extSpEa) <- getCRS('wgs84')
extSpEa <- gBuffer(extSpEa, width=50000)

thisca1 <- crop(ca1, extent(extSpEa))
thisca2 <- crop(ca2, extent(extSpEa))

# Plot
#par(oma=c(0.5, 0.2, 2, 0.2))

plot(extSpEa, border=NA)
plot(thisca2, add=TRUE, border='gray')
plot(thisca1, add=TRUE, border='gray', lwd=2)


# Inaccurate/ Imprecisce
inaccs <- records[-which(records$coordinateUncertaintyInMeters < 5000), ]

# County matching
recordsCounty <- paste(records$county)
gadmCounty <- paste(ca2$NAME_2)
index <- which(gadmCounty %in% recordsCounty)

countiesSpEa <- ca2[index, ]
expExt_co <- extent(countiesSpEa)

# Clean Duplicate Records 
cleanDuplicateRecord <- data.frame(cleanDuplicateRecord = rep(FALSE, nrow(records)))
at <- which.max(grepl(names(records), pattern='clean'))
records_c <- insertCol(cleanDuplicateRecord, into=records, at=at, before=FALSE)

match(colnames(records_c), colnames(records))


# remove records with duplicated coordinates (to a given error) and same coordinate uncertainty (to a given level)

# number of decimal digits to which coordinates of inaccurate records must be the same to be considered being at the same location
proximateDigits <- 3

dups <- records[which(duplicated(records$decimalLongitude, proximateDigits) & 
        duplicated(records$decimalLatitude, proximateDigits)), ]

records_c <- records[-which(duplicated(records$decimalLongitude, proximateDigits) & 
                             duplicated(records$decimalLatitude, proximateDigits)), ]

# Save the clean records

#write.csv(x=records_c, file="C:/Users/marvi/OneDrive/Documents/DataAnalysis/AsclepiasCode/Aibes_concolor_occ_clean_20231229.csv")



```

<br>

<br>

The data can always be cleaned until there is no more. 

The steps shown here are a quick data cleaning and arrangement to test out the Species Distribution Model shiny app.

<br>

Check out a more complete version of the methods [here](https://onlinelibrary.wiley.com/doi/abs/10.1111/geb.13628)
