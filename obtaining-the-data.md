# Obtaining the Data

Visit the [City of St. Louis Open Data](https://www.stlouis-mo.gov/data/) project page and navigate to the "Urban Development" section. Once there, look for "Park Data" and click through. From this page, download the "Parks Shapefile." The download is a `.zip` file which contains the `.shp` file along with its sidecar files. Don't unzip the downloaded file just yet.

## Staging the Shapefile

At this point, make sure your GeoServer VM is up and running in VirtualBox.

For GeoServer to be able to use the shapefile, it must be placed within GeoServer's data directory. Where is this directory? Our GeoServer VM is helpful enough to tell us when we log into it:

```
$ ssh -p 2020 training@localhost
Last login: Tue Mar 13 19:33:00 2018 from 10.0.2.2
======================================================================
Welcome! This virtual machine is designed for use with the following
Boundless course: **Boundless Suite: GeoServer I**

Useful commands:
sudo service tomcat8 start (start Tomcat)
sudo service tomcat8 stop  (stop Tomcat)
sudo poweroff              (shut down the virtual machine)

Useful directories:
/opt/boundless/suite/geoserver          (GeoServer install directory)
/var/opt/boundless/suite/geoserver/data (GeoServer data directory)
/media/sf_share                         (share directory between host and guest)

For more information: http://boundlessgeo.com
======================================================================
training@boundless:~$
```

Note near the bottom of the login message that the data directory is `/var/opt/boundless/suite/geoserver/data`. This directory lives within the virtual machine, so we first need to place the `.zip` file on the VM before we can move it to the data directory.

The welcome message also tells us that `/media/sf_share` is the directory on the VM that is shared with our host operating system. To see which directory on the host system is mapped to this directory on the VM, go to the VirtualBox application, right-click on the _Boundless GeoServer I_ VM, and select _Settings_. Select the _Shared Folders_ pane and you'll see the the directory on the host that is mapped to shared directory on the guest \(i.e. the VM\). In the example below, it is `/Users/chris`.

![](/assets/shared.png)Place the `parks.zip` file in this directory. Then SSH into the VM using `ssh -p 2020 training@localhost`. Run `ls /media/sf_share/` and make sure you see `parks.zip` listed.

Now, move the file to the GeoServer data directory. Note that we need to prepend the commands with sudo since our VM user doesn't have permission to write to the destination directory. Also recall that the password on the VM is empty \(i.e. just press Enter when prompted for a password\).

```
training@boundless:~$ sudo mv /media/sf_share/parks.zip /var/opt/boundless/suite/geoserver/data
```

Then go to this directory and unzip the file.

```
training@boundless:~$ cd /var/opt/boundless/suite/geoserver/data
training@boundless:/var/opt/boundless/suite/geoserver/data$ sudo unzip parks.zip -d stl-parks
Archive:  parks.zip
  inflating: stl-parks/parks.dbf   
  inflating: stl-parks/parks.prj   
  inflating: stl-parks/parks.sbn   
  inflating: stl-parks/parks.sbx   
  inflating: stl-parks/parks.shp   
  inflating: stl-parks/parks.shp.xml  
  inflating: stl-parks/parks.shx
```

In the next section, we will configure GeoServer to use this data.

