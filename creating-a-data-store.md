# Creating a Data Store

Log into the GeoServer admin portal by visiting [http://localhost:8080/geoserver/](http://localhost:8080/geoserver/) in your browser and entering the credentials admin/geoserver.

## Creating a Workspace

Recall that in GeoServer, every data store belongs to a workspace. Create a new workspace by selecting _Data &gt; Workspaces_ from the menu at left. Select _Add new workspace_. Name the workspace **lc** and give it the URI **http://launchcode.org**. 

![](/assets/workspace.png)

## Creating the Store

Visit _Data &gt; Stores_ and select _Add store_. Select _Shapefile_ from the _Vector Data Sources_ list. Fill out the required fields, and then browse to the _.shp_ file that you previously placed in _stl-parks_ subdirectory of GeoServer's data directory.

![](/assets/data-source.png)Upon saving the store, you will immediately be presented with the option of publishing a new layer named _parks_. Let's try to do this.

After clicking on the _Publish_ link, you will see the **Edit Layer** form. Scroll down to the **Coordinate Reference Systems** section. This is where we configure the native and published SRS for the layer. Notice that the **Native SRS** field contains UNKNOWN, with the link at right showing an SRS named "NAD\_1983\_StatePlane\_Missouri\_East\_FIPS\_2401\_Feet..." This SRS is the spatial reference system declared within the shapefile itself, but if we try to find it within GeoServer, we will fail.

Click on _Find_ next to the **Declared SRS **field and search for "1983". The only result returned is _not_ the SRS described by the file. In the next section, we'll fix this issue by adding the SRS to GeoServer and finally publishing the layer.

