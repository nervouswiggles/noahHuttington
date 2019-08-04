
## Review of GWC server
Last updated: 040819DR

## Notes
main data directory: C:\Program Files (x86)\GeoServer 2.15.1\data_dir




## Issue 1 - The main redirect was not working

To replicate the error, if you went to https://gs.furmanrecords.com/ but https://gs.furmanrecords.com/geoserver does work

#### Solution

Populated the ROOT directory with an index.jsp.

![](/images/rootChanges.png)


Within that file, the following line was populated <% response.sendRedirect("/geoserver"); %>


![](/images/indexForRoot.png)


## Issue 2 - The application simply would not start

#### Issue
#### Files needed
[geoWebCache.xml](./files/geowebcache.xml)<br>
[geowebcache-diskquota.xml](./files/geowebcache-diskquota.xml)

#### Solution
Within the
`C:\Program Files (x86)\Apache Software Foundation\Tomcat 9.0\webapps\geoserver\WEB-INF\web.xml`, ensure that following xml node contains the following xml value (Note that this was missing before, but there are multiple ways to set this)

```xml
 <!-- 080119DR Included this to avoid the null pointer exception-->
<context-param>
     <param-name>GWC_DISKQUOTA_DISABLED</param-name>
      <param-value>false</param-value>
  </context-param>
```

Review the `C:\Program Files (x86)\GeoServer 2.15.1-Service\data_dir\logs\geoserver.log file` within the following logging. Below is an example of the GWC being located and configured correctly

```log
2019-08-04 10:10:23,752 INFO [storage.BlobStoreAggregator] - BlobStoreConfiguration gwc contained no blob store infos.
2019-08-04 10:10:23,767 INFO [storage.DefaultStorageFinder] - *************************************************************************************************************************************************************
2019-08-04 10:10:23,767 INFO [storage.DefaultStorageFinder] - *** Found System environment variable GEOSERVER_DATA_DIR set to C:\Program Files (x86)\GeoServer 2.15.1-Service\data_dir, using it as the default prefix. ***
2019-08-04 10:10:23,767 INFO [storage.DefaultStorageFinder] - *************************************************************************************************************************************************************
2019-08-04 10:10:23,861 INFO [gwc.config] - Initializing GeoServer specific GWC configuration from gwc-gs.xml
2019-08-04 10:10:24,061 INFO [config.GeoserverXMLResourceProvider] - Will look for 'geowebcache-diskquota.xml' in directory 'C:\Program Files (x86)\GeoServer 2.15.1-Service\data_dir\gwc'.
2019-08-04 10:10:24,073 INFO [config.GeoserverXMLResourceProvider] - Will look for 'geowebcache-diskquota-jdbc.xml' in directory 'C:\Program Files (x86)\GeoServer 2.15.1-Service\data_dir\gwc'.
2019-08-04 10:10:24,080 INFO [config.GeoserverXMLResourceProvider] - Found configuration file in gwc
2019-08-04 10:10:24,081 INFO [diskquota.ConfigLoader] - Quota config is: gwc/geowebcache-diskquota.xml
```
