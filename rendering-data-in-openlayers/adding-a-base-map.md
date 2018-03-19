# Adding a Base Map

Open up the OpenLayers project you set up at the beginning of this tutorial. In a seperate terminal window, go ahead and build and serve the project with `npm start`.

Create a new folder at the top level of the project named `data`. Create a new file in data named `osm-basemap.json`. Copy/paste the [contents](https://gist.github.com/chrisbay/9adf9b57611fa1806d2c1dffa65f1df0) of our `osm-basemap.json` file and save. This file is derived from the `bright.json` file that you worked with on the OpenLayers Workshop, with a bit of customization. We've removed several unneeded layers to clean up the map, and have scaled and cenetered the map on St. Louis. Scroll down to line 41 and you'll see:

```
"center": [
  -90.2549,38.6447
],
"zoom": 13,
  
```

You can customize this if you want a slightly different initial center or scaling.

Open main.js, and add the following lines:

```
import {apply} from 'ol-mapbox-style';

const map = apply('map-container', './data/osm-basemap.json');
```

Visit [localhost:3000](http://localhost:3000/) in your browser and you should see something like the following.

![](/assets/basemap.png)

It's pretty, but also sort of plain. We'll add our parks in the next step.

If you do not see a map in your application, check to be sure the JSON was copied correctly.

