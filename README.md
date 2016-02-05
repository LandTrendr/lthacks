# lthacks
Set of custom modules for python scripting associated with LandTrendr projects.

#### *lthacks.py*

This python module contains most of the important functions that are used to run validation. Originally called “validation_funs.py” and used for TimeSync Validation. However, these functions can also be useful anytime you need to extract pixels from a map, open/manipulate/save CSV files and data structures, add metadata to your outputs, and more...


Functions (most commonly used up top):
- extract_kernel
- extract_kernel_and_coords
- sixDigitTSA
- fourDigitTSA
- arrayToCsv
- csvToArray
- createMetadata
- getLastCommit
- expandPathRows
- appendMetric
- makeConfusion
- makeConfusion_diffLabels
- findTSA
- getLTFile

______________________________________________________________________
**pixel_values = extract_kernel(spec_ds, x, y, width, height, band, transform)**

Reads value(s) from band centered around [x,y] with width and height

______________________________________________________________________
**pixel_values, pixel_coords = extract_kernel_and_coords(spec_ds, x, y, width, height, band, transform)**

Reads value(s) from band centered around [x,y] with width and height. Also returns corresponding coordinates.

______________________________________________________________________
**pathrow_6dig = sixDigitTSA(pathrow)**

Makes TSA six digit string for searching directories 

______________________________________________________________________
**pathrow_4dig = fourDigitTSA(pathrow)**

Makes TSA four digit string for searching CSV tables

______________________________________________________________________
**arrayToCsv(numpy_array, output_path)**

Saves a CSV to specified output_path location. If numpy_array is a structure array with names, CSV will include a row of headers. This is a numpy.savetxt wrapper.

______________________________________________________________________
**numpy_array = csvToArray(input_csv_path, names=True)**

Returns a structured numpy array from a specified CSV file. This is a numpy.genfromtxt wrapper.

______________________________________________________________________
**createMetadata(arguments, outputPath_data, altMetaDir=None, description=None, lastCommit="UNKNOWN")**

Saves a meta.txt file describing a dataset. Add this function to any script that produces significant data. Meta file includes the full path of the dataset, the name of the script it was generated by, last commit date of the script, full command used to generate dataset, the time the dataset was created, and user it was generated by.

______________________________________________________________________
**listOf6digitTSAs = expandPathRows(sceneSets)**

Returns list of 6 digit scene numbers from a string of "scene sets" in the format: *41-43/26, 39-43/27, 38-43/28, 38-42/29-31*

______________________________________________________________________
**structured_numpy_array = appendMetric(structured_numpy_array, metric, columnPrefix)**

Appends a metric [mean, median, max, min, or num_pix_gt_0] column to a structured array of data. Metric is calculated from all fields starting with columnPrefix.

___________________________________________________________________
**confusionMatrix = makeConfusion(y_test, predictions, classes)**

Creates a confusion matrix & calculated producers, users & overall accuracies. All inputs are array-like type. Output is a structured numpy array.

___________________________________________________________________
**confusionMatrix = makeConfusion_diffLabels(data, truthCol, predictionCol)**
Creates a confusion matrix for datasets with different truth & prediction labels. Does NOT calculate users/producers accuracy. truthCol and predictionCol are strings.

______________________________________________________________________
**pathrow_6dig = findTSA(tsa_ref_mask, x_coord, y_coord)**

Returns 6-digit Landsat scene as string for given coordinates. tsa_ref_mask is a mosaic of TSA masks for region of interest.

______________________________________________________________________
**ltfile = getLTFile(pathrow, search_strings)**

Finds file location within LandTrendr scenes directory. search_strings is a list. Example: ['outputs/nbr/nbr_lt_labels/*[0-9]_greatest_fast_disturbance_mmu11_tight.bsq',	   'outputs/nbr/nbr_lt_labels_mr227/*[0-9]_greatest_fast_disturbance_mmu11_tight.bsq']

**dependent on SCENES_DIR global variable being set on top of lthacks.py script

######.
######.
######.

#### *intersectMask.py*


This is a set a functions originally used for the intersectMask command-line utility. However, these functions may be useful in other scripts where you need to mask an array on the fly or compare the extent of 2 rasters.

Functions
- maskAsArray
- saveArrayAsRaster
- findLeastCommonBoundaries (from a list of maps)
- GetExtent (returns corner coordinates of map)
- transformToCenter (returns center coordinate of a map

______________________________________________________________________
**outBandArray, transform, projection, driver, nodata, datatype = maskAsArray(sourcePath, maskPath, src_band=1, msk_band=1, msk_value=None)**

This is useful for masking within a routine if not interested in saving the masked raster. It outputs the masked out numpy array, the transform, projection, and driver of the source raster.

______________________________________________________________________
**saveArrayAsRaster(outBandArray, transform, projection, driver, outPath, datatype, nodata=None)**

This saves a numpy array as a raster file. 
*Hint: To get transform, projection, & driver information from an existing raster, open the file using ds=gdal.Open(rasterPath, GA_ReadOnly), then use projection=ds.GetProjection() & transform=ds.GetGeoTransform() & ds.GetDriver(). To get datatype: band.DataType*

______________________________________________________________________
**corners = GetExtent(geotransform, cols, rows)**

Returns a list of corner coordinates from a geotransform & number of columns+rows.

______________________________________________________________________
**finalSize, finalTransform, projection, driver = findLeastCommonBoundaries(listOfMaps)**

Determines the least common extent from a list of maps.

______________________________________________________________________
**center = transformToCenter(transform, cols, rows)**

Returns the center x-y coordinate.
