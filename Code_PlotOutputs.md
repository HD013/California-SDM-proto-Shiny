
### Create Plot Outputs ###

<br>

```
## Maxent Outputs & Ranges ##
# Maxent Range Precise present
par(mfrow=c(2, 1), mar=c(2, 2, 2.5, 1.5))

# Maxent output
plot(px, 
     xaxt="n",
     yaxt = "n",
     main='Present - Precise records',
     xlim=c(-130,-110), ylim=c(30,44))
plot(ca1, add=TRUE, border='black', lwd=1.1)
axis(side=2, at=c(32,36,40,44),
     labels=c("32°N", "36°N", "40°N", "44°N"))
# Range
plot(usa1, 
     yaxt = "n",
     main = "Range: Present - Precise records",
     xlim=c(-130,-110), ylim=c(30,44), 
     axes=T,col="light yellow", bg="light blue")
plot(accs_pres_sp, add=T, col=alpha("green", 0.9))
axis(side=2, at=c(32,36,40,44),
     labels=c("32°N", "36°N", "40°N", "44°N"))

Mxnt_Range_Accurate_Present <- recordPlot()


# Maxent Range Total Present
par(mfrow=c(2, 1), mar=c(2, 2, 2.5, 1.5))

# Maxent output
plot(pxi, 
     xaxt="n",
     yaxt = "n",
     main='Present - Total records',
     xlim=c(-130,-110), ylim=c(30,44))
plot(ca1, add=TRUE, border='black', lwd=1.1)
axis(side=2, at=c(32,36,40,44),
     labels=c("32°N", "36°N", "40°N", "44°N"))
# Range
plot(usa1, 
     yaxt = "n",
     main = "Range: Present - Total records",
     xlim=c(-130,-110), ylim=c(30,44), 
     axes=T,col="light yellow", bg="light blue")
plot(inaccs_pres_sp, add=T, col=alpha("light green", 0.9))
axis(side=2, at=c(32,36,40,44),
     labels=c("32°N", "36°N", "40°N", "44°N"))

Mxnt_Range_Inccurate_Present <- recordPlot()


# Maxent Range Precise Future
par(mfrow=c(2, 1), mar=c(2, 2, 2.5, 1.5))

# Maxent output
plot(pxf, 
     xaxt="n",
     yaxt = "n",
     main='Future - Precise records',
     xlim=c(-130,-110), ylim=c(30,44))
plot(ca1, add=TRUE, border='black', lwd=1.1)
axis(side=2, at=c(32,36,40,44),
     labels=c("32°N", "36°N", "40°N", "44°N"))
# Range
plot(usa1, 
     yaxt = "n",
     main = "Range: Future - Precise records",
     xlim=c(-130,-110), ylim=c(30,44), 
     axes=T,col="light yellow", bg="light blue")
plot(accs_fut_sp, add=T, col=alpha("red", 0.9))
axis(side=2, at=c(32,36,40,44),
     labels=c("32°N", "36°N", "40°N", "44°N"))

Mxnt_Range_Accurate_Future <- recordPlot()


# Maxent Range Total Future
par(mfrow=c(2, 1), mar=c(2, 2, 2.5, 1.5))

# Maxent output
plot(pxfi, 
     xaxt="n",
     yaxt = "n",
     main='Future - Total records',
     xlim=c(-130,-110), ylim=c(30,44))
plot(ca1, add=TRUE, border='black', lwd=1.1)
axis(side=2, at=c(32,36,40,44),
     labels=c("32°N", "36°N", "40°N", "44°N"))
# Range
plot(usa1, 
     yaxt = "n",
     main = "Range: Future - Total records",
     xlim=c(-130,-110), ylim=c(30,44), 
     axes=T,col="light yellow", bg="light blue")
plot(inaccs_fut_sp, add=T, col=alpha("orange", 0.9))
axis(side=2, at=c(32,36,40,44),
     labels=c("32°N", "36°N", "40°N", "44°N"))

Mxnt_Range_Inccurate_Future <- recordPlot()


## Differences Precision & Time ##

# Diff: Precise Present & Future
par(mfrow=c(2, 2), mar=c(1.5, 1, 1.5, 1))
# Maxent
plot(px, 
     xaxt="n",
     yaxt = "n",
     main='Precise records - Present',
     xlim=c(-130,-110), ylim=c(30,44))
plot(ca1, add=TRUE, border='black', lwd=1.1)
# Future
plot(pxf, legend=F,  
     xaxt="n",
     yaxt = "n",
     main='Precise records - Future',
     xlim=c(-130,-110), ylim=c(30,44))
plot(ca1, add=TRUE, border='black', lwd=1.1)

# Range
plot(usa1,
     xaxt="n",
     yaxt = "n",
     xlim=c(-130,-110), ylim=c(30,44), 
     axes=T,col="light yellow", bg="light blue")
plot(accs_pres_sp, add=T, col=alpha("green", 0.9))
# Future
plot(usa1, 
     xaxt="n",
     yaxt = "n",
     xlim=c(-130,-110), ylim=c(30,44), 
     axes=T,col="light yellow", bg="light blue")
plot(accs_fut_sp, add=T, col=alpha("red", 0.9))

Diff_Accurate_presFut <- recordPlot()

# Diff: Total Present & Future
par(mfrow=c(2, 2), mar=c(1.5, 1, 1.5, 1))
# Maxent
plot(pxi, 
     xaxt="n",
     yaxt = "n",
     main='Total records - Present',
     xlim=c(-130,-110), ylim=c(30,44))
plot(ca1, add=TRUE, border='black', lwd=1.1)
# Future
plot(pxfi, legend=F,  
     xaxt="n",
     yaxt = "n",
     main='Total records - Future',
     xlim=c(-130,-110), ylim=c(30,44))
plot(ca1, add=TRUE, border='black', lwd=1.1)

# Range
plot(usa1,
     xaxt="n",
     yaxt = "n",
     xlim=c(-130,-110), ylim=c(30,44), 
     axes=T,col="light yellow", bg="light blue")
plot(inaccs_pres_sp, add=T, col=alpha("light green", 0.9))
# Future
plot(usa1, 
     xaxt="n",
     yaxt = "n",
     xlim=c(-130,-110), ylim=c(30,44), 
     axes=T,col="light yellow", bg="light blue")
plot(inaccs_fut_sp, add=T, col=alpha("orange", 0.9))

Diff_Inaccurate_presFut <- recordPlot()

# Diff: Present Precise & Total
par(mfrow=c(2, 2), mar=c(1.5, 1, 1.5, 1))
# Maxent
plot(px, 
     xaxt="n",
     yaxt = "n",
     main='Present - Precise records',
     xlim=c(-130,-110), ylim=c(30,44))
plot(ca1, add=TRUE, border='black', lwd=1.1)
# Inaccs
plot(pxi, legend=F,  
     xaxt="n",
     yaxt = "n",
     main='Present - Total records',
     xlim=c(-130,-110), ylim=c(30,44))
plot(ca1, add=TRUE, border='black', lwd=1.1)

# Range
plot(usa1,
     xaxt="n",
     yaxt = "n",
     xlim=c(-130,-110), ylim=c(30,44), 
     axes=T,col="light yellow", bg="light blue")
plot(accs_pres_sp, add=T, col=alpha("green", 0.9))
# Inaccs
plot(usa1, 
     xaxt="n",
     yaxt = "n",
     xlim=c(-130,-110), ylim=c(30,44), 
     axes=T,col="light yellow", bg="light blue")
plot(inaccs_pres_sp, add=T, col=alpha("light green", 0.9))

Diff_In_Accurate_Present <- recordPlot()

# Diff: Future Precise & Total
par(mfrow=c(2, 2), mar=c(1.5, 1, 1.5, 1.5))
# Maxent
plot(pxf, 
     xaxt="n",
     yaxt = "n",
     main='Future - Precise records',
     xlim=c(-130,-110), ylim=c(30,44))
plot(ca1, add=TRUE, border='black', lwd=1.1)
# Inaccs
plot(pxfi, legend=F,  
     xaxt="n",
     yaxt = "n",
     main='Future - Total records',
     xlim=c(-130,-110), ylim=c(30,44))
plot(ca1, add=TRUE, border='black', lwd=1.1)

# Range
plot(usa1,
     xaxt="n",
     yaxt = "n",
     xlim=c(-130,-110), ylim=c(30,44), 
     axes=T,col="light yellow", bg="light blue")
plot(accs_fut_sp, add=T, col=alpha("red", 0.9))
# Inaccs
plot(usa1, 
     xaxt="n",
     yaxt = "n",
     xlim=c(-130,-110), ylim=c(30,44), 
     axes=T,col="light yellow", bg="light blue")
plot(inaccs_fut_sp, add=T, col=alpha("orange", 0.9))

Diff_In_Accurate_Future <- recordPlot()


## Full Diff Plots ##

# fullDiff_Maxent
par(mfrow=c(2, 2), mar=c(1.5, 1, 1.5, 1))
# Accs
plot(px, 
     xaxt="n",
     yaxt = "n",
     main='Present-Precise Records',
     ylab='Precise',
     xlim=c(-130,-110), ylim=c(30,44))
plot(ca1, add=TRUE, border='black', lwd=1.1)
# Future
plot(pxf, legend=F, 
     xaxt="n",
     yaxt = "n",
     main= 'Future-Precise Records',
     xlim=c(-130,-110), ylim=c(30,44))
plot(ca1, add=TRUE, border='black', lwd=1.1)
# Inaccs
plot(pxi, 
     xaxt="n",
     yaxt = "n",
     main='Present-Total Records',
     xlim=c(-130,-110), ylim=c(30,44))
plot(ca1, add=TRUE, border='black', lwd=1.1)
# Future
plot(pxfi, legend=F, 
     xaxt="n",
     yaxt = "n",
     main='Future-Total Records',
     xlim=c(-130,-110), ylim=c(30,44))
plot(ca1, add=TRUE, border='black', lwd=1.1)

fullDiff_Maxent <- recordPlot()


# fullDiff_Range
par(mfrow=c(2, 2), mar=c(1.5, 1, 1.5, 1.5))

# Accs
plot(usa1,
     xaxt="n",
     yaxt = "n",
     main = "Present-Precise Records",
     xlim=c(-130,-110), ylim=c(30,44), 
     axes=T,col="light yellow", bg="light blue")
plot(accs_pres_sp, add=T, col=alpha("green", 0.9))
# Future
plot(usa1, 
     xaxt="n",
     yaxt = "n",
     main = "Future-Precise Records",
     xlim=c(-130,-110), ylim=c(30,44), 
     axes=T,col="light yellow", bg="light blue")
plot(accs_fut_sp, add=T, col=alpha("red", 0.9))

# Inaccs
plot(usa1,
     xaxt="n",
     yaxt = "n",
     main = "Present-Total Records",
     xlim=c(-130,-110), ylim=c(30,44), 
     axes=T,col="light yellow", bg="light blue")
plot(inaccs_pres_sp, add=T, col=alpha("light green", 0.9))
# Future
plot(usa1, 
     xaxt="n",
     yaxt = "n",
     main = "Future-Total Records",
     xlim=c(-130,-110), ylim=c(30,44), 
     axes=T,col="light yellow", bg="light blue")
plot(inaccs_fut_sp, add=T, col=alpha("orange", 0.9))

fullDiff_Range <- recordPlot()


#fullDiff_Climate
# plot these w/ present (precise) data b/c the trees aren't going anywhere soon
par(mfrow=c(2, 2), mar=c(1.5, 1, 1.5, 1.5))

# Temperature for PRESENT
plot(bio_pres, 1,
     xaxt="n",
     yaxt = "n",
     main='Present Temperature',
     xlim=c(-130,-110), ylim=c(30,44),
     axes=T, bg="light blue")
plot(ca1, add=TRUE, border='black', lwd=1.1)
points(occ_accs, pch='+', col='dark red')
# Temperature for FUTURE
plot(bio_fut, 1, legend=F,
     xaxt="n",
     yaxt = "n",
     main='Future Temperature',
     xlim=c(-130,-110), ylim=c(30,44),
     axes=T, bg="light blue")
plot(ca1, add=TRUE, border='black', lwd=1.1)
points(occ_accs, pch='+', col='dark blue')

# Precip for 
plot(bio_pres, 1, 
     xaxt="n",
     yaxt = "n",
     main='Present Precipitation',
     xlim=c(-130,-110), ylim=c(30,44),
     axes=T, bg="light blue")
plot(ca1, add=TRUE, border='black', lwd=1.1)
points(occ_accs, pch='+', col='red')
# Precip for FUTURE
plot(bio_fut, 1, legend=F,
     xaxt="n",
     yaxt = "n",
     main='Future Precipitation',
     xlim=c(-130,-110), ylim=c(30,44),
     axes=T, bg="light blue")
plot(ca1, add=TRUE, border='black', lwd=1.1)
points(occ_accs, pch='+', col='blue')

fullDiff_Climate <- recordPlot()


# fullDiff_County
par(mfrow=c(2, 2), mar=c(1.5, 1, 1.5, 1.5))

# CountyMap for ACCURATE & PRESENT
plot(ca1, main = "Present-Precise Records")
plot(ca1, add=TRUE, border='gray', col='gray80', lwd=1)
plot(accspresCoo, col=alpha('green', 0.1), add=TRUE)
plot(accspresCoo, col=alpha('green', 0.8), add=TRUE)

# CountyMap for ACCURATE & FUTURE
plot(ca1, main = "Future-Precise Records")
plot(ca1, add=TRUE, border='gray', col='gray80', lwd=1)
plot(accsFutCoo, col=alpha('red', 0.1), add=TRUE)
plot(accsFutCoo, col=alpha('red', 0.8), add=TRUE)

# CountyMap for INACCURATE & PRESENT
plot(ca1, main = "Present-Total Records")
plot(ca1, add=TRUE, border='gray', col='gray80', lwd=1)
plot(inaccspresCoo, col=alpha('light green', 0.1), add=TRUE)
plot(inaccspresCoo, col=alpha('light green', 0.8), add=TRUE)

# CountyMap for INACCURATE & FUTURE
plot(ca1, main = "Future-Total Records")
plot(ca1, add=TRUE, border='gray', col='gray80', lwd=1)
plot(inaccsFutCoo, col=alpha('orange', 0.1), add=TRUE)
plot(inaccsFutCoo, col=alpha('orange', 0.8), add=TRUE)

fullDiff_County <- recordPlot()
```

<br>

### Save Output List ###

<br>

```
outputList2 <- list(Mxnt_Range_Accurate_Present, Mxnt_Range_Inccurate_Present,
                    Mxnt_Range_Accurate_Future, Mxnt_Range_Inccurate_Future, 
                    Diff_Accurate_presFut, Diff_Inaccurate_presFut, 
                    Diff_In_Accurate_Present, Diff_In_Accurate_Future,
                    fullDiff_Maxent, fullDiff_Range, fullDiff_Climate, 
                    fullDiff_County)
show(outputList2[1])

# Save outputList
#saveRDS(outputList2, file=paste0(path.to.files,
 #                                "Shiny/pre_outputList2.RData"))
readRDS("Shiny/pre_outputList2.RData")


```

<br>

<br>
