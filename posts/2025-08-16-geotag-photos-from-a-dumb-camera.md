---
title: Add locations to photos' EXIF data
description: How to geotag photos taken with a "dumb" camera
date: 2025-08-16
tags:
  - exif
  - photography
  - camera
extraWideMedia: false
opengraph:
  image: /assets/images/geotag-photos/00-by-Stanislav-Kondratiev-pexels.jpg
---

I have an older camera without "smart" features like geotagging. But I do like to organise photos by location so here's how I go about adding that data in retrospect.

## Collecting the geodata with GPSLogger

The excellent, lightweight [GPSLogger](https://gpslogger.app/) app is running on my mobile phone constantly. It records a point every 15 seconds and backs up the day's record to Google Drive as a zipped GPX file at the end of the day.

Therefore we know from these files where the phone was when the photo was taken. Assuming the camera was nearby, all that needs to be done is merge the geodata from the GPX file into a photo's EXIF metadata.

## Enter exiftool

To do that, use [exiftool](https://exiftool.org/) to modify the image's metadata. Assuming a selection of GPX files in '~/gps/':

```bash
exiftool -geotag "~/gps/*20250816*.gpx" *.jpg
```

Note that this will modify the image's metadata directly in the file (caveat: I have seen arguments that this should be avoided in preference of metadata in 'sidecar' files). By default, exiftool creates a copy of the image, adds a suffix _original to the original and modifies the one with the original filename. This is good for checking that the changes made are as expected, and undoing them if not.

With some confidence built, add `-overwrite_original` to skip that copy step.

Looking to 'discard changes' on several files and restore the originals?

```bash
for file in *_original; do mv "$file" "${file%_original}"; done
```

## O, joy of timestamps

*A bane of a data engineer, they come back to haunt me in my free time.*

From what I can gather, different cameras have different conventions for recording timestamps. None of the timestamp fields outright include timezones or timezone offsets.

<aside>The photo organiser I use, <a href="https://github.com/jmathai/elodie">Elodie</a>, renames files using the timestamp without offset. I would prefer to rename them using UTC time so will look to amend that at some point.</aside>

From [EXIF v2.31](https://photo.stackexchange.com/questions/96711/why-dont-exif-tags-contain-time-zone-information#comment224110_97149) it appears that some convention was standardised to include the offset in a separate field, but from my limited sample size this has varying degrees of adoption. The Google Pixel follows the standard but the Fujifilm X100T does not (at least on the firmware I'm running and not in a field that is obvious to me).

Not sure if you've set the timezone correctly on your camera? Good luck! It's a bit of a guess for me to figure out what setting I had (e.g. if I'd changed the time to local time on travels) for photos I took a while ago. For my future self, it's probably simplest to set the camera's time to UTC and not worry about recording what the local time was e.g. during travelling or daylight savings.

### Ensuring a good timestamp merge

The GPS timestamps from GPSLogger are in UTC and the standard for EXIF is to have the GPS timestamp in UTC also. So a basic approach to take on photos without any timezone offset information is to convert EXIF timestamps to the local time and then use that known offset when doing the `-geotag` merge.

This might need a bit of investigation and trial and error i.e. making a change, doing a dry-run geotag, looking up the resolved coordinates, and trying to remember if the photo looks like it was taken where it resolved to.

Show all timestamp info ([source](https://superuser.com/questions/1840199/how-to-fix-timezone-of-images-using-exiftool#comment2908853_1840199)):

```bash
exiftool -time:all -G1 -a -s image.jpg
```

Shift all timestamps forward by an hour:

```bash
exiftool "-AllDates+=1" image.jpg
```

Merge with the UTC offset defined for `-geotime`:

```bash
exiftool -geotag "~/gps/*" "-geotime<${DateTimeOriginal}+HH:MM" *.jpg
```

The [Reddit post](https://www.reddit.com/r/photography/comments/gbks0c/guide_to_using_exiftool_to_correct_the_time/) I found the `-geotime` information on also has suggestions for no interpolation (`-api GeoMaxIntSecs=0`) and using the closest found coordinate within a set time window (`-api GeoMaxExtSecs=120` for 2 minutes) which seem like good options to consider.

This is one of those situations where I think 'good enough' is probably enough. The results may not be 100% precise but if they end up being in the right city/country, that'll do for me.