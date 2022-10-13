## Repository Overview

Through our work at the Texas Advanced Computing Center (TACC) over the past two years, we have learned ways to use computer science, artificial intelligence (AI) and machine learning to improve the lives of others on a large scale. Flooding in Austin has led to families paying large amounts to repair damages or even losing their houses, and we are exploring how data science and AI approaches may improve mitigation measures to protect people and their homes. In this repository, we are tracking 311 flooding calls in the Austin area. By examining the amount of precipitation leading up to the call, we predict 311 flooding calls throughout the city based on projected precipitation. This allows residents and city officials to be prepared for potential flooding and alleviate damages before they happen. 

The scripts should be run in this order:
- [311-Data-Cleaning.ipynb](311-Data-Cleaning.ipynb)
- [Background-Layers.ipynb](Background-Layers.ipynb)
- [Rainfall_Scrape-copy.ipynb](Rainfall_Scrape-copy.ipynb)
- [VRTs-Creation.ipynb](VRTs-Creation.ipynb)
- [Model-Creation.ipynb](Model-Creation.ipynb)

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

From here, you can start running through the code cells in the 311-Data-Cleaning.ipynb file. To do so, make sure you have the KATT, KAUS and KEDC [data](https://www.texmesonet.org/DataProducts/CustomDownloads).  From there type in the code for the station from which are pulling data. Next put in your start date and end date.  
- Start Date: 1/1/2014 
- End Date: 8/5/2021
*Note that you can only download one year at a time so you will have to download multiple CSVs*.

Once you have the name of the station and the dates in, click on Get Data. This will give you a CSV that you can import into your preferred environment and folder.
To run the [311-Data-Cleaning.ipynb](311-Data-Cleaning.ipynb) file, you will also need the [data](https://data.austintexas.gov/Locations-and-Maps/BOUNDARIES_jurisdictions/3pzb-6mbr) in the Austin folder. 

Select :
	`export -> shapefile `

This will give you a zipfile you can then unzip and import into your desired environment and folder.

You will also need the [Roadways Data](https://gis-txdot.opendata.arcgis.com/datasets/TXDOT::txdot-roadways/explore?location=31.008846%2C-100.055172%2C6.57). Select download and then shapefile once again

Once you have all of these files and folders, you should be able to run through all the cells in the 311.ipynb file. This file should conclude with the creation of the confirmed_floods.csv file.

## Get non-flooded datset by scraping the background structures in the region

Now, you can move on to the Background-Layers.ipynb file. To run this file you will need the Texas.geojson [file](https://github.com/microsoft/USBuildingFootprints). Click on Texas, this will download a zip file from which you can pull the geojson file to import it into your desired environment and folder. You will also need the wbdhu12_a_us_september2020-copy.gdb [folder](https://www.sciencebase.gov/catalog/item/61f8b8edd34e622189c3293f). Follow instructions to get the data. 

Once you have these two things, you can run through the cells in the [Background-Layers.ipynb](Background-Layers.ipynb) file. This file should conclude with the creation of the background_locations.csv file.

## Get GRIB2 files necessary to set up model 

Next, open the [Rainfall_Scrape-copy.ipynb](Rainfall_Scrape-copy.ipynb) file. Run through the code cells in this file. This should conclude with the creation of the GRIB2 files, as well as the creation of an hours.txt file.

## Get VRTs necessary to create model

Next, you will have to run through the [VRTs-Creation.ipynb](VRTs-Creation.ipynb) file. Before you can do this, you must create a folder called VRTs inside your desired folder. This should conclude with the creation of the VRTs in the VRT Folder.

## Finally, create and save your model and check results

There is one last ipynb file to run through. However, before we can do that, you have to go back to the terminal and copy and paste the following lines 8-37. 

```
pip install seaborn
sudo mkdir /mnt/corral-sync
if [ ! -d "/etc/smbcredentials" ]; then
sudo mkdir /etc/smbcredentials
fi
if [ ! -f "/etc/smbcredentials/tdiscorral.cred" ]; then
    sudo bash -c 'echo "username=tdiscorral" >> /etc/smbcredentials/tdiscorral.cred'
    sudo bash -c 'echo "password=2/4N4Y4PhBliAxvqhs74n8Y684gPiRsBZH27NKSLYmZYRPGCxlGr9XljNzLbQ8xJP2MOtXzt2Mtn/HseXwLObw==" >> /etc/smbcredentials/tdiscorral.cred'
fi
sudo chmod 600 /etc/smbcredentials/tdiscorral.cred

sudo bash -c 'echo "//tdiscorral.file.core.windows.net/corral-sync /mnt/corral-sync cifs nofail,vers=3.0,credentials=/etc/smbcredentials/tdiscorral.cred,dir_mode=0777,file_mode=0777,serverino" >> /etc/fstab'
sudo mount -t cifs //tdiscorral.file.core.windows.net/corral-sync /mnt/corral-sync -o vers=3.0,credentials=/etc/smbcredentials/tdiscorral.cred,dir_mode=0777,file_mode=0777,serverino
```

After that, you must convert the GRIB2 files to tif files. Before you can do so, you must create a folder named Exported_Tif_Files. You also have to download the wct-4.6.1-copy [folder](https://www.ncdc.noaa.gov/wct/install.php). Download the correct zip given your environment. Unzip the file to create the folder. Pull the [wctBacthConfig-copy.xml](wctBacthConfig-copy.xml) out of the folder. Once you have done that, go back to the terminal and run the following line:

```
bash wct-4.6.1/wct-export.sh "311-Publish/GRIB2_Files2/" "311-Publish/Exported_Tif_Files/" tif  "311-Publish/wctBatchConfig-copy.xml"
```

Finally, create another folder named 'reproject' inside of the Exported_Tif_Files. 

Once you have done all this you can run the cells in the [Model-Creation.ipynb](Model-Creation.ipynb). This should end with the creation and saving of your model.
