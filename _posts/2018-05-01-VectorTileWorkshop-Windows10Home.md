---
layout: post
title:  "Generating Vector Tiles using the Docker Containers for Windows 10 (Home)"
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

__If you are running Windows 10 Pro or Window 10 Enterprise please use the [Windows 10 Pro instructions](/vector%20tiles/markup/2018/04/26/VectorTileWorkshop-Windows10.html)__

# Roger's Full Notes #

```
Install Docker Toolbox:
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

docker is configured to use the default machine with IP 192.168.99.100
For help getting started, check out the docs at https://docs.docker.com


- Start docker interactive shell

1. docker pull jskeates/tippecanoe

randre@Agecanonix MINGW64 /c/Program Files/Docker Toolbox
$ docker pull jskeates/tippecanoe
Using default tag: latest
latest: Pulling from jskeates/tippecanoe
Digest: sha256:cdaa7f2dce4df0e340687b79febb6f983baa92d4a15ba0486441b78c40c07d3a
Status: Image is up to date for jskeates/tippecanoe:latest

1.5 Download GDAL docker instance
docker pull klokantech/gdal

2. Created C:\Users\randre\tippecanoe

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

---

How to install Maputnik on windows:
https://github.com/maputnik/editor/releases/tag/v1.0.2

Maputnik:

http://192.168.99.100:8080/data/census_blocks.json

# End Roger's Full Notes #

## 1). Install Docker on Windows 10 Edition
* Docker Community Edition for Windows [Here](https://docs.docker.com/docker-for-windows/install/#download-docker-for-windows)
	1. __Requires Windows 10 Pro  or Enterprise__
	2.  Open Power Shell Command Terminal as Adminstrator.
	3.  Need to turn on Hyper-V which is hardware virtualization support by type in following command at command prompt

`Enable-WindowsOptionalFeature -Online -FeatureName Microsoft-Hyper-V –All`


## Install Docker on Windows 10 Home Edition
* __Does Not Work with Windows 10 Home__
* Approaches to work arround this:
	1.	Can be installed on [Windows Subsystem for Linux](https://docs.microsoft.com/en-us/windows/wsl/install-win10)
        + Can be installed on a virtual machine running linux
        + [Docker Toolbox](https://docs.docker.com/toolbox/toolbox_install_windows/)
            + Accept Defaults during install
            + Run the Quick Start Terminal
            + Execute the steps at the Quick Start Terminal

## 2). Install a GDAL/OGR tools container
+ Go to [Docker Hub](https://hub.docker.com/)
+ Search for gdal
+ Select [klokantech/gdal](https://hub.docker.com/r/klokantech/gdal/)
+ Copy the Docker Pull Command & run it at command line `docker pull klokantech/gdal`


## 3). Convert Shape file into GeoJSON file
1. Download Zipped files of King County Censcus Block [Here](https://drive.google.com/open?id=1tgXXA9rZaMXdLL-eqh0GnU4qon6QoRsI)
2. Download into your local directory
3. Run OGR tool to convert shape file into geojson


* Checking shape file information `docker run -ti --rm -v $(pwd):/data klokantech/gdal ogrinfo PugetSound.shp -al -so` (need to change to work in Windows environment and correct file name)

* Converting shape file into GeoJSON file `docker run -ti --rm -v $(pwd):/data klokantech/gdal ogr2ogr -t_srs EPSG:4326 -f GeoJSON PS_test.geojson PugetSound.shp -Progress`(need to change to work in Windows enviro and data path and correct file name)

## 4). Install a Tippecanoe container which is a utility tool to create vector tiles
1.	Search for Tippecanoe on [Docker Hub](https://hub.docker.com/).
+ Select the [jskeates/tippecanoe repository](https://hub.docker.com/r/jskeates/tippecanoe/)
+ Copy the appropriate command from the *Docker Pull Command* section of the page, then paste it at the terminal prompt and hit enter to run it.
	`docker pull jskeates/tippecanoe`

## Locate a GeoJSON file
1. Down load KC Census Geojson data [Here](https://drive.google.com/file/d/1ofMZSOH34HIMNKqjo0w4H9qzzAukCKQg/view?usp=sharing)
2. Created C:\Users\randre\tippecanoe
+ [usStates.geojson](https://github.com/pkgeo-org/pkgeo-org.github.io/tree/master/tippecanoe) - figure out how to download the file
+ Move to $HOME/tippecanoe

## Create some vector tiles
+ Mac: Be sure Docker is running on your computer
+ Windows: Be in Docker Quick Start Terminal
+ Start Tippecanoe container in interactive mode: `docker run -it -v $HOME/tippecanoe:/home/tippecanoe jskeates/tippecanoe:latest`
* Issue tippecanoe command: `tippecanoe -o states.mbtiles usStates.geojson`
+ Exit the container when it is done. The vector tiles will be in a new folder: $HOME/tippecanoe/states.mbtiles.
    + $HOME is replaced with your user directory at a unix style command promt. This works for MacOS, Docker Toolbox, or Linux, but probably not for Windows

## Install a TileServer GL Container
+ Go to [Docker Hub](https://hub.docker.com/)
+ Search for tileserver-gl
+ Select [klokantech/tileserver-gl](https://hub.docker.com/r/klokantech/tileserver-gl/)
+ Copy the Docker Pull Command & run it at command line `docker pull klokantech/tileserver-gl`

## Run TileServer GL
+ Be sure Docker is running on your computer
+ From the command line change into the tippecanoe directory - *This is where you have placed your geoJSON file* `cd $HOME/tippecanoe`
+ Start the TileServer GL container from the command line`docker run --rm -it -v $(pwd):/data -p 8080:80 klokantech/tileserver-gl`
+ Mac: http://localhost:8080
+ Windows: http://192.168.99.100:8080
    + The specific IP address will be listed near the whale when you start the Quick Start Terminal. The above address is the default.
+ ctl-C to quit TileServer GL



## Document Installing, using, and publishing data using Maputnik for styling ##
