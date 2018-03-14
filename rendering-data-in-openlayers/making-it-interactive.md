# Making It Interactive

Now we'll turn our attention to display park names on user click. We'll largely follow the [OpenLayers workshop instructions on adding a popup](https://openlayers.org/workshop/en/basics/popup.html).

In fact, the tasks are so similar that we'll let you review those instructions and only mention the parts that are different for our case.

The only modifications you'll need to make are in the click handler, which we include here for reference:

```
﻿﻿﻿map.on('click', function(e) {
  overlay.setPosition();
  var features = map.getFeaturesAtPixel(e.pixel);
  if (features) {
    var coords = features[0].getGeometry().getCoordinates();
    var hdms = coordinate.toStringHDMS(proj.toLonLat(coords));
    overlay.getElement().innerHTML = hdms;
    overlay.setPosition(coords);
  }
});
```

## Getting Park Names

The array of features, `features`, representes all features at the location that was clicked. For us, we this will only be one \(there can be only one park at a given location\). Thus, `features[0]` will be the park feature in question. You'll want to put this feature in its own variable, and then get the properties associated with the park using its `getProperties` method. See the [OpenLayers API docs](http://openlayers.org/en/latest/apidoc/ol.Feature.html#getProperties) for usage.

The park name will be one of the properties returned. Which one? Use `console.log` to print all of the properties to the browser JS console to find out. Once you know the name of the property that contains each park's name, put it in the overlay.

## Positioning the Overlay

The example code above works to position the overlay on features that are points. If you try to use the same positioning calls with your parks, which have extents \(i.e. width and height\) you'll get some odd results. Let's look at how we can position the overlay more nicely.

The `getGeometry` method will get the full geometry of a feature. Use this method to get the geometry for the park and place it in a variable \(and [read about it](http://openlayers.org/en/latest/apidoc/ol.Feature.html#getGeometry)\).

Once we have a park's geometry, we can get its extent, which represents the bounding box for the park. Calling `getExtent` on the geometry object will return the extent in the form of an array, which looks like `[minx, miny, maxx, maxy]`, where the four values represent the corresponding coordinate values for the corners of the park's bounding box. Put the extent in yet another variable. [Read about the method](http://openlayers.org/en/latest/apidoc/ol.geom.Geometry.html#getExtent).

Once you have the extent, you might try to position the overlay at, say, the top left using the `minx` and `maxy` coordinates. Try this and see what the results are.

When you tried using the exact values of the extent, the overlay was rarely, if ever, positioned in a way in which it touched the given park. This is because the corner of the bounding box is usually outside of a feature's geometry. What we really want to do is to position the overlay at the point on the park's geometry that is _closest_ to the top left corner. Thankfully, there's a method to help us with this!

`getClosestPoint` can be called on a geometry object. It takes as a parameter a point, and it returns the point within the given geometry that is closest to the point paramter that it is given. [Read about the method](http://openlayers.org/en/latest/apidoc/ol.geom.Geometry.html#getClosestPoint), and then use it to obtain the point within a park that is closes to the top left corner of its bounding box. Then, position the overlay at this point.

When you have it working, your map will look something like this:

![](/assets/overlay-positioned.png)Congrats! Your St. Louis City parks map is looking great!

Continue on to the next section to read about some additional ways that you might extend the app to continue learning about mapping applications, OpenLayers, and GeoServer.

