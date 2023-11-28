# README.md for AstroBinUploader.py  

# Table of Contents

- [Features](#features)
- [Prerequisites](#prerequisites)
- [Installation of Python script](#installation-of-python-script)
- [External Files](#external-files)
- [Running the Script](#running-the-script)
- [Additional Information](#additional-information)
- [AstroBin's Acquisition CSV File Format](#astrobins-acquisition-csv-file-format)
- [Example `acquisition.csv` file](#example-acquisitioncsv-file)
- [filter.csv file](#filtercsv-file)
- [Finding the AstroBin Numeric ID for Filters](#finding-the-astrobin-numeric-id-for-filters)
- [Creating the filters.csv File](#creating-the-filterscsv-file)
- [sites.csv file](#sitescsv-file)
- [secret.csv and Accessing Sky Quality Data](#secretcsv-and-accessing-sky-quality-data)
- [FWHM values](#fwhm-values)
- [default.csv file](#defaultcsv-file)
- [Contributing to AstroImage Processing Script](#contributing-to-astroimage-processing-script)
- [Contact](#contact)
- [Licence](#licence)



## Features

This Python script is designed to create an acquisition.csv file suitable for upload to AstroBin's import CSV dialogue.  
The acquisition.csv file is obtained by extracting FITS (Flexible Image Transport System) or XISF (Extensible Image Serialization Format) headers of the image files associated with a given astronomical target.
With the correct default.csv entries, outputs from most image generation packages can be accommodated.

Key features include:

- **Extraction of FITS/XISF Headers**: Extracts headers for all FITS/XISF files in specified directories. Directories can have a mix of FITS and XISF files.
- **Calibration Data Summarization**: Summarizes calibration sessions by processing FLAT, BIAS, DARK, and FLATDARK frames.
- **LIGHT Data Processing**: Aggregates LIGHT frames for the identified target.
- **Sky Quality Analysis**: Calculates SQM and Bortle scale classification based on the observation location coordinates.
- **Auxiliary Parameter Calculation**: Calculates additional parameters like Image Scale (IMSCALE), and Full-width Half Maximum (FWHM) from measured/estimated HFR values for each image.
- **AstroBin Compatibility**: Formats aggregated data for upload to AstroBin's import CSV file dialogue.

## Prerequisites

Before using this script, ensure you have the following installed:

- Python 3.x (see the provided link for installation instructions).
- Pandas library.
- Astropy library.
- Requests library.

Instructions on how to install Python can be found here [Python Installation Instructions](https://python.land/installing-python).   
With Python installed, the required libraries can be installed from the command line by typing: 

`pip install pandas astropy requests`

## Installation of Python script

To install this script, follow these steps:

1. Clone or download the repository from my [GitHub repository](https://github.com/SteveGreaves/AstroBinUploader).
2. Ensure you have all the required libraries installed. If not, use `pip install pandas astropy requests`.
3. Place the script in a directory, naming it as you want.

## External Files

The script requires the following external CSV files for its operation:

- **filter.csv**: Contains mappings of filter names to their codes.
- **sites.csv**: Stores latitude, longitude, Bortle scale, and SQM values for known locations.
- **secret.csv**: Holds API keys and endpoints for external services (e.g., light pollution data).
- **defaults.csv**: Holds default values for missing FITS header file elements and HFR.

Ensure these files are present in the same directory as the script. You can copy those in the repository and modify as required.

### Running the Script

Run the script from the installation directory as follows:


        `python3 AstroBinUpload.py directory_path1 directory_path2 ...  directory_pathx`.

There can be as many directory paths as required to capture all the image files, but the minimum is one. The image files can be collected in one or more directories. If items are missing from the FITS/XISF headers they are substituted with values taken from the 'defaults.csv' file. Edit the 'defaults.csv' file so that it reflects your equipment set-up. HFR is taken from the 'defaults.csv' file and used if there is no HFR value found in the image file path. The aim of this approach is to make the code as agnostic as possible to the program that generates the image file header.  

An example program call is given below:

        'python3 AstroBinUpload.py "/mnt/HDD_8TB/Preselected/Rosette Nebula (Caldwell 49)" "/mnt/HDD_8TB/Preselected/Calibration data/30th April 2023" "/mnt/HDD_8TB/Preselected/Calibration data/20th April 2023/DARK" '

This generates the following command line output:

        Reading FITS headers...
        Extracting headers from /mnt/HDD_8TB/Preselected/Rosette Nebula (Caldwell 49)
        Extracting headers from OIII
        Extracting headers from Blue
        Extracting headers from Ha
        Extracting headers from SII
        Extracting headers from Red
        Extracting headers from Green
        Extracting headers from /mnt/HDD_8TB/Preselected/Calibration data/30th April 2023
        Extracting headers from BIAS
        Extracting headers from FLAT
        Extracting headers from /mnt/HDD_8TB/Preselected/Calibration data/20th April 2023/DARK
        Header extraction complete

        Observation session Summary:

        LIGHTS:
        Filter Ha: 63 frames, Exposure time: 10.0 hrs 30.0 mins 0 secs
        Filter OIII: 56 frames, Exposure time: 9.0 hrs 20.0 mins 0 secs
        Filter SII: 75 frames, Exposure time: 12.0 hrs 30.0 mins 0 secs

        Total session exposure for LIGHTs: 32.0 hrs 20.0 mins 0 secs

        FLATS:
        Filter Blue: 50 frames, Exposure time: 9 secs
        Filter Green: 50 frames, Exposure time: 8 secs
        Filter Ha: 50 frames, Exposure time: 23 secs
        Filter Lum: 50 frames, Exposure time: 2 secs
        Filter OIII: 50 frames, Exposure time: 25 secs
        Filter Red: 50 frames, Exposure time: 6 secs
        Filter SII: 50 frames, Exposure time: 28 secs

        BIAS with GAIN 0: 100 frames, Exposure time: 0 secs
        BIAS with GAIN 100: 100 frames, Exposure time: 0 secs
        DARK with GAIN 0: 50 frames, Exposure time: 50.0 mins 0 secs
        DARK with GAIN 100: 50 frames, Exposure time: 8.0 hrs 20.0 mins 0 secs

        Used existing Bortle of 4.0 and SQM of 20.52 mag/arc-seconds^2 for lat 52.25, lon -0.12 from sites.csv.

        Completed sky quality extraction

        Data exported to Rosette Nebula (Caldwell 49) acquisition.csv

The contents of Rosette Nebula (Caldwell 49) acquisition.csv are given below:

| date       | filter | number | duration | binning | gain | sensorCooling | fNumber | darks | flats | flatDarks | bias | bortle | meanSqm | meanFwhm | temperature |
|------------|--------|--------|----------|---------|------|---------------|---------|-------|-------|-----------|------|--------|---------|----------|-------------|
| 2022-01-12 | 4663   | 15     | 600      | 1       | 100  | -10           | 5.4     | 50    | 50    | 0         | 100  | 4      | 20.52   |    2.3   | 0.62        |
| 2022-01-13 | 4663   | 34     | 600      | 1       | 100  | -10           | 5.4     | 50    | 50    | 0         | 100  | 4      | 20.52   |    2.3   | -0.26       |
| 2022-01-13 | 4844   | 4      | 600      | 1       | 100  | -10           | 5.4     | 50    | 50    | 0         | 100  | 4      | 20.52   |    2.3   | -0.38       |
| 2022-01-14 | 4663   | 14     | 600      | 1       | 100  | -10           | 5.4     | 50    | 50    | 0         | 100  | 4      | 20.52   |    2.3   | -1.04       |
| 2022-01-14 | 4844   | 23     | 600      | 1       | 100  | -10           | 5.4     | 50    | 50    | 0         | 100  | 4      | 20.52   |    2.3   | -1.7        |
| 2022-01-15 | 4844   | 4      | 600      | 1       | 100  | -10           | 5.4     | 50    | 50    | 0         | 100  | 4      | 20.52   |    2.3   | -2          |
| 2022-01-18 | 4844   | 1      | 600      | 1       | 100  | -10           | 5.4     | 50    | 50    | 0         | 100  | 4      | 20.52   |    2.3   | -0.8        |
| 2022-01-19 | 4844   | 2      | 600      | 1       | 100  | -10           | 5.4     | 50    | 50    | 0         | 100  | 4      | 20.52   |    2.3   | -0.55       |
| 2022-01-20 | 4844   | 11     | 600      | 1       | 100  | -10           | 5.4     | 50    | 50    | 0         | 100  | 4      | 20.52   |    2.3   | -0.5        |
| 2022-01-21 | 4844   | 17     | 600      | 1       | 100  | -10           | 5.4     | 50    | 50    | 0         | 100  | 4      | 20.52   |    2.3   | -0.72       |
| 2022-01-22 | 4844   | 13     | 600      | 1       | 100  | -10           | 5.4     | 50    | 50    | 0         | 100  | 4      | 20.52   |    2.3   | -0.56       |
| 2022-01-27 | 4752   | 22     | 600      | 1       | 100  | -10           | 5.4     | 50    | 50    | 0         | 100  | 4      | 20.52   |    2.3   | 1.78        |
| 2022-01-28 | 4752   | 3      | 600      | 1       | 100  | -10           | 5.4     | 50    | 50    | 0         | 100  | 4      | 20.52   |    2.3   | 0.67        |
| 2022-01-29 | 4752   | 20     | 600      | 1       | 100  | -10           | 5.4     | 50    | 50    | 0         | 100  | 4      | 20.52   |    2.3   | 3.74        |
| 2022-01-30 | 4752   | 11     | 600      | 1       | 100  | -10           | 5.4     | 50    | 50    | 0         | 100  | 4      | 20.52   |    2.3   | 1.79        |

In the case above there was no HFR value in the image file paths so the value HFR = 1.6 was obtained from the 'default.csv' file. It can also be seen that a 4-digit code is presented for each filter used. This code is AstroBin's representation of the filter and is described below.

The results in the 'acquisition.csv' file can be copied and pasted into AstroBin's import CSV dialogue.

## Additional Information

### Night Time Imaging N Astronomy ([N.I.N.A](https://nighttime-imaging.eu/))

This code was primarily developed for use with FITS/XIFS files generated by the astro-imaging package [N.I.N.A](https://nighttime-imaging.eu/), but with the use of the 'default.csv' file the code should be able to handle images generated by other packages. We will continue to use header files generated by N.I.N.A to help explain the code operation.

A FITS header file generated by N.I.N.A is quite rich in information, an example is given below:

| Key       | Value                                                                                         |
|-----------|-----------------------------------------------------------------------------------------------|
| SIMPLE    | True                                                                                          |
| BITPIX    | 16                                                                                            |
| NAXIS     | 2                                                                                             |
| NAXIS1    | 9576                                                                                          |
| NAXIS2    | 6388                                                                                          |
| EXTEND    | True                                                                                          |
| EXTEND    | True                                                                                          |
| BZERO     | 32768                                                                                         |
| IMAGETYP  | LIGHT                                                                                         |
| EXPOSURE  | 600.0                                                                                         |
| EXPTIME   | 600.0                                                                                         |
| DATE-LOC  | 2023-08-10T23:30:56.722                                                                       |
| DATE-OBS  | 2023-08-10T22:30:56.722                                                                       |
| XBINNING  | 1                                                                                             |
| YBINNING  | 1                                                                                             |
| GAIN      | 100                                                                                           |
| OFFSET    | 10                                                                                            |
| EGAIN     | 0.246658                                                                                      |
| XPIXSZ    | 3.76                                                                                          |
| YPIXSZ    | 3.76                                                                                          |
| INSTRUME  | ZWO ASI6200MM Pro                                                                             |
| SET-TEMP  | -10.0                                                                                         |
| CCD-TEMP  | -10.0                                                                                         |
| USBLIMIT  | 40                                                                                            |
| TELESCOP  | NP101is                                                                                       |
| FOCALLEN  | 540.0                                                                                         |
| FOCRAITO  | 5.4                                                                                           |
| RA        | 303.912892                                                                                    |
| DEC       | 39.248115                                                                                     |
| CENTALT   | 76.64632                                                                                      |
| CENTAZ    | 163.49302                                                                                     |
| AIRMASS   | 1.027669                                                                                      |
| PIERSIDE  | West                                                                                          |
| SITEELEV  | 58.8                                                                                          |
| SITELAT   | 52.248472                                                                                     |
| SITELONG  | -0.123167                                                                                     |
| FWHEEL    | Starlight Xpress Fil                                                                          |
| FILTER    | OIII                                                                                          |
| OBJECT    | Gamma Cygni Nebula                                                                            |
| OBJCTRA   | 20 15 39                                                                                      |
| OBJCTDEC  | +39 14 56                                                                                     |
| OBJCTROT  | 0.0                                                                                           |
| FOCNAME   | FocusLynx Focuser 1                                                                           |
| FOCPOS    | 13196.0                                                                                       |
| FOCUSPOS  | 13196.0                                                                                       |
| FOCUSSZ   | 1.31                                                                                          |
| FOCTEMP   | 18.4                                                                                          |
| FOCUSTEM  | 18.4                                                                                          |
| CLOUDCVR  | 0.0                                                                                           |
| HUMIDITY  | 0.0                                                                                           |
| PRESSURE  | 0.0                                                                                           |
| AMBTEMP   | 0.0                                                                                           |
| WINDDIR   | 0.0                                                                                           |
| WINDSPD   | 0.0                                                                                           |
| ROWORDER  | TOP-DOWN                                                                                      |
| EQUINOX   | 2000.0                                                                                        |
| SWCREATE  | N.I.N.A. 2.3.0.2001                                                                           |
| file_path | /mnt/HDD_8TB/Preselected/Sadr Region/OIII/11th August/Sadr Region_Date_2023-08-10_Time_23-30-56_Filter_OIII_Exposure_600.00s_HFR_1.67pxs_FrameNo_0005.fits |
| DEWPOINT  | (empty value)                                                                                 |

A subset of these header file parameters are used in the code:

| Key      | Description                                                      |
|----------|------------------------------------------------------------------|
| EXPOSURE | image exposure time in seconds                                   |
| DATE-LOC | date at image location, YYYY-MM-DD is extracted                  |
| XBINNING | binning level used                                               |
| GAIN     | camera gain                                                      |
| XPIXSZ   | pixel size in microns                                            |
| CCD-TEMP | ccd temperature in degrees centigrade                            |
| FOCALLEN | telescope focal length in mm                                     |
| FOCRATIO | telescope focal ratio                                            |
| SITELAT  | latitude of observation site                                     |
| SITELONG | longitude of observation site                                    |
| FILTER   | filter used                                                      |
| OBJECT   | target name (sequence name in N.I.N.A)                           |
| FOCTEMP  | ambient temperature in degrees centigrade as measured by the focuser |

## AstroBin's Acquisition CSV File Format

AstroBin allows users to enter acquisition details associated with uploaded images via a CSV file. This section outlines the format for the `acquisition.csv` file, which will be automatically generated from image data by this code.

## AstroBin Long Exposure Acquisition Fields

| **Field**        | Description | Validation |
|------------------|-------------|------------|
| **date**         | The date when the acquisition took place | YYYY-MM-DD format |
| **filter**       | Filter used | Numeric ID of a valid filter (found in the URL of the filter's page in the equipment database) |
| **number***      | Number of frames | Whole number |
| **duration***    | Duration of each frame in seconds | Number, Max decimals: 4, Min value: 0.0001, Max value: 999999.9999 |
| **iso**          | ISO setting on the camera | Whole number |
| **binning**      | Binning of pixels | One of [1, 2, 3, 4] |
| **gain**         | Gain setting on the camera | Number, Max decimals: 2 |
| **sensorCooling**| The temperature of the chip in Celsius degrees, e.g., -20 | Whole number, Min value: -274, Max value: 100 |
| **fNumber**      | If a camera lens was used, specify the f-number used for this acquisition session | Number, Max decimals: 2, Min value: 0 |
| **darks**        | The number of dark frames | Whole number, Min value: 0 |
| **flats**        | The number of flat frames | Whole number, Min value: 0 |
| **flatDarks**    | The number of flat dark frames | Whole number, Min value: 0 |
| **bias**         | The number of bias/offset frames | Whole number, Min value: 0 |
| **bortle**       | Bortle dark-sky scale | Whole number, Min value: 1, Max value: 9 |
| **meanSqm**      | Mean SQM mag/arcsec^2 as measured by a Sky Quality Meter | Number, Max decimals: 2, Min value: 0 |
| **meanFwhm**     | Mean Full Width at Half Maximum in arc seconds, a measure of seeing | Number, Max decimals: 2, Min value: 0 |
| **temperature**  | Ambient temperature in Celsius degrees | Number, Max decimals: 2, Min value: -88, Max value: 58 |

## Example `acquisition.csv` file generated by this code for part

| **date** | **filter** | **number** | **duration** | **binning** | **gain** | **sensorCooling** | **fNumber** | **darks** | **flats** | **flatDarks** | **bias** | **meanSqm** | **meanFWHm** | **temperature** |
| -------- | ---------- | ---------- | ------------ | ----------- | -------- | ----------------- | ----------- | --------- | --------- | ------------- | -------- | ----------- | ------------- | -------------- |
| 2023-08-15 | 4663 | 9 | 600.0 | 1 | 100 | -10 | 5.4 | 50 | 150 | 0 | 100 | 20.4 | 3.6 | 10.7 |
| 2023-08-17 | 4663 | 19 | 600.0 | 1 | 100 | -10 | 5.4 | 50 | 150 | 0 | 100 | 20.4 | 3.6 | 13.3 |
| 2023-08-28 | 4663 | 2 | 600.0 | 1 | 100 | -10 | 5.4 | 50 | 150 | 0 | 100 | 20.4 | 3.6 | 11.8 |
| 2023-08-28 | 4844 | 7 | 600.0 | 1 | 100 | -10 | 5.4 | 50 | 150 | 0 | 100 | 20.4 | 3.6 | 10.4 |
| 2023-09-02 | 4663 | 16 | 600.0 | 1 | 100 | -10 | 5.4 | 50 | 150 | 0 | 100 | 20.4 | 3.6 | 13.6 |
| 2023-09-03 | 4663 | 14 | 600.0 | 1 | 100 | -10 | 5.4 | 50 | 150 | 0 | 100 | 20.4 | 3.6 | 11.3 |
| 2023-09-03 | 4844 | 16 | 600.0 | 1 | 100 | -10 | 5.4 | 50 | 150 | 0 | 100 | 20.4 | 3.6 | 15.9 |
| 2023-09-04 | 4844 | 27 | 600.0 | 1 | 100 | -10 | 5.4 | 50 | 150 | 0 | 100 | 20.4 | 3.6 | 13.7 |
| 2023-09-15 | 4752 | 33 | 600.0 | 1 | 100 | -10 | 5.4 | 50 | 150 | 0 | 100 | 20.4 | 3.6 | 12.7 |
| 2023-09-16 | 4752 | 25 | 600.0 | 1 | 100 | -10 | 5.4 | 50 | 150 | 0 | 100 | 20.4 | 3.6 | 11.8 |
| 2023-09-18 | 4844 | 4 | 600.0 | 1 | 100 | -10 | 5.4 | 50 | 150 | 0 | 100 | 20.4 | 3.6 | 11.1 |

The fields in the CSV are obtained or derived from the header files of the captured images. However, the filter value requires translation from the filter name to the AstroBin filter code.

## filter.csv file

The 'filter.csv' file defines the mapping from the filter name to AstroBin's filter code. The contents of a `filters.csv` file is given below. It shows the names my Astronomik 2 inch filters, as generated by N.I.N.A, and their corresponding AstroBin codes:

| **Filter** | **Code** |
| ---------- | -------- |
| Ha         | 4663     |
| SII        | 4844     |
| OIII       | 4752     |
| Red        | 4649     |
| Green      | 4643     |
| Blue       | 4637     |
| Lum        | 2906     |
| CLS        | 4061     |

You can edit this file to enable your filters used. The filter names should match the names N.I.N.A generates for your filters.

## Finding the AstroBin Numeric ID for Filters

The numeric ID of a filter can be found by examining the URL of the filter's page in the [AstroBin equipment database](https://app.astrobin.com/equipment/explorer/filter?page=1).   

For example, consider a [2-inch H-alpha CCD 6nm filter from Astronomik](https://app.astrobin.com/equipment/explorer/filter/4663/astronomik-h-alpha-ccd-6nm-2). By using [AstroBin's filter explorer](https://app.astrobin.com/equipment/explorer/filter?page=1) to navigate to this filter's page the URL is found to be :

https://app.astrobin.com/equipment/explorer/filter/4663/astronomik-h-alpha-ccd-6nm-2

From this URL, the AstroBin code for this Astronomik 2-inch H-alpha CCD 6nm filter is 4663.

## Creating the filters.csv File

The `filters.csv` file can be created using a text editor and should be located in the directory from which the Python script is executed. The script relies on this file to ensure the correct codes are used for your filters.

## sites.csv file

The `sites.csv` file contains the Bortle and SQM values for a given latitude and longitude. An example of a `sites.csv` file with one entry is given below.

| **Latitude** | **Longitude**| **Bortle** | **SQM** |
| ----------- | ------------ | ---------- | ------- |
| 52.248 | -0.123 | 4 | 20.52|

The program reads the `sites.csv` data into a dataframe. As the code loops through the data it obtains the site location's latitude and longitude from the header for the image under consideration. If the location matches a location in the sites dataframe it uses the values of Bortle and SQM for those coordinates.
If there is no match it obtains the values of Bortle and SQM from an external website. The code will then add the new site information to the sites.csv file. Site data also can be entered manually into the sites.csv. If no matching site data is found in the `sites.csv` and the external site cannot be accessed the Bortle and SQM values are set equal to zero.

## secret.csv and accessing sky quality data

The artificial_brightness of the sky at a given latitude and longitude is obtained from the excellent web resource https://www.lightpollutionmap.info. This can be by done by visiting the website and entering the latitude and longitude of the observation site and obtaining the parameters Bortle and SQM. These parameters can then be entered into the sites.csv file. It can also be done programmatically by the code. To do this you need to place an API_KEY and API_ENDPOINT for the service in the secret.csv file. The only API_ENDPOINT supported currently is https://www.lightpollutionmap.info/QueryRaster/. You will have to apply to Jurij Stare, the website owner, for an API key. Jurij's email address is starej@t-2.net. The approach I suggest is: donate a small amount

`secrets.csv` format
| **API Key** | **API Endpoint**|
| ----------- | --------------- | 
| **************** | https://www.lightpollutionmap.info/QueryRaster/ |

## FWHM Values

The AstroBin Long Exposure Acquisition Fields has an entry for meanFwhm. This is not directly available from the header file. But N.I.N.A allows for the mean HFR value of an image to be embedded in the image file name. An example of my file naming convention, with HFR embedded, is given below:

'NGC 7822_Panel_1_Date_2023-09-02_Time_21-09-01_Filter_Ha_Exposure_600.00s_HFR_1.64px_FrameNo_0002.fits'

The code will look for the keyword HFR in the image file name. If found it will extract the HFR value and assign it to a variable hfr. As HFR is in pixels, it calculates the image scale from the telescope information held in the FITS header. In particular XPIXSZ the x pixel size in microns and FOCALLEN the telescope focal length in mm

imscale = XPIXSZ / FOCALLEN * 206.265
fwhm = hfr * imscale

The calculations above assume that all stars are circular making hfr and fwhm are equivalent. This is a reasonable approximation as the code averages hfr across all images taken on a particular date with a given filter and gain and then AstroBin further averages hfr across all entries in the uploaded CSV file. Where HFR is not available in the filename it is obtained from the defaults.csv file.

## default.csv File

The use of a `default.csv` file enables the code to run with headers produced by different capture programs. The `default.csv` file contains default definitions for each header keyword that is required to be mapped to the Long Exposure Acquisition Fields. It also allows for a definition of HFR in pixels. The contents of the `default.csv` file provided in the repository are shown below.

`default.csv` format

| Key      | Value     | Description                                      |
|----------|-----------|--------------------------------------------------|
| EXPOSURE | 100       | Exposure time in seconds                         |
| DATE-LOC | 2023-01-01| Observation date                                 |
| XBINNING | 1         | Camera binning                                   |
| GAIN     | 0         | Camera gain                                      |
| XPIXSZ   | 1         | Camera pixel size in um                          |
| CCD-TEMP | -10       | Camera sensor temperature in degrees C           |
| FOCALLEN | 540       | Telescope focal length in mm                     |
| FOCRATIO | 5.4       | Telescope focal ratio                            |
| SITELAT  | 52.25     | Observation site latitude in decimal degrees     |
| SITELONG | -0.12     | Observation site longitude in decimal degrees    |
| FILTER   | No Filter | Filter name                                      |
| OBJECT   | No target | Target name                                      |
| FOCTEMP  | 20        | Ambient temperature in degrees C as measure by the focuser |
| HFR      | 1.6       | Half-flux radius in pixels                       |

This table contains all elements required by AstroBin, however, if your headers are not reporting the basics, for instance EXPOSURE, you cannot make good use of this code as all your images will be reported with the same EXPOSURE time to AstroBin. All elements are included for completeness. To use this correctly inspect a header using your package of choice and enter suitable values to represent your imaging setup that are missing from your header files. For imagers that use color cameras with filters that are not reported in the header files, enter the filter code for your filter in the `filters.csv`, along with the AstroBin filter code. Then enter the same name in the `defaults.csv`. This will ensure that your filter code is applied to all your images when uploaded to AstroBin.

## Contributing to AstroImage Processing Script

This script is intended for educational purposes in the field of astrophotography. It is part of an open-source project and contributions or suggestions for improvements are welcome.

To contribute to this project, follow these steps:

1. Fork this repository.
2. Create a branch: `git checkout -b <branch_name>`.
3. Make your changes and commit them: `git commit -m '<commit_message>'`.
4. Push to the original branch: `git push origin <project_name>/<location>`.
5. Create the pull request.

Alternatively, see the GitHub documentation on [creating a pull request](https://docs.github.com/en/github/collaborating-with-issues-and-pull-requests/creating-a-pull-request).

## Contact

If you want to contact me, you can reach me at sgreaves139@gmail.com.

## Licence

This project uses the following licence: [GNU General Public Licence v3.0](https://github.com/SteveGreaves/AstroBinUploader/blob/main/LICENSE).



