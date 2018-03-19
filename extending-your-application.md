# Extending Your Application

Now that you've created a GIS application using both front and back-end technologies--publishing a data set and pulling it into an interactive map--you're ready to venture out on your own to continue learning.

Here are a few ways in which you might proceed.

## Adding Controls

OpenLayers provides the [`Control` class](http://openlayers.org/en/latest/apidoc/ol.control.Control.html), which can be used to add user controls. It has various subclasses that you can use out of the box, and can also be used to create controls with custom behavior. You might try to add:

* The ZoomSlider control for controlling zoom level
* A control that can be clicked on to zoom to the user's location \(refer to the OpenLayers Workshop to [see how to retrieve the user's location and use it](https://openlayers.org/workshop/en/basics/geolocation.html)\)
* A control to toggle a layer's visibility

## Publishing Additional Layers

The city of St. Louis provides additional GIS data in the form of shapefiles, including [ward and neighborhood boundaries](https://www.stlouis-mo.gov/data/boundaries/ward-neighborhood-boundaries.cfm), [historic districts](https://www.stlouis-mo.gov/data/historic-districts.cfm), [land use data](https://www.stlouis-mo.gov/data/land-use.cfm), and more. There is a [listing of all shapefile data](https://www.stlouis-mo.gov/data/types/gis.cfm) as well.

You might also look for additional open data resources. For example, after a few minutes of searching for geo data for other area parks, we found a [shapefile for St. Charles County Parks](http://gis-sccmo.opendata.arcgis.com/datasets/SCCMOBILE::parks/data).

When publishing new layers in GeoServer, be sure to pay attention to the SRS of the layer, along with styling \(in particular, the zIndex\).

## Creating Yet Another Application

To bite off a bigger task, consider a completely different mapping application based on OpenLayers and GeoServer. Look for a usable open data set, and you can build your own app from scratch by publishing it in GeoServer, and starting with the OpenLayers template we used for our STL Parks app. You can follow the [same setup steps](https://launchcodeeducation.gitbooks.io/using-geoserver-with-openlayers/content/) that we started with for this tutorial to get off and running!

