## Scraping 311 call data from datbase and rainfall data from tracking sites.

In order to create the Flooding.csv file, visit [Austin 311](https://data.austintexas.gov/Utilities-and-City-Services/Austin-311-Public-Data/xwdj-i9he/ ) and select view data. Once there, type 'Flooding' into the search bar. This will give you all of the 311 calls relating to flooding. Hit export to download the file onto your computer and select CSV as the file type. Once you have the file on your computer you can import it into your preferred environment and folder.

## Create Environment
Before you start reading in data from the CSV, you must create and activate the GDAL environment. To do so, start a compute, and open your terminal and copy and paste the following lines.

```
conda env create -f environment.yml
conda activate gdal
python -m ipykernel install --user --name 
gdal  --display-name "GDAL"
```

You will need to have the environment.yml file for this code to work in the terminal. Then, open up the 311.ipynb file and click on the drop down menu in the top right corner. Select GDAL from the list. 

From here, you can start running through the code cells in the 311.ipynb file. To do so, make sure you have the KATT, KAUS and KEDC [data](https://www.texmesonet.org/DataProducts/CustomDownloads).  From there type in the code for the station from which are pulling data. Next put in your start date and end date.  
- Start Date: 1/1/2014 
- End Date: 8/5/2021
*Note that you can only download one year at a time so you will have to download multiple CSVs*.

Once you have the name of the station and the dates in, click on Get Data. This will give you a CSV that you can import into your preferred environment and folder.
To run the [311.ipynb](311.ipynb) file, you will also need the [data](https://data.austintexas.gov/Locations-and-Maps/BOUNDARIES_jurisdictions/3pzb-6mbr) in the Austin folder. 

Select :
	`export -> shapefile `

This will give you a zipfile you can then unzip and import into your desired environment and folder.

You will also need the [Roadways Data](https://gis-txdot.opendata.arcgis.com/datasets/TXDOT::txdot-roadways/explore?location=31.008846%2C-100.055172%2C6.57). Select download and then shapefile once again

Once you have all of these files and folders, you should be able to run through all the cells in the 311.ipynb file. This file should conclude with the creation of the confirmed_floods.csv file.

## Get non-flooded datset by scraping the background structures in the region

Now, you can move on to the Layers.ipynb file. To run this file you will need the Texas.geojson file. To download this file, go to 'https://github.com/microsoft/USBuildingFootprints' and click on Texas. This will download a zip file from which you can pull the geojson file to import it into your desired environment and folder. You will also need the wbdhu12_a_us_september2020-copy.gdb folder. To get this folder, go to this link: https://www.sciencebase.gov/catalog/item/61f8b8edd34e622189c3293f and follow instructions to get the data. 

Once you have these two things, you can run through the cells in the Layers.ipynb file. This file should conclude with the creation of the background_locations.csv file.

Get GRIB2 files necessary to set up model 

    Next, open the Rainfall_Scrape-copy.ipynb file. Run through the code cells in this file. This should conclude with the creation of the GRIB2 files, as well as the creation of an hours.txt file.

Get VRTs necessary to create model

Next, you will have to run through the VRTs-copy.ipynb file. Before you can do this, you must create a folder called VRTs inside your desired folder. This should conclude with the creation of the VRTs in the VRT Folder.

Finally, create and save your model and check results

There is one last ipynb file to run through. However, before we can do that, you have to copy lines 8-37 from the StartupCode-copy.txt file and paste them into the terminal. After that, you must convert the GRIB2 files to tif files. Before you can do so, you must create a folder named Exported_Tif_Files. You also have to download the wct-4.6.1-copy folder. To do so, go to this link https://www.ncdc.noaa.gov/wct/install.php and download the correct zip given your environment. Unzip the file to create the folder. Pull the wctBacthConfig-copy.xml out of the folder. Once you have done that, run the final line of the StartupCode-copy.txt file in the terminal. Finally, create another folder named 'reproject' inside of the Exported_Tif_Files. 

Once you have done all this you can run the cells in thr HazardEstimates.ipynb. This should end with the creation and saving of your model.
Get non-flooded datset by scraping the background structures in the region

    Now, you can move on to the Layers.ipynb file. To run this file you will need the Texas.geojson file. To download this file, go to 'https://github.com/microsoft/USBuildingFootprints' and click on Texas. This will download a zip file from which you can pull the geojson file to import it into your desired environment and folder. You will also need the wbdhu12_a_us_september2020-copy.gdb folder. To get this folder, go to this link: https://www.sciencebase.gov/catalog/item/61f8b8edd34e622189c3293f and follow instructions to get the data. 
    Once you have these two things, you can run through the cells in the Layers.ipynb file. This file should conclude with the creation of the background_locations.csv file.

Get GRIB2 files necessary to set up model 

    Next, open the Rainfall_Scrape-copy.ipynb file. Run through the code cells in this file. This should conclude with the creation of the GRIB2 files, as well as the creation of an hours.txt file.

Get VRTs necessary to create model

    Next, you will have to run through the VRTs-copy.ipynb file. Before you can do this, you must create a folder called VRTs inside your desired folder. This should conclude with the creation of the VRTs in the VRT Folder.

Finally, create and save your model and check results

    There is one last ipynb file to run through. However, before we can do that, you have to copy lines 8-37 from the StartupCode-copy.txt file and paste them into the terminal. After that, you must convert the GRIB2 files to tif files. Before you can do so, you must create a folder named Exported_Tif_Files. You also have to download the wct-4.6.1-copy folder. To do so, go to this link https://www.ncdc.noaa.gov/wct/install.php and download the correct zip given your environment. Unzip the file to create the folder. Pull the wctBacthConfig-copy.xml out of the folder. Once you have done that, run the final line of the StartupCode-copy.txt file in the terminal. Finally, create another folder named 'reproject' inside of the Exported_Tif_Files. 
    Once you have done all this you can run the cells in thr HazardEstimates.ipynb. This should end with the creation and saving of your model.