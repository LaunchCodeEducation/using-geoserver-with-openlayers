# Publishing a Layer

In the previous section, we created a new workspace and data store from a shapefile, and discovered that the geo data within the shapefile used an SRS that was unknown to our GeoServer. In order for the data to be properly served by GeoServer, a layer must be configured with the correct SRS. Fortunately, we can add new SRS configurations to GeoServer.

## Adding a New SRS

We will add a new SRS definition to GeoServer following the [instructions in the GeoServer documentation](http://docs.geoserver.org/stable/en/user/configuration/crshandling/customcrs.html). 

If you don't have a terminal open and logged in to your GeoServer VM, do so now:

```
$ ssh -p 2020 training@localhost
```

Recall that the password is empty.

Once logged into your GeoServer VM, go to the data directory:

```
training@boundless:~$ cd /var/opt/boundless/suite/geoserver/data
```

From here, descend one more directory into `user_projections`: 

```
training@boundless:/var/opt/boundless/suite/geoserver/data$ cd user_projections/
```

Open the file `epsg.properties` for editing in the text editor nano:

```
training@boundless:/var/opt/boundless/suite/geoserver/data/user_projections$ sudo nano epsg.properties
```

Use the down arrow key to navigate to the end of the file. We'll add the new SRS definition here. We need to grab the SRS defintion, so let's head over to [epsg.io](http://epsg.io/), a site that is a reference for coordinate system definitions.

Search for "1983 missouri 2401" and the first result you see should be the coordinate system we're looking for, "NAD 1983 StatePlane Missouri East FIPS 2401 Feet." Select this result to see the full details. Select and copy the GeoServer version of the coordinate system.

Back in your open text editor, on a new line at the end of the file, paste in the definition. Then use `ctrl+X` to quit nano, select "Yes" when asked if you want to save the file.

To finish up, we need to restart GeoServer:

```
$ sudo service tomcat8 stop
[sudo] password for training: 
Stopping tomcat8:  * 
$ sudo service tomcat8 start
Starting tomcat8:  * 

```

Verify that the new SRS was found by GeoServer by selecting _Demos_ from the left-hand menu, and then choosing _SRS List_. Search for the coordinate system by number, 102696. If it isn't, retrace your steps from the instructions above.

## Publishing the Layer

We can now properly publish the layer. Go to _Data &gt; Layers_ and select _Add a new layer_. Choose the data store you created for the parks data, and click on _Publish_ next to the `parks` layer.

The only settings on the Edit Layer page that need to be changed are in the **Coordinate Reference Systems** and **Bounding Box** sections. Update these to match the displayed settings below, finding the **Declared SRS** using the Find dialog and searching for "102696." The **Bounding Box** fields are populated by simply clicking on the highlighted links.

![](/assets/crs-config.png)

Save the new layer, and give yourself a high five for publishing your first data set in GeoServer!

## Previewing the Layer

Let's make sure everything looks good with the layer before moving on. Navigate to _Data &gt; Layer Preview_ and choose the _OpenLayers_ option for the newly-created layer. A new tab will open with the geometries in the given layer displayed in a small box. Without a base layer to give them context, they'll look a bit funny, but you should recocnize some familiar city parks. If your preview doesn't look like this, then edit the layer and make sure you have the CRS and Bounding Box settings correct.

![](/assets/layer-preview.png)

