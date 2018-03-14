# Using GeoServer With OpenLayers

This tutorial demonstrates how to use GeoServer and OpenLayers together to create dynamic, interactive mapping applications. It was written for LaunchCode, and assumes that you have completed both the [OpenLayers Workshop](https://openlayers.org/workshop/en/) and the GeoServer I class by Boundless.

If you found this tutorial on your own and want to give it a shot, you should have a basic knowledge of how to publish data in GeoServer, along with a local GeoServer instance running on port 8080. You can set up such an instance using [a Docker container](https://github.com/kartoza/docker-geoserver). If you completed the Boundless curriculum, you should use the virtual machine that you set up for that course.

## The Project

We will use a data set published by the [City of St. Louis Open Data](https://www.stlouis-mo.gov/data/) project to render St. Louis City parks on a map. The end result will look something like this, displaying park names when the user clicks on a given park.

![](/assets/stl-parks.png)

## Setup

Clone the repository, renaming the project directory as you do so:

```
$ git clone https://github.com/LaunchCodeEducation/openlayers-template.git stl-parks
```

Then create a new GitHub repository named `stl-parks` in your account to store your project code. Connect you local repository to your new remote repository by running these Git commands from the project root

```
$ git remote rm origin
$ git remote add origin REPO_URL
$ git push origin master
```

Here, `REPO_URL` is the project URL \(ending in `.git`\) for the repository you created above.

Install the project dependencies via NPM. From the project root, run `npm install`.

Then make sure the project builds and runs by running `npm start` and visiting `localhost:3000` in your browser. You should see a "Hello World" message in an alert box.

