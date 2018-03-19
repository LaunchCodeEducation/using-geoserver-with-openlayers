# Fetching Parks Data

For these last sections, we leave some details to you to figure out. Refer to the [OpenLayers Workshop](https://openlayers.org/workshop/en/) and the [OpenLayers API](http://openlayers.org/en/latest/apidoc/index.html) whenever get stuck or need more info. Everything we'll ask you to do will either have been covered in the workshop, or will be covered in another related resource that we'll provide.

## Creating the Layer

Before we can fetch data from GeoServer, we need a layer in which to put it. We'll create a vector layer to store the features fetched from the parks WFS. In this and all remaining sections, all JavaScript code referenced will be in `main.js`.

First, add a couple of imports:

```
import VectorLayer from 'ol/layer/vector';
import VectorSource from 'ol/source/vector';
```

Then, below the line which creates the map, create a vector source and layer, and add them to the map.

```
let parksSource = new VectorSource();
let parksLayer = new VectorLayer({
  source: parksSource
});

parksLayer.setZIndex(50);
map.addLayer(parksLayer);
```

Note the next to last line in the snippet above. This sets the z-index of the parks layer to a relatively high number, which ensures that the parks will display "on top of" other layers. Layers with higher z-index are displayed above layers with lower z-index.

If you were to refresh your app, nothing would change. While we've added a layer, there are not features in the layer to display!

## Populaing the Layer

We'll follow the [WFS - GetFeature demo](https://openlayers.org/en/latest/examples/vector-wfs-getfeature.html) provided by OpenLayers. Have a look at this code and try to decipher what it is doing before continuing on here. Note that the code is structured to be embedded in an HTML file, rather than in a separate JS file. Hence, there are no imports and the OpenLayers references have the full object namespaces referenced.

Let's adapt this example for our purposes. In particular, we want to adapt the two code blocks near the bottom, which look like this in the example:

```
// generate a GetFeature request
var featureRequest = new ol.format.WFS().writeGetFeature({
  srsName: 'EPSG:3857',
  featureNS: 'http://openstreemap.org',
  featurePrefix: 'osm',
  featureTypes: ['water_areas'],
  outputFormat: 'application/json',
  filter: ol.format.filter.and(
      ol.format.filter.like('name', 'Mississippi*'),
      ol.format.filter.equalTo('waterway', 'riverbank')
  )
});

// then post the request and add the received features to a layer
fetch('https://ahocevar.com/geoserver/wfs', {
  method: 'POST',
  body: new XMLSerializer().serializeToString(featureRequest)
}).then(function(response) {
  return response.json();
}).then(function(json) {
  var features = new ol.format.GeoJSON().readFeatures(json);
  vectorSource.addFeatures(features);
  map.getView().fit(vectorSource.getExtent());
});
```

This code is fetching water features from the WFS at `https://ahocevar.com/geoserver/wfs`. Your task is to adapt each of these blocks so that they work in your code. We'll describe what each portion of this example is doing, and leave the task of modifying it for your app up to you.

### Building the Request

A GeoServer WFS request must be made in XML format. Building up XML within JavaScript code would be very painful and clunky, so thankfully OpenLayers provides an interface for us to configure our requests using its JS API, and then turn a request object into XML. The first block in the code above builds up the request. It specifies the request parameters that we want to pass to GeoServer to cusomize the feature set we get back.

Let's look at each parameter of the call to `writeGetFeature`:

* `srsName` - specifies the SRS \(or coordinate system\) that we want our data in. Recall that the SRS of our parks data is EPSG:102696. This is different from the SRS of our base map. Thus, you'll need to ask GeoServer to convert the park data to the same SRS as the base map by setting the value of this paramter. Which SRS is your OSM basemap rendered in? Figure that out, and use the value to customize your own request.
* `featureNS` - the URI of the workspace that owns the layer
* `featurePrefix` - the name of the workspace that owns the layer
* `featureTypes` - the name of the WFS layer
* `outputFormat` - the format that we want the data returned in \(can be `application/json`, `application/xml`, etc\)
* `filter` - the filter to apply to the feature set. In the example above, only river bank water features that start with "Mississippi" are returned. If you leave off the `filter` parameter, all features will be returned.

For additional parameters that can be used with `GetFeature`, see the [OpenLayers API documentation](http://openlayers.org/en/latest/apidoc/ol.format.WFS.html#writeGetFeature). 

Add a customized version of the above GetFeature call to your app. Be sure to add the following import:

```
import WFS from 'ol/format/wfs';
```

With these, you can modify the first line to simply:

```
let featureRequest = new WFS().writeGetFeature({
```

### Making the Request

Now we will customize the second block, which looks like this:

```
fetch('https://ahocevar.com/geoserver/wfs', {
  method: 'POST',
  body: new XMLSerializer().serializeToString(featureRequest)
}).then(function(response) {
  return response.json();
}).then(function(json) {
  var features = new ol.format.GeoJSON().readFeatures(json);
  vectorSource.addFeatures(features);
  map.getView().fit(vectorSource.getExtent());
});
```

In this example, a POST request is made to `https://ahocevar.com/geoserver/wfs` with body that contains XML generated from the previously configured request object. In particular, `new XMLSerializer().serializeToString(featureRequest)` creates the given XML.

The request is first handled by this function:

```
.then(function(response) {
  return response.json();
})
```

which turns the response string into JSON.

The next handler uses the OpenLayers API to parse the JSON object into GeoJSON format and extract the features. It then adds the features to the map. Finally, it zooms the map to fit around the given set of features.

```
.then(function(json) {
  var features = new ol.format.GeoJSON().readFeatures(json);
  vectorSource.addFeatures(features);
  map.getView().fit(vectorSource.getExtent());
});
```

For your own code, you can make minimal modifications to this block to get it working. In particular, you'll need to customize the URL for the request, and make sure all variable names are correct. We also recommend using `let` instead of `var` to create your variables. You'll also likely want to remove the last line, which shifts the map view. Finally, you'll need the import:

```
import GeoJSON from 'ol/format/geojson';
```

Which will allow you to modify the line calling `GeoJSON()` accordingly.

Once you modify the code discussed to fit your projeject, you should see parks rendered on the map!

![](/assets/park-features.png)

Next, we'll style the parks to look nice.

