---
layout: post
title:  "Generating Vector Tiles using the Docker Containers for Windows 10 (Home)"
categories:
  - Vector Tiles
last_modified_at: 2018-05-19
---


In this lesson we will learn to use Docker containers to deploy various tools to create and serve vector tiles on your laptop running Windows Home Edition.
<!--more-->

__If you are running Windows 10 Pro or Window 10 Enterprise please use the [Windows 10 Pro instructions](/vector%20tiles/2018/05/06/VectorTileWorkshop-Windows10.html)__

## 1. Install Docker Toolbox ##
+ Download [Docker Toolbox for Windows](https://docs.docker.com/toolbox/toolbox_install_windows/)
+ Run  _DockerToolbox.exe_
    + Accept Defaults during install

## 2. Run the Quick Start Terminal ##
+ Double-click the  Quick Start Terminal installed by Docker Toolbox
    + You will execute all the docker commands in the steps below at the *Docker Quickstart Terminal* prompt
    + For help getting started, check out the docs at https://docs.docker.com

        `
Docker is up and running!
To see how to connect your Docker Client to the Docker Engine running on this virtual machine, run: C:\Program Files\Docker Toolbox\docker-machine.exe env default

                        ##         .
                  ## ## ##        ==
               ## ## ## ## ##    ===
           /"""""""""""""""""\___/ ===
      ~~~ {~~ ~~~~ ~~~ ~~~~ ~~~ ~ /  ===- ~~~
           \______ o           __/
             \    \         __/
              \____\_______/
`

+ Docker Toolbox is configured to present it's default machine as IP 192.168.99.100 to the browser running natively on your computer

## 3. Install a GDAL/OGR tools container ##
+ Go to [Docker Hub](https://hub.docker.com/)
+ Search for gdal
+ Select [klokantech/gdal](https://hub.docker.com/r/klokantech/gdal/)
+ Copy the Docker Pull Command & run it at Quick Start Terminal prompt

    `docker pull klokantech/gdal`

## 4. Convert Shape file into GeoJSON file ##
+ Download zipped files of [King County 2000 Census Block Groups](https://drive.google.com/file/d/1FfLKbGalJnULsJo1fjzOhwtO4wVYHvoq)
+ Place zip file into your local user directory (eg: C:\Users\mccombsp)
    + "mccombsp" will be replaced with your local user name
+ Unzip zip file. Be sure that the files are in your local user directory, not in a subdirectory.
+ Use OGR tools at the Quick Start Terminal prompt
    + _ogrinfo_: check the shape file's information

        `docker run -it --rm -v $HOME:/data klokantech/gdal ogrinfo KingCo_2000_Census_BlockGroups.shp -al -so`

    + _ogr2ogr_: convert shape file to GeoJSON file

        `docker run -it --rm -v $HOME:/data klokantech/gdal ogr2ogr -t_srs EPSG:4326 -f GeoJSON KingCo_2000_Census_BlockGroups.geojson KingCo_2000_Census_BlockGroups.shp -Progress`

## 5. Locate a GeoJSON file ##
+ You can download the following GeoJSON file for next step if needed
    + [King County 2000 Census Block Groups GeoJSON](https://drive.google.com/open?id=1ofMZSOH34HIMNKqjo0w4H9qzzAukCKQg)

## 6. Install a Tippecanoe container which is a utility tool to create vector tiles ##
* Search for Tippecanoe on [Docker Hub](https://hub.docker.com/)
* Select the [jskeates/tippecanoe repository](https://hub.docker.com/r/jskeates/tippecanoe/)
* Copy the appropriate command from the *Docker Pull Command* section of the page
* Paste it at the Quick Start Terminal prompt, and hit enter to run it

	`docker pull jskeates/tippecanoe`

## 7. Create some vector tiles ##
+ Ensure Docker is running on your computer
+ Start Tippecanoe container in interactive mode at the Quick Start Terminal prompt

	`docker run -it -v $HOME:/home/tippecanoe jskeates/tippecanoe:latest`

+ You will see your command prompt change to look like `bash-4.3$`
+ Use the tippecanoe command at the Terminal prompt to create vector tiles from the geoJSON file

	`tippecanoe -o KingCo_2000_Census_BlockGroups.mbtiles KingCo_2000_Census_BlockGroups.geojson`

+ Exit the container when it is done

	`exit`

+ The vector tiles will be $HOME/KingCo_2000_Census_BlockGroups.mbtiles
    + $HOME represents your user directory at a unix style command prompt.

## 8. Install a TileServer GL container ##
+ Go to [Docker Hub](https://hub.docker.com/)
+ Search for tileserver-gl
+ Select [klokantech/tileserver-gl](https://hub.docker.com/r/klokantech/tileserver-gl/)
+ Copy the Docker pull command & run it at the Quick Start Terminal prompt

    `docker pull klokantech/tileserver-gl`

## 9. Run TileServer GL <a name='starttileserver'></a> ##
+ Ensure Docker is running on your computer
+ From the command line change into the directory `cd $HOME` where you have placed your mbtiles file

+ Start the TileServer GL container from the Quick Start Terminal prompt

    `docker run --rm -it -v $(pwd):/data -p 8080:80 klokantech/tileserver-gl`

+ Test that the vector tiles are being served by entering [http://192.168.99.100:8080/](http://192.168.99.100:8080) into your browser's address bar

+ After testing, hit __ctl-C__ to quit TileServer GL
