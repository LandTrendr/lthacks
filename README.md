# lthacks
Python package for image processing and geospatial summarizing associated with LandTrendr projects.

#### *lthacks.py*

This python module contains most of the important functions that are used to run validation. Originally called “validation_funs.py” and used for TimeSync Validation. However, these functions can also be useful anytime you need to extract pixels from a map, open/manipulate/save CSV files and data structures, add metadata to your outputs, and more...


Functions:

*Extracting Pixels*
- pixel_values = extract_kernel(spec_ds, x, y, width, height, band, transform)
- pixel_values, pixel_coords = extract_kernel_and_coords(spec_ds, x, y, width, height, band, transform)

*Working with CSVs & Data Structures*
- arrayToCsv(numpy_array, output_path)
- numpy_array = csvToArray(input_csv_path, names=True)
- ds = loadPickle(input_pickle_path)
- savePickle(ds, output_pickle_path)
- array_rows = extractTSArows(csvdata_structured_array, tsa_list, tsa_col="TSA")
- csvdata_structured_array, additional_headers = appendSumKernels(csvdata_structured_array, column_prefixes_list)
- csvdata_structured_array = appendMetric(csvdata_structured_array, metric, columnPrefix)

*Calculating Statistics*
- statistical_function = getStatFunc(stat_string, options=None)
- confusion_matrix = makeConfusion(y_test, predictions, classes)
- confusion_matrix = makeConfusion_diffLabels(data, truthCol, predictionCol)

*LandTrendr-specific Functions*
- pathrow_6dig = sixDigitTSA(pathrow)
- pathrow_4dig = fourDigitTSA(pathrow)
- pathrow_6dig = findTSA(tsa_ref_mask, x_coord, y_coord)
- tsa_list = expandPathRows(sceneSets)
- lt_file_path = getLTFile(pathrow, search_strings)

*Writing Metadata*
- createMetadata(arguments, outputPath_data, altMetaDir=None, description=None, lastCommit="UNKNOWN")
- commit_string = getLastCommit(scriptPath)

.
.

#####*Extracting Pixels*
______________________________________________________________________
**pixel_values = extract_kernel(spec_ds, x, y, width, height, band, transform)**

Reads value(s) from band centered around [x,y] with width and height

______________________________________________________________________
**pixel_values, pixel_coords = extract_kernel_and_coords(spec_ds, x, y, width, height, band, transform)**

Reads value(s) from band centered around [x,y] with width and height. Also returns corresponding coordinates.

.
#####*Working with CSVs & Data Structures*
______________________________________________________________________
**arrayToCsv(numpy_array, output_path)**

Saves a CSV to specified output_path location. If numpy_array is a structure array with names, CSV will include a row of headers. This is a numpy.savetxt wrapper.

______________________________________________________________________
**numpy_array = csvToArray(input_csv_path, names=True)**

Returns a structured numpy array from a specified CSV file. This is a numpy.genfromtxt wrapper.

______________________________________________________________________
**ds = loadPickle(input_pickle_path)**

Unpickles a data structure.

______________________________________________________________________
**savePickle(ds, output_pickle_path)**

Pickles a saves a data structure.

______________________________________________________________________
**array_rows = extractTSArows(csvdata_structured_array, tsa_list, tsa_col="TSA")**

Returns rows from csvdata_structured_array that match given TSAs

______________________________________________________________________
**csvdata_structured_array, additional_headers = appendSumKernels(csvdata_structured_array, column_prefixes_list)**

Calculates the sum of matching pixels from different maps, indicated by columnPrefixes, and appends sum as a column to structured array. 

______________________________________________________________________
**structured_numpy_array = appendMetric(structured_numpy_array, metric, columnPrefix)**

Appends a metric [mean, median, mode, min, max, stdev, num_pix_gt_0, num_pix_equal, num_pix_between] column to a structured array of data. Metric is calculated from all fields starting with columnPrefix. Options are only necessary for num_pix_equal & num_pix_between, and should be a list.

.
#####*Calculating Statistics*
______________________________________________________________________
statistical_function = getStatFunc(stat_string, options=None)

Returns a statistical function from a string.
Stat menu: median, mean, mode, min, max, stdev, num_pix_gt_0, num_pix_equal, num_pix_between
Options are only necessary for num_pix_equal & num_pix_between, and should be a list.

___________________________________________________________________
**confusionMatrix = makeConfusion(y_test, predictions, classes)**

Creates a confusion matrix & calculated producers, users & overall accuracies. All inputs are array-like type. Output is a structured numpy array.

___________________________________________________________________
**confusionMatrix = makeConfusion_diffLabels(data, truthCol, predictionCol)**
Creates a confusion matrix for datasets with different truth & prediction labels. Does NOT calculate users/producers accuracy. truthCol and predictionCol are strings.

.
#####*LandTrendr-specific Functions*
______________________________________________________________________
**pathrow_6dig = sixDigitTSA(pathrow)**

Makes TSA six digit string for searching directories 

______________________________________________________________________
**pathrow_4dig = fourDigitTSA(pathrow)**

Makes TSA four digit string for searching CSV tables

______________________________________________________________________
**pathrow_6dig = findTSA(tsa_ref_mask, x_coord, y_coord)**

Returns 6-digit Landsat scene as string for given coordinates. tsa_ref_mask is a mosaic of TSA masks for region of interest.

______________________________________________________________________
**listOf6digitTSAs = expandPathRows(sceneSets)**

Returns list of 6 digit scene numbers from a string of "scene sets" in the format: *41-43/26, 39-43/27, 38-43/28, 38-42/29-31*

______________________________________________________________________
**createMetadata(arguments, outputPath_data, altMetaDir=None, description=None, lastCommit="UNKNOWN")**

Saves a meta.txt file describing a dataset. Add this function to any script that produces significant data. Meta file includes the full path of the dataset, the name of the script it was generated by, last commit date of the script, full command used to generate dataset, the time the dataset was created, and user it was generated by.

______________________________________________________________________
**ltfile = getLTFile(pathrow, search_strings)**

Finds file location within LandTrendr scenes directory. search_strings is a list. Example: ['outputs/nbr/nbr_lt_labels/*[0-9]_greatest_fast_disturbance_mmu11_tight.bsq',	   'outputs/nbr/nbr_lt_labels_mr227/*[0-9]_greatest_fast_disturbance_mmu11_tight.bsq']

**dependent on SCENES_DIR global variable being set on top of lthacks.py script

.
#####*Writing Metadata*
______________________________________________________________________
**createMetadata(arguments, outputPath_data, altMetaDir=None, description=None, lastCommit="UNKNOWN")**

Creates a meta.txt file describing a dataset. Add this function to any script that produces significant data.

______________________________________________________________________
**commit_string = getLastCommit(scriptPath)**

Returns last git commit hash, user, and time of specified script.



######.
######.
######.

#### *kernelshapes.py*


This module contains class definitions for various kernel shapes used to extract pixels surrounding an (x,y) coordinate from a map.

Shape Menu:
- Rectangle(width, height)
- Circle(diameter)
- Triangle(width, height, quadrant)
- PieSlice(diameter, startangle, endangle, argNegBool=False)

.
______________________________________________________________________
**class kernelshapes.Rectangle(width, height)**

| PARAMETER     |             |
| :------------ |:-------------|
| **height**    | *integer* - height of the rectangle kernel for extraction|
| **width**     | *integer* - width of the rectangle kernel for extraction |

| ATTRIBUTE    |             |
| :------------ |:-------------|
| **height**    | *integer* - height of the rectangle kernel for extraction|
| **width**     | *integer* - width of the rectangle kernel for extraction |

Methods:

*masked_array, data_only_pixels = Rectangle.extract_kernel(ds, band, x, y, transform)*

Extracts rectangle kernel from dataset and returns a 2-d numpy masked array, and a flattened array of only non-masked pixels. For the rectangle kernel, these are the same.

.
______________________________________________________________________
**class kernelshapes.Circle(diameter)**

| PARAMETER     |             |
| :------------ |:-------------|
| **diameter**    | *integer* - diameter of circle kernel|

| ATTRIBUTE     |             |
| :------------ |:-------------|
| **radius**    | *float* - radius of circle kernel|
| **height**    | *integer* - height of the rectangle kernel for extraction|
| **width**     | *integer* - width of the rectangle kernel for extraction |

Methods:

*masked_array, data_only_pixels = Circle.extract_kernel(ds, band, x, y, transform)*

Extracts circle kernel from dataset and returns a 2-d numpy masked array, and a flattened array of only non-masked pixels. 

.
______________________________________________________________________
**class kernelshapes.Triangle(width, height, quadrant)**

| PARAMETER     |             |
| :------------ |:-------------|
| **width**     | *integer* - width of the rectangle kernel for extraction |
| **height**    | *integer* - height of the rectangle kernel for extraction|
| **quadrant**     | *string* - which section of rectangle kernel to extract triangle from [choices: “north”, “east”, “south”, “west”] |
NOTE: Use parameters to control angles & direction of triangle.

| ATTRIBUTE     |             |
| :------------ |:-------------|
| **width**     | *integer* - width of the rectangle kernel for extraction |
| **height**    | *integer* - height of the rectangle kernel for extraction|
| **quadrant**     | *string* - which section of rectangle kernel to extract triangle from |
| **midpoint**     | *tuple* - middle coordinate of rectangle kernel |


Methods:

*masked_array, data_only_pixels = Triangle.extract_kernel(ds, band, x, y, transform)*

Extracts triangle kernel from dataset and returns a 2-d numpy masked array, and a flattened array of only non-masked pixels.

.
______________________________________________________________________
**class kernelshapes.PieSlice(diameter, startangle, endangle, arcNegBool=False)**

| PARAMETER     |             |
| :------------ |:-------------|
| **diameter**    | *integer* - diameter of circle kernel|
| **startangle**     | *float* - start angle of pie arc, in degrees. Angle is based off unit circle. |
| **endangle**    | *float* - end angle of pie arc, in degrees. Angle is based off unit circle.|
| **arcNegBool**     | *boolean, optional (default = False)* - Arc to be drawn in the direction of decreasing angles? |

| ATTRIBUTE     |             |
| :------------ |:-------------|
| **radius**    | *float* - radius of circle kernel|
| **width**     | *integer* - width of the rectangle kernel for extraction |
| **height**    | *integer* - height of the rectangle kernel for extraction|
| **startangle**     | *float* - start angle of pie arc, in degrees. Angle is based off unit circle. |
| **endangle**    | *float* - end angle of pie arc, in degrees. Angle is based off unit circle.|
| **arcNegBool**     | *boolean, optional (default = False)* - Arc to be drawn in the direction of decreasing angles? |


Methods:

*masked_array, data_only_pixels = PieSlice.extract_kernel(ds, band, x, y, transform)*

Extracts pie slice kernel from dataset and returns a 2-d numpy masked array, and a flattened array of only non-masked pixels.

######.
######.
######.

#### *intersectMask.py*


This is a set a functions originally used for the intersectMask command-line utility. However, these functions may be useful in other scripts where you need to mask an array on the fly or compare the extent of 2 rasters.

Functions
- maskAsArray
- saveArrayAsRaster
- saveArrayAsRaster_multiband
- findLeastCommonBoundaries (from a list of maps)
- GetExtent (returns corner coordinates of map)
- transformToCenter (returns center coordinate of a map

______________________________________________________________________
**outBandArray, transform, projection, driver, nodata, datatype = maskAsArray(sourcePath, maskPath, src_band=1, msk_band=1, msk_value=None)**

This is useful for masking within a routine if not interested in saving the masked raster. It outputs the masked out numpy array, the transform, projection, and driver of the source raster.

______________________________________________________________________
**saveArrayAsRaster(outBandArray, transform, projection, driver, outPath, datatype, nodata=None)**

This saves a 2D numpy array as a raster file. 
*Hint: To get transform, projection, & driver information from an existing raster, open the file using ds=gdal.Open(rasterPath, GA_ReadOnly), then use projection=ds.GetProjection() & transform=ds.GetGeoTransform() & ds.GetDriver(). To get datatype: band.DataType*

______________________________________________________________________
**saveArrayAsRaster_multiband(outbands, transform, projection, driver, outPath, datatype, nodata=None)**

This saves a list of numpy arrays as a raster file (1 array per band).

______________________________________________________________________
**corners = GetExtent(geotransform, cols, rows)**

Returns a list of corner coordinates from a geotransform & number of columns+rows.

______________________________________________________________________
**finalSize, finalTransform, projection, driver = findLeastCommonBoundaries(listOfMaps)**

Determines the least common extent from a list of maps.

______________________________________________________________________
**center = transformToCenter(transform, cols, rows)**

Returns the center x-y coordinate.

