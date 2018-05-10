---
layout: post
title:  "Generating Vector Tiles using the Docker Containers for Windows 10 (Home)"
categories:
  - Vector Tiles
last_modified_at: 2018-05-09
---


In this lesson we will learn to use Docker containers to deploy various tools to create and serve vector tiles on your local machine.
<!--more-->

__If you are running Windows 10 Pro or Window 10 Enterprise please use the [Windows 10 Pro instructions](/vector%20tiles/2018/05/06/VectorTileWorkshop-Windows10.html)__

## 1. Install Docker Toolbox ##
+ Download [Docker Toolbox for Windows](https://docs.docker.com/toolbox/toolbox_install_windows/)
+ Run  _DockerToolbox.exe_ 
    + Accept Defaults during install
	
## 2. Run the Quick Start Terminal ##
+ Double-click the  Quick Start Terminal installed by Docker Toolbox
    + You will execute all the docker commands in the steps below at the *Quick Start Terminal* prompt
    + For help getting started, check out the docs at https://docs.docker.com

        ```
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
```

+ Docker Toolbox is configured to present it's default machine as IP 192.168.99.100 to the browser running natively on your computer

## 3. Install a GDAL/OGR tools container ##
+ Go to [Docker Hub](https://hub.docker.com/)
+ Search for gdal
+ Select [klokantech/gdal](https://hub.docker.com/r/klokantech/gdal/)
+ Copy the Docker Pull Command & run it at Quick Start Terminal prompt
  
    `docker pull klokantech/gdal`
	
## 4. Convert Shape file into GeoJSON file ##
+ Download zipped files of [King County 2000 Census Block Groups](https://drive.google.com/open?id=1UKC5AZYtN1tId1jqORPmBO90LetsYJ9C)
+ Place zip file into your local user directory (eg: C:\Users\mccombsp)
    + "mccombsp" will be replaced with your local user name
+ Unzip zip file
+ Use OGR tools at the Quick Start Terminal prompt
    + _ogrinfo_: check the shape file's information

        `docker run -it --rm -v $HOME:/data klokantech/gdal ogrinfo KingCo_2000_Census_BlockGroups.shp -al -so`


    + _ogr2ogr_: convert shape file to GeoJSON file
 
        `docker run -it --rm -v C:\Users\keump:/data klokantech/gdal ogr2ogr -t_srs EPSG:4326 -f GeoJSON KingCo_2000_Census_BlockGroups.geojson KingCo_2000_Census_BlockGroups.shp -Progress`






1. docker pull jskeates/tippecanoe

randre@Agecanonix MINGW64 /c/Program Files/Docker Toolbox
$ docker pull jskeates/tippecanoe
Using default tag: latest
latest: Pulling from jskeates/tippecanoe
Digest: sha256:cdaa7f2dce4df0e340687b79febb6f983baa92d4a15ba0486441b78c40c07d3a
Status: Image is up to date for jskeates/tippecanoe:latest

3. ran "docker run -it -v $HOME/tippecanoe:/home/tippecanoe jskeates/tippecanoe:latest" in Docker Quickstart terminal

4. Downloaded data file from https://raw.githubusercontent.com/pkgeo-org/pkgeo-org.github.io/master/tippecanoe/usStates.geojson
(or for Census blockgroup data) https://drive.google.com/open?id=1tgXXA9rZaMXdLL-eqh0GnU4qon6QoRsI
Census_2000_Block_Groups__blkgrp00_area.shp*

5. Convert Census shapefile to geojson

$ docker run -it -v $HOME/tippecanoe:/home/tippecanoe klokantech/gdal ogr2ogr /home/tippecanoe/Census_block.geojson -t_srs EPSG:4326 -f GeoJSON  /home/tippecanoe/Census_2000_Block_Groups__blkgrp00_area.shp -Progress

4. Now in Docker terminal, I can see the geojson:
ash-4.3$ ls -l *.geojson
-rwxrwxrwx    1 tippecan 50        33624667 Apr 17 23:51 Census_block.geojson
-rwxrwxrwx    1 tippecan 50        86111601 Mar  3 15:32 europe_admin0.geojson
-rwxrwxrwx    1 tippecan 50          772518 Mar 30 00:03 usStates.geojson

4.5 Enter tippecanoe Docker instance:
 docker run -it -v $HOME/tippecanoe:/home/tippecanoe jskeates/tippecanoe:latest

5. Use tippecanoe instance to create mbtiles:
For U.S. States:
  tippecanoe -o states.mbtiles usStates.geojson
For Census:
   tippecanoe -o census_blocks.mbtiles Census_block.geojson

- Exit docker instance:
$ exit

- In Docker quickstart term:

6. docker pull klokantech/tileserver-gl

7. Change dir into windows tipecanoe dir

$ cd c/Users/randre/tippecanoe
or
$ cd $HOME/tippecanoe

8. docker run --rm -it -v $(pwd):/data -p 8080:80 klokantech/tileserver-gl

9. After which you can access the service locally at:

http://192.168.99.100:8080/

(See IP listed at top after Docker install)
