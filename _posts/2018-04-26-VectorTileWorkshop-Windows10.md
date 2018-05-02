---
layout: post
title:  "Generating Vector Tiles using the Docker Containers for Windows 10 (Pro and Enterprise)"
categories:
  - Vector Tiles
  - Markup
tags:
  - Post Formats
  - readability
  - standard
last_modified_at: 2018-05-01
---


In this lesson we will learn to use Docker containers to deploy various tools to create and serve vector tiles on your local machine.
<!--more-->

__If you are running Windows 10 Home please use the [Windows 10 Home instructions](/vector%20tiles/markup/2018/05/01/VectorTileWorkshop-Windows10Home.html)__

## 1. Enable Hyper-V in Windows ##
+ __Requires Windows 10 Pro  or Enterprise__
+ [Hyper-V](https://docs.microsoft.com/en-us/virtualization/hyper-v-on-windows/about/) provides hardware virtualization support in Windows
+ Open a PowerShell terminal window as Adminstrator
+ To enable Hyper-V enter the following command at the Power Shell prompt
  
  `Enable-WindowsOptionalFeature -Online -FeatureName Microsoft-Hyper-V –All`

## 2. Install Docker on Windows 10 ##
+ __Requires Windows 10 Pro  or Enterprise__
+ Download [Docker Community Edition for Windows](https://docs.docker.com/docker-for-windows/install/#download-docker-for-windows)
+ Double Click the Docker for Windows Installer.exe file
  + Follow the instructions
  + You will be asked to provide your password durring installation
+ Start Docker for Windows from the Start Menu

## 3. Install a GDAL/OGR tools container ##
+ Go to [Docker Hub](https://hub.docker.com/)
+ Search for gdal
+ Select [klokantech/gdal](https://hub.docker.com/r/klokantech/gdal/)
+ Copy the Docker Pull Command & run it at PoweShell prompt
  
  `docker pull klokantech/gdal`

## 4. Convert Shape file into GeoJSON file ##
+ Download zipped files of [King County Census Blocks](https://drive.google.com/open?id=1tgXXA9rZaMXdLL-eqh0GnU4qon6QoRsI)
+ Place zip file into your local directory
+ Unzip zip file
+ Use OGR tools at the PowerShell prompt
  + _ogrinfo_: check the shape file's information

    `docker run -ti --rm -v $(pwd):/data klokantech/gdal ogrinfo PugetSound.shp -al -so`
  
    __(need to change to work in Windows environment and correct file name)__

   + _ogr2ogr_: convert shape file to GeoJSON file
  
	  `docker run -ti --rm -v $(pwd):/data klokantech/gdal ogr2ogr -t_srs EPSG:4326 -f GeoJSON PS_test.geojson PugetSound.shp -Progress`

     __(need to change to work in Windows environment and correct file name)__
 
## 5. Install a Tippecanoe container which is a utility tool to create vector tiles ##
* Search for Tippecanoe on [Docker Hub](https://hub.docker.com/)
* Select the [jskeates/tippecanoe repository](https://hub.docker.com/r/jskeates/tippecanoe/)
* Copy the appropriate command from the *Docker Pull Command* section of the page
* Paste it at the Terminal prompt, and hit enter to run it

	`docker pull jskeates/tippecanoe`

## 6. Locate a geoJSON file ##
+ Download one or both of the following geoJSON files:
  + [King County census geoJSON data](https://drive.google.com/file/d/1ofMZSOH34HIMNKqjo0w4H9qzzAukCKQg/view?usp=sharing)
  + [United States boundary geoJSON data](https://raw.githubusercontent.com/pkgeo-org/jekyll-site-code/master/tippecanoe/usStates.geojson)
	+ Use _File->Save Page As_ or similar command from your browser to save on your computer
	+ Be sure to save the file as _usStates.geojson_
+ If neccesary, move the downloaded files into your local tippecanoe subdirectory

## 7. Create some vector tiles ##
+ Ensure Docker is running on your computer
+ Start Tippecanoe container in interactive mode at the Terminal prompt

	`docker run -it -v $HOME/tippecanoe:/home/tippecanoe jskeates/tippecanoe:latest`

  __(need to change to work in Windows environment and correct file name)__

+ You will see your command prompt change to look like `bash-4.3$`
+ Use the tippecanoe command at the Terminal prompt to create vector tiles from the geoJSON file

	`tippecanoe -o states.mbtiles usStates.geojson`
	
+ Exit the container when it is done

	`exit`
	
+ The vector tiles will be in a new folder: $HOME/tippecanoe/states.mbtiles.
  + $HOME is replaced with your user directory at a unix style command promt.

  __(need to change to work in Windows environment and correct file name)__
  
## 8. Install a TileServer GL container ##
+ Go to [Docker Hub](https://hub.docker.com/)
+ Search for tileserver-gl
+ Select [klokantech/tileserver-gl](https://hub.docker.com/r/klokantech/tileserver-gl/)
+ Copy the Docker pull command & run it at the Terminal prompt
  `docker pull klokantech/tileserver-gl`

## 9. Run TileServer GL ##
+ Ensure Docker is running on your computer
+ From the command line change into the tippecanoe directory
  + *This is where you should have placed your geoJSON file*
  
  `cd $HOME/tippecanoe`

  __(need to change to work in Windows environment and correct file name)__
  
+ Start the TileServer GL container from the command lin
  `docker run --rm -it -v $(pwd):/data -p 8080:80 klokantech/tileserver-gl`
  
  __(need to change to work in Windows environment and correct file name)__
  
+ Test that the vector tiles are being servered by entering [http://localhost:8080/](http://localhost:8080) into your browser's address bar
  __(need to test if localhost works in the Windows environment)__
+ After testing, hit ctl-C to quit TileServer GL

# TO DO: Document Installing, using, and publishing data using Maputnik for styling #
### From Roger's Notes ###
How to install Maputnik on windows:
https://github.com/maputnik/editor/releases/tag/v1.0.2

Maputnik:

http://192.168.99.100:8080/data/census_blocks.json
