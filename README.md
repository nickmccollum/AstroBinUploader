# README.md for GitHub Repository

## Features

This Python script is designed for astronomers and astrophotographers to process and analyze FITS (Flexible Image Transport System) file headers. Key features include:

- **Extraction of FITS Headers**: Automatically extracts headers from FITS files in specified directories.
- **Session Summarization**: Summarizes observation sessions by processing LIGHT, FLAT, BIAS, and DARK frames.
- **Calibration Data Handling**: Creates a DataFrame for calibration data like DARK, BIAS, FLAT, and FLATDARKS.
- **LIGHT Data Processing**: Aggregates 'LIGHT' type data for detailed analysis.
- **Sky Quality Analysis**: Retrieves Bortle scale classification and SQM values based on the location's coordinates.
- **Auxiliary Parameter Calculation**: Calculates additional parameters like HFR, IMSCALE, and FWHM for each observation.
- **AstroBin Compatibility**: Formats aggregated data for easy uploading to AstroBin.

## Prerequisites

Before using this script, ensure you have the following installed:

- Python 3.x
- Pandas library
- Astropy library
- Requests library

FITS image files must be generated by the image capture package [N.I.N.A](https://nighttime-imaging.eu/)

## Installation and Usage

To use this script, follow these steps:

1. Clone or download the repository from GitHub.
2. Ensure you have all the required libraries installed. If not, use `pip install pandas astropy requests`.
3. Place the script in a directory with your FITS files or specify the path to the directories as command-line arguments.
4. Run the script using Python: `python script_name.py [directory_path1] [directory_path2] ...`.

## External Files

The script requires the following external CSV files for its operation:

- **filter.csv**: Contains mappings of filter names to their codes.
- **sites.csv**: Stores latitude, longitude, Bortle scale, and SQM values for known locations.
- **secret.csv**: Holds API keys and endpoints for external services (e.g., light pollution data).

Ensure these files are present in the same directory as the script or update their paths in the script accordingly.

## Additional Information

This script is intended for educational purposes in the field of astronomy. It is part of an open-source project and contributions or suggestions for improvements are welcome.

# Night Time Imaging N Astronomy (N.I.N.A)

This code has been developed for use with FITS files generated by [N.I.N.A](https://nighttime-imaging.eu/) At the moment it will not work with images produced by other capture packages, such as SGP. If there is enough interest I may make it compatible with other popular packages.
A typical FITS header file generated by N.I.N.A is quite rich and is given below:

| Key       | Value                                                                                         |
|-----------|-----------------------------------------------------------------------------------------------|
| SIMPLE    | True                                                                                          |
| BITPIX    | 16                                                                                            |
| NAXIS     | 2                                                                                             |
| NAXIS1    | 9576                                                                                          |
| NAXIS2    | 6388                                                                                          |
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
| BORTLE    | 4.0                                                                                           |
| SQM       | 20.4                                                                                          |
| IMSCALE   | 1.436216                                                                                      |
| HFR       | 1.67                                                                                          |
| FWHM      | 2.39848                                                                                       |
| file_path | /mnt/HDD_8TB/Preselected/Sadr Region/OIII/11th August/Sadr Region_Date_2023-08-10_Time_23-30-56_Filter_OIII_Exposure_600.00s_HFR_1.67pxs_FrameNo_0005.fits |
| DEWPOINT  | (empty value)                                                                                 |


# AstroBin's Acquisition CSV File Format

AstroBin allows users to enter acquisition details associated with uploaded images via a CSV file. This section outlines the format for the `acquisition.csv` file, which will be automatically generated from image data by this code.

## AstroBin Long Exposure Acquisition Fields


| **Field** | Description | Validation |
| --------- | ----------- | ---------- |
| **date** | The date when the acquisition took place | YYYY-MM-DD format |
| **filter** | Filter used | Numeric ID of a valid filter (found in the URL of the filter's page in the equipment database) |
| **number*** | Number of frames | Whole number |
| **duration*** | Duration of each frame in seconds | Number, Max decimals: 4, Min value: 0.0001, Max value: 999999.9999 |
| **iso** | ISO setting on the camera | Whole number |
| **binning** | Binning of pixels | One of [1, 2, 3, 4] |
| **gain** | Gain setting on the camera | Number, Max decimals: 2 |
| **sensorCooling** | The temperature of the chip in Celsius degrees, e.g., -20 | Whole number, Min value: -274, Max value: 100 |
| **fnumber** | If a camera lens was used, specify the f-number used for this acquisition session | Number, Max decimals: 2, Min value: 0 |
| **darks** | The number of dark frames | Whole number, Min value: 0 |
| **flats** | The number of flat frames | Whole number, Min value: 0 |
| **flatDarks** | The number of flat dark frames | Whole number, Min value: 0 |
| **bias** | The number of bias/offset frames | Whole number, Min value: 0 |
| **bortle** | Bortle dark-sky scale | Whole number, Min value: 1, Max value: 9 |
| **meanSqm** | Mean SQM mag/arcsec^2 as measured by a Sky Quality Meter | Number, Max decimals: 2, Min value: 0 |
| **meanFwhm** | Mean Full Width at Half Maximum in arc seconds, a measure of seeing | Number, Max decimals: 2, Min value: 0 |
| **temperature** | Ambient temperature in Celsius degrees | Number, Max decimals: 2, Min value: -88, Max value: 58 |

# Example `acquisition.csv` file generated by this code for part of an observing session

| **date** | **filter** | **number** | **duration** | **binning** | **gain** | **sensorCooling** | **darks** | **flats** | **flatDarks** | **bias** | **meanSqm** | **meanFWHm** | **temperature** |
| -------- | ---------- | ---------- | ------------ | ----------- | -------- | ----------------- | --------- | --------- | ------------- | -------- | ----------- | ------------- | -------------- |
| 2023-08-15 | 4663 | 9 | 600.0 | 1 | 100 | -10 | 50 | 150 | 0 | 100 | 20.4 | 3.6 | 10.7 |
| 2023-08-17 | 4663 | 19 | 600.0 | 1 | 100 | -10 | 50 | 150 | 0 | 100 | 20.4 | 3.6 | 13.3 |
| 2023-08-28 | 4663 | 2 | 600.0 | 1 | 100 | -10 | 50 | 150 | 0 | 100 | 20.4 | 3.6 | 11.8 |
| 2023-08-28 | 4844 | 7 | 600.0 | 1 | 100 | -10 | 50 | 150 | 0 | 100 | 20.4 | 3.6 | 10.4 |
| 2023-09-02 | 4663 | 16 | 600.0 | 1 | 100 | -10 | 50 | 150 | 0 | 100 | 20.4 | 3.6 | 13.6 |
| 2023-09-03 | 4663 | 14 | 600.0 | 1 | 100 | -10 | 50 | 150 | 0 | 100 | 20.4 | 3.6 | 11.3 |
| 2023-09-03 | 4844 | 16 | 600.0 | 1 | 100 | -10 | 50 | 150 | 0 | 100 | 20.4 | 3.6 | 15.9 |
| 2023-09-04 | 4844 | 27 | 600.0 | 1 | 100 | -10 | 50 | 150 | 0 | 100 | 20.4 | 3.6 | 13.7 |
| 2023-09-15 | 4752 | 33 | 600.0 | 1 | 100 | -10 | 50 | 150 | 0 | 100 | 20.4 | 3.6 | 12.7 |
| 2023-09-16 | 4752 | 25 | 600.0 | 1 | 100 | -10 | 50 | 150 | 0 | 100 | 20.4 | 3.6 | 11.8 |
| 2023-09-18 | 4844 | 4 | 600.0 | 1 | 100 | -10 | 50 | 150 | 0 | 100 | 20.4 | 3.6 | 11.1 |

The fields in the CSV can be obtained or derived from the FITS header files of the captured images. However, the filter value requires translation from the filter name to a filter code requred by AstroBin 

## filter.csv file

The 'Filter.csv' file defines the mapping from filter name to AstroBin filter code. Given below is an example showing my filter set of Astronomik filters and their AstroBin codes:

| **Filter** | **Code** |
| ---------- | -------- |
| Ha | 4663 |
| SII | 4844 |
| OIII | 4752 |
| Red | 4649 |
| Green | 4643 |
| Blue | 4637 |
| Lum | 2906 |
| CLS | 4061 |

You can edit this file to enable your filters to be represented

### Finding the AstroBin Numeric ID for Filters

As indicated in the validation column for the filter field, the numeric ID of a valid filter can be found by examining the URL of the filter's page in the [AstroBin equipment database](https://app.astrobin.com/equipment/explorer/filter?page=1).   

For example, consider a Ha filter: a 2-inch H-alpha CCD 6nm filter from Astronomik. By using [AstroBin's filter explorer](https://app.astrobin.com/equipment/explorer/filter?page=1) to navigate to this filter's page, the URL is:

`https://app.astrobin.com/equipment/explorer/filter/4663/astronomik-h-alpha-ccd-6nm-2`

From this URL, the filter code for code for a 2-inch H-alpha CCD 6nm filter from Astronomik 4663.

### Creating the filters.csv File

The `filters.csv` file can be created using a text editor and should be located in the directory from which the Python script is executed. The script relies on this file to ensure the correct codes are used for your filters.

## sites.csv file

The sites.csv file contains the Bortle and SQM values for a given latitude and longtitude. An example of a sites.csv file with one entry is given below.

|
|**Latitude** | **Longitude**| **Bortle** | **SQM** |
| ----------- | ------------ | ---------- | ------- |
| 52.248 | -0.123 | 4 | 20.52|

The program reads this sites.csv data into a dataframe. As the code loops through the data it obtains the site location's latitude and longtitude from the FITS header for the image under consideration. If the location matches a location in teh sites dataframe it uses the values
of Bortle and SQM for that location from teh data frame. If there is no match it obtains the values of Bortle and SQM from an external website. The code will then add the new site information to the list. Site data also can be entered manually into the sites.csv. If no matching site data is in the sites.csv and teh external site cannot be accessed the parameters Bortle and SQM are set equal to zero.

## secret.csv and accessing sky quality data

The artificial_brightness of the sky at a given latitude and longtitude is obtained from the excellent web resource https://www.lightpollutionmap.info. This can be by visiting the website and entering the parameters obtained into the sites.csv file. It can also be done programatically by the code. To do this you need place an API_KEY and API_ENDPOINT for the service in the secret.csv file. The only API_ENDPOINT supported currently is https://www.lightpollutionmap.info/QueryRaster/. You have to apply Jurij Stare, the website owner, for an API key. Jurij's emaail address is starej@t-2.net. The approach I suggest is as follows: donate a small amount to to his excellent website. You will then receive a thank you e-mail and in response to this ask Jurij for an API key.  If there are other endpoints prople wish to be supported I may could look at adjusting the code to handle the response format required, let me know.

secret.csv format
| **API Key** | **API Endpoint**|
| ----------- | --------------- | 
| **************** | https://www.lightpollutionmap.info/QueryRaster/ |


The `get_bortle_sqm` function is designed to `get_bortle_sqm` function is designed to retrieve the Bortle scale classification and the SQM (Sky Quality Meter) value for a specified geographic location. It then converts the result into SQM in $\text{mag} \cdot \text{arcsec}^{-2}$ as well as the Bortle scale. The Bortle scale is a nine-level numeric scale that quantifies the observability of celestial objects (like stars and planets) and the impact of light pollution on the night sky. Bortle scale is provided by the helper function `sqm_to_bortle`

## FWHM values

The AstroBin Long Exposure Acquisition Fields has an entry for meanFwhm. This is not directly available from the FITS header file whne the image is generated by N.I.N.A. But both N.I.N.A and SGP allow for the mean HFR value of an image to be embedded in the image file name. An example of my file naming convention is given below: 

NGC 7822_Panel_1_Date_2023-09-02_Time_21-09-01_Filter_Ha_Exposure_600.00s_HFR_1.64px_FrameNo_0002.fits

The code will look for the key word HFR in the image file name. If it finds it it will extract the HFR value and assign it to a variable hfr. As HFR is in pixels it calculates the image scale from the telescope information held in the FITS header. In particular XPIXSZ: x pixel size in microns and FOCALLEN: telescope focal length in mm

imscale = XPIXSZ / FOCALLEN * 206.265
fwhm = hfr * imscale

The calculations above assume that all stars are circular and so hfr and fwhm are equivalent, its a reasonable approximation as the code averages across all images taken on a particular date with a given filter and gain and then AstroBin averages across all entries in the uploaded csv file. 

## Contributing to AstroImage Processing Script

To contribute to this project, follow these steps:

1. Fork this repository.
2. Create a branch: `git checkout -b <branch_name>`.
3. Make your changes and commit them: `git commit -m '<commit_message>'`.
4. Push to the original branch: `git push origin <project_name>/<location>`.
5. Create the pull request.

Alternatively, see the GitHub documentation on [creating a pull request](https://docs.github.com/en/github/collaborating-with-issues-and-pull-requests/creating-a-pull-request).

## Contact

If you want to contact me, you can reach me at sgreaves139@gmail.com

## License

This project uses the following license: [GNU General Public License v3.0](https://github.com/SteveGreaves/AstroBinUploader/blob/main/LICENSE).

