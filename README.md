# Sentinel-3-with-Dask


Hi! In this folder, you can find my code to process Sentinel3 SLSTR data (using Dask), and visualize an NDVI plot. You'll find the following in the main folder -

- Codes Folder: Contains-
 	- code to process data and compute and visualize NDVI for a 			single dataset (NDVI_Computation.ipynb). Please change the path 	to the respective dataset that you want to run it for, and change the name of the file you want to export the data to.
	- code to visualize the NDVI for the two datasets provided (as they are spatially continuous). Note that this takes the two netCDF files that are exported in the previous code for the two given datasets.

- Results Folder: Contains-
    * the NDVI netCDF files for the two datasets provided
    * reflectance netCDF files
    * the joint visualization of the two NDVI images as tiff and png

- Report.md summarizing the results and pre-processing/processing of the data.
Enjoy!
