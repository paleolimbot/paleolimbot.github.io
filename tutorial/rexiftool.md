---
title: EXIF in R
layout: page
---

# Getting EXIF data in R

Recently I was tasked with organizing a large number of geotagged images extracted from several years of field data. The photos came from a GPS with a camera, but because there were tons of duplicate files, any GPS waypoints they were associated with were lost. Enter EXIF data, the format in which date/time, GPS, resolution, camera make/model, and a number of other fields are stored within image files. There is no package available for this, however [exiftool](http://www.sno.phy.queensu.ca/~phil/exiftool/), written by Phil Harvey, is a multi-platform command-line interface that extracts this data and outputs in a number of formats. Using the `system()` command in R, we can write a simple wrapper around the exiftool command that produces a nice `data.frame` with all the information about our image files.

First thing is first, you're going to need to install exiftool. It's available for Windows, Mac, and Unix-oid systems (although it's a little more complicated to [install on the unix-oid ones](http://www.sno.phy.queensu.ca/~phil/exiftool/install.html#Unix)). In Windows you'll end up with an `exiftool.exe` file that you should put in your RStudio Project directory (or working directory, if you don't use RStudio). If you can type `system("exiftool")` into your R console and not get any text saying "command not found", you're good to go.

The next thing you'll need is a photo with some EXIF data. Any photo taken by a digital camera has at least *some* kind of EXIF data, so this shouldn't be hard to find. Once you have one in your RStudio project (or working directory), try the following:

```R
system("exiftool my_file.jpg")
```

You should get something like this:

```
======== ./Garmin_USA.jpg
ExifTool Version Number         : 10.07
File Name                       : Garmin_USA.jpg
Directory                       : .
File Size                       : 49 kB
File Modification Date/Time     : 2015:11:21 14:16:21-04:00
File Access Date/Time           : 2015:12:13 14:02:36-04:00
File Inode Change Date/Time     : 2015:11:21 14:16:21-04:00
File Permissions                : rw-r--r--
File Type                       : JPEG
File Type Extension             : jpg
MIME Type                       : image/jpeg
JFIF Version                    : 1.01
Exif Byte Order                 : Little-endian (Intel, II)
Modify Date                     : 1990:01:01 08:00:00
GPS Version ID                  : 2.2.0.0
GPS Map Datum                   : WGS-84
GPS Latitude Ref                : North
GPS Longitude Ref               : West
GPS Altitude Ref                : Above Sea Level
Compression                     : Uncompressed
Photometric Interpretation      : RGB
Strip Offsets                   : 425
Samples Per Pixel               : 3
Rows Per Strip                  : 84
Strip Byte Counts               : 28224
X Resolution                    : 72
Y Resolution                    : 72
Resolution Unit                 : inches
Image Width                     : 475
Image Height                    : 163
Encoding Process                : Baseline DCT, Huffman coding
Bits Per Sample                 : 8
Color Components                : 3
Y Cb Cr Sub Sampling            : YCbCr4:2:0 (2 2)
GPS Altitude                    : 365 m Above Sea Level
GPS Latitude                    : 38 deg 51' 20.15" N
GPS Longitude                   : 94 deg 47' 56.41" W
GPS Position                    : 38 deg 51' 20.15" N, 94 deg 47' 56.41" W
Image Size                      : 475x163
Megapixels                      : 0.077
```

As you can see, all the information we need is here, but it's not in a format that is particularly conducive to parsing in R. Also, things like "GPS Latitude" are in a pretty unitelligible format (we'll probably want something like `-94.526` instead of `94 deg 47' 56.41" W` if we're going to do any processing in R). Luckily, the genious behind exiftool figured this out already...all you have to do is pass the `-n` parameter. Pass the `-csv` parameter and you've got the output in nice parsing form, ready for R to convert to a `data.frame`.

```R
system("exiftool -n -csv my_file.jpg")
```

Gives us:

```
SourceFile,APP14Flags0,APP14Flags1,BitsPerSample,ColorComponents,ColorTransform,Compression,DCTEncodeVersion,Directory,EncodingProcess,ExifByteOrder,ExifToolVersion,FileAccessDate,FileInodeChangeDate,FileModifyDate,FileName,FilePermissions,FileSize,FileType,FileTypeExtension,GPSAltitude,GPSAltitudeRef,GPSLatitude,GPSLatitudeRef,GPSLongitude,GPSLongitudeRef,GPSMapDatum,GPSPosition,GPSVersionID,ImageHeight,ImageSize,ImageWidth,JFIFVersion,Megapixels,MIMEType,ModifyDate,PhotometricInterpretation,Quality,ResolutionUnit,RowsPerStrip,SamplesPerPixel,StripByteCounts,StripOffsets,XResolution,YCbCrSubSampling,YResolution
./Garmin_Asia.jpg,,,8,3,,1,,.,0,II,10.07,2015:12:13 14:45:01-04:00,2015:11:21 14:16:21-04:00,2015:11:21 14:16:21-04:00,Garmin_Asia.jpg,644,84254,JPEG,JPG,319.9804698,0,25.0617996231694,N,121.640300536606,E,WGS-84,25.0617996231694 121.640300536606,2 2 0 0,409,800x409,800,1 1,0.3272,image/jpeg,1990:01:01 08:00:00,2,,2,84,3,28224,425,72,2 2,72
```

With that, we can use `read.csv()` to process the output. Since our output is not a file, we'll have wrap our string with `textConnection()` to make it accessible to `read.csv()`. With two lines of code, we can get a `data.frame` out of our EXIF data.

```R
output <- system("exiftool -n -csv my_file.jpg", intern=TRUE)
df <- read.csv(textConnection(output), stringsAsFactors = FALSE)
```

Note that we have to pass `intern=TRUE` to `system()` in order to obtain the string produced by `exiftool` (otherwise `system()` returns `0`). Passing `stringsAsFactors=FALSE` isn't necessary but you will get odd behaviour if all of your filenames are treated as factors and not strings.

Of course, this is much more useful distilled into a function:

```R
get.exif <- function(filename) {
  command <- paste("exiftool -n -csv", 
  				paste(shQuote(filename), collapse=" "))
  read.csv(textConnection(system(command, intern=TRUE)), 
  			stringsAsFactors = FALSE)
}
```

In case you're wondering, the `paste(shQuote(filename), collapse=" ")` allows you to pass a character vector as the `filename` argument (e.g. from `list.files()`), that will be separated by `" "` into a single string (the command would then look like `exiftool -n -csv file1.jpg file2.jpg`). Using `shQuotes()` lets there be spaces in the filenames (producing a command like this: `exiftool -n -csv 'file 1.jpg' 'file 2.jpg'`.

How might this get used in real life? As I alluded to earlier, my most recent project involved organizing a number of pictures by date/time using code something like this:

```R
library(lubridate)

#define exif function
get.exif <- function(filename) {
  command <- paste("exiftool -n -csv", 
  				paste(shQuote(filename), collapse=" "))
  read.csv(textConnection(system(command, intern=TRUE)), 
  			stringsAsFactors = FALSE)
}

#load exif data from my_directory
exifdata <- get.exif(list.files(path="my_directory"))

#set output directory
outdir <- "my_new_directory"

for(i in 1:nrow(exifdata)) {
  row <- exifdata[i, ]
  d <- ymd_hms(row$DateTimeOriginal)
  ext <- tools::file_ext(row$SourceFile) #maintain file extension
  newname <- file.path(outdir, 
  				sprintf("%04d-%02d-%02d %02d.%02d.%02d.%s",
                       year(d), month(d), day(d), hour(d), minute(d),
                       second(d), ext))
  file.copy(row$SourceFile, newname)
}
```

There's some pretty heavy usage of `sprintf()` and the [lubridate](https://cran.r-project.org/package=lubridate) package (`ymd_hms()`, `year()`, `month()`, `day()` etc.) that is a topic for another day, but you get the jist of it: working with EXIF data in R isn't too bad.