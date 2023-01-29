```
  ____   _                    _    ____         _         _           __        __     _     
 / ___| | |__    ___    ___  | |_ |  _ \  ___  (_) _ __  | |_  ___    \ \      / /___ | |__  
 \___ \ | '_ \  / _ \  / _ \ | __|| |_) |/ _ \ | || '_ \ | __|/ __| __ \ \ /\ / // _ \| '_ \ 
  ___) || | | || (_) || (_) || |_ |  __/| (_) || || | | || |_ \__ \ __  \ V  V /|  __/| |_) |
 |____/ |_| |_| \___/  \___/  \__||_|    \___/ |_||_| |_| \__||___/      \_/\_/  \___||_.__/ 
```


# Overview
ShootPoints-Web is a set of programs for total station surveying. Based on the [SiteMap](https://www.academia.edu/78544111/SiteMap_Innovations_in_Computer_Based_Mapping_for_Archaeologists) surveying package originally developed at the Museum Applied Science Center for Archaeology (MASCA) at the University of Pennsylvania Museum, ShootPoints-Web streamlines and simplifies total station operation and data collection on archaeological excavations. Developed independently, it was heavily tested and refined at the Penn Museum projects at Ur and [Lagash](https://web.sas.upenn.edu/lagash/), Iraq.

ShootPoints-Web consists of two interrelated projects: shootpoints-web-api and shootpoints-web-frontend. Though shootpoints-web-api can be run from a laptop connected directly to the total station via a serial cable, it is intended to be installed on a dedicated Raspberry Pi mounted on the tripod of the total station, and controlled wirelessly via shootpoints-web-frontend in a web browser running on a smartphone or tablet on a local wifi network. Instructions for basic installation are provided below and instructions for creating a headless Raspberry Pi ShootPoints-Web server are forthcoming.

[See ShootPoints-Web in Action.](https://youtu.be/kiFEff1Kne4)


# Why use ShootPoints-Web?
Most surveying software, whether running on the total station itself or on an external data collector, is made for surveyors, not archaeologists and is often overly-complicated. Though the tools are the same, archaeologists’ needs are different than general land surveying, so ShootPoints-Web is designed for them. Its core principles are:

* **ShootPoints-Web records _archaeological_ data.**  
Archaeologists need extensive metadata for their surveying points. ShootPoints-Web structures your workflow to facilitate that while cutting out the cruft.  

* **ShootPoints-Web is easy to learn.**  
Tired of the typical on-site bottleneck of waiting for the one person who knows how to use your total station to be free so you can take your point elevations? ShootPoints-Web can be learned quickly by multiple team members, eliminating that bottleneck.  

* **ShootPoints-Web is easy to use on-site.**  
Setting up the total station should be the hardest part of surveying. Once that’s done, the rest of the job should be point-and-shoot. ShootPoints-Web imposes a workflow that reduces confusion and improves the quality of your surveying data.  

* **ShootPoints-Web is inexpensive.**  
ShootPoints-Web is open source and runs well on inexpensive computers and older total stations. Data collection is triggered from any excavator’s personal cellphone via a webapp. You can have a fully-functioning and high-quality surveying solution for about $1000.


# Requirements
shootpoints-web-api is written for Python 3.10 and later. Python versions as early as 3.8 may work, but have not been tested.

ShootPoints-Web’s processing and storage requirements are minimal, and it runs well on Raspberry Pi 2 Model B and better SBCs.

Serial communications protocols have only been created for Topcon GTS-300 series total stations, but ShootPoint Web’s modular design means that other makes and models of total station will be added in the future.

shootpoints-web-api requires the following third-party Python packages, installation instructions for which are provided below:
* [FastAPI](https://fastapi.tiangolo.com)
* [pySerial](https://github.com/pyserial/pyserial)
* [PyShp](https://github.com/GeospatialPython/pyshp)
* [Python-Multipart](https://github.com/andrew-d/python-multipart)
* [utm](https://github.com/Turbo87/utm)
* [Uvicorn](https://www.uvicorn.org)

# Installation
## Clone ShootPoints-Web into your project directory:
```bash
git clone --recurse-submodules https://github.com/Lugal-PCZ/ShootPoints-Web.git
```

## Install the required Python packages:
```bash
pip3 install -r <path/to/>ShootPoints-Web/shootpoints-web-api/requirements.txt
```

# Quick Start
These instructions presume that you will be installing ShootPoints-Web on a laptop or desktop computer for initial testing purposes. Instructions for installing ShootPoints-Web on a Raspberry Pi for fieldwork are forthcoming.

By default, shootpoints-web-api will launch in “demo” mode with no serial connection and simulated shot data so you can familiarize yourself with the software without a total station. This, however, can be easily overridden for testing with an actual live connection to a supported total station.

## Data Management and Categorization
Following the model developed for SiteMap, ShootPoints-Web categorizes shot data to simplify its visualization and interpretation. The two primary categorizations are groupings (collections of related points) and class/subclass (archaeological metadata about the shots). All data are saved to a local database which can be downloaded *in toto* or exported as shapefiles via the web interface for easy inclusion in your project’s GIS.

ShootPoints-Web will not let you begin collecting data without the proper prerequisites (site, station coordinates, and surveying session).

### Geometries
Every measurement taken with ShootPoints-Web is part of a grouping, which can be any of the following four geometries:
* **Isolated Point**: A discrete point that encapsulates granular information such as a point elevation or the location of a small artifact.
* **Point Cloud**: Multiple non-sequential point samples that do not carry information individually but as elements of a group that together describe an entity (such as topography).
* **Open Polygon**: Multiple sequential points that trace an outline wherein the start and end points do not connect.
* **Closed Polygon**: Multiple sequential points that trace an outline wherein the start point is connected to the end point.

### Classes and Subclasses
Each grouping also is assigned a class and subclass to assist in categorization and visualization of the data collected. The following classes and subclasses are populated in the ShootPoints database with a fresh install, but new ones can be added and removed under the “Setup” section, as is appropriate for your site.
* **Architecture**: Human-built structures.
  * **Floor**: Prepared surfaces upon which human activities took place.
  * **Wall**: Vertical, human-made, constructions that enclose, divide, or delimit space.
* **Artifact**: Objects made, modified, or used by people.
* **Feature**: Natural formations or immovable, non-architectural, human creations.
  * **Pit**: Hole or depression that cuts through lower stratigraphic layers.
  * **Topography**: Ground surface.
* **Operation**: Excavation units, controls, grids, and measurements.
  * **Elevation Control Point**: Control point for taking local elevations, as with a string and bubble level.
  * **GCP**: Photogrammetry ground control points.
  * **Grid**: Site or survey grid.
  * **Survey Station**: Benchmarks for survey station setup or backsights.
  * **Trench**: Excavation units.

## ShootPoints-Web Interface
ShootPoints-Web’s interface has five primary components:

![ShootPoints-Web interface overview](https://github.com/Lugal-PCZ/readme-images/blob/main/shootpoints-web-frontend_Overview.png?raw=true)
1. **On-The-Fly Adjustments**: Click the arrows icon in the upper left to set atmospheric corrections and prism offsets, which may vary from one shot to the next.
2. **Utilities**: Click the gears icon in the upper right to download data or delete a surveying session. Also, if ShootPoints-Web is running on a Raspberry Pi, you will have options to safely shut it down or reboot it.
3. **Output Box**: The results of your commands will be displayed here.
4. **“Surveying” Section**: Expand this area to collect data with the total station.
5. **“Setup” Section**: Expand this area to input values that should be set prior to beginning surveying, such as your site, total station benchmarks, and class/subclass.

## Start the ShootPoints-Web software:
```bash
cd <path/to/>ShootPoints-Web/shootpoints-web-api/
uvicorn api:app --host 0.0.0.0
```

Open a web browser on your computer and access ShootPoint Web’s interface at [http://localhost:8000/](http://localhost:8000/) or else open a web browser on a device connected to the same wifi network and navigate to [http://<your.computer's.ip.address>:8000/](http://<your.computer's.ip.address>:8000/).

## Save a new site:
1. Expand the “Setup” section.
2. Enter a name for the new site.
3. (*optional*) Enter a description for the new site.
4. Click the “Save New Site” button.  
![Save New Site form](https://github.com/Lugal-PCZ/readme-images/blob/main/shootpoints-web-frontend_SaveNewSite.png?raw=true)

## Save a new station:
1. Choose the site where this survey station is located.
2. Enter a name for the new station.
3. (*optional*) Enter a description for the new station.
4. Choose the coordinate system.
5. Enter the station coordinates.
6. Click the “Save New Station” button.  
![Save New Station form](https://github.com/Lugal-PCZ/readme-images/blob/main/shootpoints-web-frontend_SaveNewStation.png?raw=true)

Add additional stations if you’re working with an existing site with multiple benchmarks with known coordinates.

## Start a new surveying session:
(*If you’re testing with a total station, be sure that it is set up properly, turned on, and connected to your computer’s serial port. Also verify that the proper serial port is selected under the “Set Configs” form of the “Setup” section.*)
1. Minimize the “Setup” section and expand the “Surveying” section.
2. Enter a label for the new surveying session.
3. Enter your name or initials as the responsible surveyor.
4. Choose the site.
5. Choose the occupied point (the station over which the total station is set up).
6. Choose the session type (note: choose “Azimuth,” if you’re in demo mode):
   * **Azimuth**: You will aim the total station at a known landmark, enter the bearing to that landmark, and measure the instrument height by hand.
     * Enter the azimuth to the known landmark and the height of the total station above the occupied point.
   * **Backsight**: You will to shoot a point between two pre-set stations with known coordinates and have ShootPoints-Web calculate the azimuth and instrument height.
     * Select the backsight station and enter the height of the prism pole.
7. Click the “Set Instrument Azimuth” or “Shoot Backsight” button. (The button label will change, depending on the session type.)  
![Start New Session form](https://github.com/Lugal-PCZ/readme-images/blob/main/shootpoints-web-frontend_SetInstrumentAzimuth.png?raw=true)  
![Start New Session form](https://github.com/Lugal-PCZ/readme-images/blob/main/shootpoints-web-frontend_ShootBacksight.png?raw=true)
1.  When prompted to check, verify that the atmospheric conditions displayed in the page header are correct. If they aren’t, dismiss the dialog and click the “On-The-Fly” Adjustments (arrows) icon in the upper left to make the necessary adjustments.
2.  If the atmospheric conditions are correct, aim the total station at the landmark or the prism and click “OK” to start the new surveying session.  
![“Please verify” dialog box](https://github.com/Lugal-PCZ/readme-images/blob/main/shootpoints-web-frontend_PleaseVerify.png?raw=true)

## Create a new grouping:
1. Select the appropriate geometry.
2. Select the class and subclass of this grouping.
3. Enter a label for the grouping.
4. (*optional*) Enter a description for the new grouping.
5. Click the “Start New Grouping” button.  
![Start New Grouping form](https://github.com/Lugal-PCZ/readme-images/blob/main/shootpoints-web-frontend_StartNewGrouping.png?raw=true)


## Collect data:
1. Aim the total station at the prism.
2. Click the “Take Shot” button.  
![Take Shot button](https://github.com/Lugal-PCZ/readme-images/blob/main/shootpoints-web-frontend_TakeShot.png?raw=true)
3. While the shot is being taken, you can click the “Cancel Shot” button to abort.
4. After the shot data have been returned from the total station, you will be given the option to save the shot or discard the data.
5. (*optional*) When saving the shot, you can add a comment (such as “NE corner” or “Edge of marsh”) to assist your recollection and interpretation of the data.  
![Save Last Shot form](https://github.com/Lugal-PCZ/readme-images/blob/main/shootpoints-web-frontend_SaveLastShot.png?raw=true)

Continue taking shots, each of which will be saved to the current grouping. To begin taking shots in a new grouping, simply create a new grouping as described above. You can also end a grouping or session deliberately. (This is useful if multiple excavators are using the total station and you want to prevent the accidental addition of new shots to existing groupings.)

Note that any grouping shot with an “Isolated Point” geometry can logically only have one shot saved to it, so if you’re taking a series of these (such as is typical of end-of-day point elevations in a trench), you will need to create a new grouping for each shot. Though this sounds cumbersome, in practice it is a quick process and ensures that your data are marked consistently.


## Download data:
1. Click the gears icon in the upper right to open the Utilities panel.
2. Select the desired session under “Export Surveying Session Data.”
3. Click the “Export” button.  
![Export Surveying Session Data form](https://github.com/Lugal-PCZ/readme-images/blob/main/shootpoints-web-frontend_ExportData.png?raw=true)
4. Find the newly-downloaded “ShootPoints_Export.zip” file in your Downloads folder and unzip it.

The unzipped directory will be named “ShootPoints_Data_*nnnnnnnnnnnnnn*” with a timestamp for when the session was started (e.g., “ShootPoints_Data_20221031063732”) with the following structure:

```
.
|-- gis_shapefiles
|    |-- allshots.dbf
|    |-- allshots.shp
|    |-- allshots.shx
|    |-- closedpolygons.dbf
|    |-- closedpolygons.shp
|    |-- closedpolygons.shx
|    |-- openpolygons.dbf
|    |-- openpolygons.shp
|    |-- openpolygons.shx
|    |-- pointclouds.dbf
|    |-- pointclouds.shp
|    |-- pointclouds.shx
|-- photogrammetry_gcps
|    |-- gcps_for_dronedeploy.csv
|    |-- gcps_for_metashape.csv
|    |-- gcps_for_webodm.txt
|-- session_info.json
|-- shots_data.csv
```

The contents of the files are as follows:
* **gis_shapefiles**: Directory with shapefiles of the shots taken. Note that because shapefiles can only hold one geometry per file, each of the four ShootPoints geometries is saved as a separate file, to be imported as a separate GIS layer.
  * **allshots.***: All the shots in the surveying session represented as _POINTZ_ objects.
  * **closedpolygons.***: All shots in the surveying session with the “Closed Polygon” ShootPoints geometry represented as _POLYGONZ_ objects.
  * **openpolygons.***: All shots in the surveying session with the “Open Polygon” ShootPoints geometry represented as _POLYLINEZ_ objects.
  * **pointclouds.***: All shots in the surveying session with the “Point Cloud” ShootPoints geometry represented as _MULTIPOINTZ_ objects.
* **photogrammetry_gcps**: Directory with files of ground control points formatted for Photogrammetry processing. All shots taken with the ShootPoints class/subclass of Operation/GCP will be automatically added to these files.
  * **gcps_for_dronedeploy.csv**: CSV file of GCPs for importing into [DroneDeploy](https://www.dronedeploy.com).
  * **gcps_for_metashape.csv**: CSV file of GCPs for importing into [Agisoft Metashape](https://www.agisoft.com).
  * **gcps_for_webodm.txt**: Text file of GCPs for importing into [WebODM](https://www.opendronemap.org/webodm/).
* **session_info.json**: Comprehensive metadata about the surveying session, including counts of the groupings and shots taken.
* **shots_data.csv**: All shots taken during the surveying session, saved as a flat CSV file.
