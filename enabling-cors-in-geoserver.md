# Enabling CORS in GeoServer

CORS stands for **Cross-Origin Resource Sharing**, and it refers to the ability of a JavaScript resource from one origin to access resources from another origin.

For example, a script hosted at `mydomain.com` may not access resources at `yourdomain.com` unless the latter domain allows it to do so. This is a security precaution, to prevent malicious scripts from accessing resources they shouldn't have access to. Most servers disable CORS to some degree \(or entirely\). Resource sharing can be enabled at-large \(for requests from any origin\) or on a case-by-case basis \(only for some origins\).

We encourage you to [read more about CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS), as it is an essential topic for anybody doing front-end development.

If you were to try to access data from GeoServer \(hosted at `localhost:8080`\) via your OpenLayers/JavaScript app \(hosted at `localhost:3000`\) you would likely see an error in  your browser console referring to the `Access-Control-Allow-Origin` HTTP header \(i.e. GeoServer\). This header is set by the server that is restricting cross-origin access. To enable CORS, we need to update GeoServer's configuration.

## Getting Our Hands Dirty

From a terminal, SSH into your GeoServer VM: `ssh -p 2020 training@localhost`. From there, go to the directory for GeoServer, and then into the WEB-INF subdirectory.

```
$ cd /opt/boundless/suite/geoserver/
$ cd WEB-INF
```

This directory contains a file, `web.xml`, that configures the GeoServer application. Open the file with:

```
$ sudo nano web.xml
```

Move your cursor to a blank line just inside the `<web-app>` tag \(you can create such a blank line, if necessary, but don't go within any other XML tag\). Paste this code block:

```
<filter>
  <filter-name>CorsFilter</filter-name>
  <filter-class>org.apache.catalina.filters.CorsFilter</filter-class>
</filter>
<filter-mapping>
  <filter-name>CorsFilter</filter-name>
  <url-pattern>/*</url-pattern>
</filter-mapping>
```

This will allow access to GeoServer resources from _any_ origin. For a production server, this is definitely not something we'd want to allow, but it's fine for local development.

Save the file by entering `ctrl+X` and then answering "Y" when asked if you want to save your changes. For the changes to take effect, we need to restart the Tomcat server \(i.e. the application server running GeoServer\).

```
$ sudo service tomcat8 stop
$ sudo service tomcat8 start
```

That's it! In upcoming sections, you'll be able to fetch geo data from your OpenLayers app with no CORS errors.

