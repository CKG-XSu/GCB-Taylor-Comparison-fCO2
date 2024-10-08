# GCB-Taylor-Comparison-fCO2
Performance comparison of surface ocean fCO2 and air-sea CO2 flux across four databases from the Global Carbon Budget, using Taylor diagrams to evaluate the performance of individual global ocean biogeochemical models and fCO2-based data products.

# The main functionalities implemented include:
1. Downloading data from the source databases to GCS and automatically saving it. If the path already exists, the data is loaded from the given path without re-downloading.
2. Cleaning and preprocessing the source data, including handling missing values, duplicates, outliers, and converting data into a unified format, with the output in zarr format. The code automatically detects the latest update date and includes it in the filename of the processed files. For example: HOT_spco2_202312.zarr.
3. Downloading all Model and Data Product data from Zenodo, cleaning and preprocessing it. Along with HPD and Residual data, the data is validated and relevant statistics are calculated across four databases. The output is a CSV file saved to GCS. In addition, we have the option to create these statistics for a subset of years.
4. Generating Taylor Diagrams based on the CSV files, including options for subplot versions with time periods displayed next to the vertical axis, customizable legend formats and colors, selection of partial or all models/products, the option to save the diagrams to the directory home, and combination of four databases’ figure.

# Notes and Instructions:
1. In the four processing notebooks, if there is an update to the database, you might need to modify the download URL and also update the file name at the end of the `gcs_path` based on the actual file name.

   - If you want to change the output save path or file name, search for 'zarr_gcs_path'.
   - If you want to delete a specific file in GCS, there is relevant code in the last cell of each processing notebook that you can use.

   Current version:
   - BATS: 1991 - 2023
   - HOT: 1988 - 2022
   - GLODAP: 1986 - 2021
   - LDEO: 1957 - 2019
2. In the Input_Taylor notebook, there are four functions—BATS_stats, HOT_stats, LDEO_stats, GLODAP_stats—for calculating statistics.
   
   - Within these functions, you can set the start year. You might need to hardcode the default start year and modify the numbers in the variables `start_yr = max(start_yr, 1991)` and `end_yr = min(end_yr, 2023)`. For example, you can leave the start year undefined to get full-version statistics, or you can set it to 1990-2000 to calculate statistics for that specific time period.
   
   - The URLs for the models and products are hardcoded and may need to be updated each year. After updating the download URLs, you also need to modify the file name at the end of the `save_path` based on the actual file name.

3. In Plot_Taylor notebook:

   - First, manually hard code the CSV file name of the version you need to read.
   
   - The function needs the following factors: `Taylor_locs(start_yr, end_yr, location, select_variables, figname=None, show_legend=True, show_year_range=True)`.
   
   - In `select_variables`, select which models/products you want to include in the Taylor diagram, and also define the color and transparency here.
   
   - To save the output Taylor Diagram, simply write `figname="save_name"` in the parentheses of `Taylor_locs()` to save it to the directory home.
   
   - In `Taylor_locs(start_yr, end_yr)`, manually input the corresponding years based on the start year of the `inputs.csv` file we are reading.
   
   - The location mapping corresponds to:
     - `1: BATS, 2: HOT, 3: LDEO, 4: GLODAP`
   
   - Set `mfc = 'None'` to have unfilled circles.
   
   - If you want to change the axis range, two ways:
     - Search for `srange` and edit. `Eg: srange=(0, 1.5)` means set the axis range as (0* refstd–1.5* refstd)
     - Search for `self.smax`, you could set `self.smax = 45` to have 45 as the upper bound.
