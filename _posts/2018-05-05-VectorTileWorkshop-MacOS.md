---
layout: post
title:  "Generating Vector Tiles using the Docker Containers for macOS"
categories:
  - Vector Tiles
last_modified_at: 2018-05-19
---

In this lesson we will learn to use Docker containers to create and serve vector tiles on your Macintosh laptop.
<!--more-->

## 1. Install Docker on macOS ##

+ Download [Docker Community Edition for Mac](https://store.docker.com/editions/community/docker-ce-desktop-mac)
+ Mount Docker.dmg
+ Drag Docker App onto Applications Shortcut
+ Launch Docker (double-click Docker Icon)
+ Agree to run application from Internet
+ Supply admin password when prompted

## 2. Install a GDAL/OGR tools container ##

+ Go to [Docker Hub](https://hub.docker.com/)
+ Search for gdal
+ Select [klokantech/gdal](https://hub.docker.com/r/klokantech/gdal/)
+ Copy the Docker Pull Command & run it at the Terminal prompt
  
    `docker pull klokantech/gdal`

## 3. Convert shape file into GeoJSON file ##
+ Download zipped files of [King County Census Blocks](https://drive.google.com/open?id=1tgXXA9rZaMXdLL-eqh0GnU4qon6QoRsI)
+ Place zip file into your local directory
+ Unzip zip file
+ Use OGR tools at the Terminal prompt
    + _ogrinfo_: check the shape file's information
  
        `docker run -ti --rm -v $(pwd):/data klokantech/gdal ogrinfo KingCo_2000_Census_BlockGroups.shp -al -so`
	  
    + _ogr2ogr_: convert shape file to GeoJSON file
  
        `docker run -ti --rm -v $(pwd):/data klokantech/gdal ogr2ogr -t_srs EPSG:4326 -f GeoJSON KingCo_2000_Census_BlockGroups.geojson KingCo_2000_Census_BlockGroups.shp -Progress`

## 4. Locate a geoJSON file ##
+ You can download the following GeoJSON file for next step if needed
    + [King County 2000 Census Block Groups GeoJSON](https://drive.google.com/open?id=1ofMZSOH34HIMNKqjo0w4H9qzzAukCKQg)

## 5. Install a Tippecanoe container which is a utility tool to create vector tiles ##
* Search for Tippecanoe on [Docker Hub](https://hub.docker.com/)
* Select the [jskeates/tippecanoe repository](https://hub.docker.com/r/jskeates/tippecanoe/)
* Copy the appropriate command from the *Docker Pull Command* section of the page
* Paste it at the Terminal prompt, and hit enter to run it

	`docker pull jskeates/tippecanoe`

## 6. Create some vector tiles ##
+ Ensure Docker is running on your computer
+ Start Tippecanoe container in interactive mode at the Terminal prompt

	`docker run -it -v $(pwd):/home/tippecanoe jskeates/tippecanoe:latest`

+ You will see your command prompt change to look like `bash-4.3$`
+ Use the tippecanoe command at the Terminal prompt to create vector tiles from the geoJSON file

	`tippecanoe -o KingCo_2000_Census_BlockGroups.mbtiles KingCo_2000_Census_BlockGroups.geojson`
	
+ Exit the container when it is done

	`exit`
	
+ The vector tiles will now be in your directory

## 7. Install a TileServer GL container ##
+ Go to [Docker Hub](https://hub.docker.com/)
+ Search for tileserver-gl
+ Select [klokantech/tileserver-gl](https://hub.docker.com/r/klokantech/tileserver-gl/)
+ Copy the Docker pull command & run it at the Terminal prompt
    `docker pull klokantech/tileserver-gl`

## 8. Run TileServer GL <a name='starttileserver'></a> ##
+ Ensure Docker is running on your computer
+ From the command line change into the directory where you have placed your mbtiles file
+ Start the TileServer GL container from the Terminal prompt

    `docker run --rm -it -v $(pwd):/data -p 8080:80 klokantech/tileserver-gl`

+ Test that the vector tiles are being servered by entering [http://localhost:8080/](http://localhost:8080) into your browser's address bar
+ After testing, hit __ctl-C__ to quit TileServer GL
