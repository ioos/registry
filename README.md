# Updates


IOOS Service Registry Webinar
Anna Milan gave a presentation on the IOOS Service Registry on March 13 for the IOOS data management community.  A recording of the webinar is available from the url:  https://mmancusa.webex.com/mmancusa/ldr.php?RCID=1ed739d35c7dc0de30ec03a3a7d0e086.

In this webinar, Anna covered the processes that takes place behind the scenes when a service is submitted to the registry.  She described NGDC's Enterprise Metadata Management Architecture (EMMA).  There was a great discussion.  Please view the Webinar if you are interested in learning more.   

# IOOS Service Registry at NGDC

The Service Registry (aka the registry) provides the master list of data sets available via DMAC data access services.  The registry is the official record of what is included in U.S. IOOS.  It is hosted and operated by NGDC and provides a web based discovery interface for IOOS data. 

Functionally the service registry comprises the metadata harvesting process, the WAF generation and maintenance, and the publication of metadata records into the NGDC Geoportal. The Geoportal CSW interface is the main public interface to the metadata. This repository is the primary documentation for the process by which metadata is ingested in the service registry.  The issue tracker should be used for all metadata and harvesting related issues.

The primary client for the Service Registry is the [IOOS Catalog](http://catalog.ioos.us) and the related [software repository](http://github.com/ioos/catalog) for all code supporting the generation of the http://catalog.ioos.us web site. This web site is intended to summarize the data that is published on the services visible in the Service Registry. The interface between the registry and the catalog is the CSW interface to the NGDC geoportal. The repository and issue tracker for the catalog should probably be limited to issues with the code and not procedural issues related to harvesting RA or other partner metadata. 

TODO: Describe the layout using the figure below (Harvestors, XSLT, ncISO, WAF, Metrics and Tools, Geoportal)  

## How do I submit metadata to the Service Registry?
  
![Registration Process Workflow](https://raw.githubusercontent.com/ioos/registry/master/doc/images/IoosWorkFlow12172013(8).png)

**Figure 1.** *IOOS service metadata registration steps.*

NOTE: For some time the registration process has been initiated by an email to  ioos.catalog@noaa.gov including the following 3 pieces of information.  Currently, we are experimenting with using this repository as the primary record of requests for registration.  For now, please log an issue on the issue tracker AND send an email to ioos.catalog@noaa.gov.

1. Service URL or metadata collection in a Web Accessible Folder (WAF)
   * WAF: A web accessible folder containing ISO metadata XML documents.  This offers the most control over the metadata that will appear in the registry, but requires effort to create and maintain. Example: http://www.neracoos.org/WAF/iso/
   * THREDDS: A THREDDS catalog using the .xml extension.  The catalog tree is crawled for all child datasets, but other catalogs referenced by `CatalogRef` are not followed.   Thus registering a catalog that just points to other catalogs will not work.  The individual catalogs must be registered.  Example: http://dm2.caricoos.org/thredds/catalog/swan/catalog.xml
   * SOS: A single getCapabilities request. Example: http://sos.aoos.org/sos/service?service=SOS&request=GetCapabilities&AcceptVersions=1.0.0
   * ERDDAP: The list of ISO records provided by ERDDAP. Example: http://erddap.secoora.org/erddap/metadata/iso19115/xml/
   * WMS: A single getCapabilities file. Example: http://www.neracoos.org/thredds/wms/WW3/fine.nc?service=WMS&version=1.3.0&request=GetCapabilities
2. Point of contact: The person responsible for maintaining the service, or that person's supervisor
3. Affilitation: Regional Association (NERACOOS) or Federal Partner (e.g. NOAA CO-OPS).


## What happens then?
* The URL is manually added to the Service Registry [collection source table](https://www.ngdc.noaa.gov/docucomp/collectionSource/list?&layout=fluid).  It is listed as "submitted"  
* The "submitted" service metadata will be automatically harvested into a [test Web Accessible Folder (WAF)](http://www.ngdc.noaa.gov/metadata/published/test/NOAA/IOOS/). This harvest begins each evening around 1930 MT. 
* If successfully harvested, the test WAF is populated with an ISO metadata record by 0715 MT.  The WAF is manually checked, the next day, to verify the harvest has been successful.  The service status is changed from "submitted" to "approved" in the collection source table. Harvest is considered 'successful' when the metadata records appear in the */iso WAF and pass ISO 19139 schema validation. 
* The [production WAF](http://www.ngdc.noaa.gov/metadata/published/NOAA/IOOS/) in EMMA is automatically populated on day 3 (day after the status has been changed to "approved ") by 0715.  
* The [NGDC Geoportal] (http://www.ngdc.noaa.gov/geoportal/catalog/main/home.page) automatically harvests records from the WAF around 0900. 
* The [IOOS Catalog](http://catalog.ioos.us/) will automatically harvest records from the NGDC Geoportal every 8 hours. 

![Registration process](https://raw.github.com/ioos/registry/master/doc/images/IOOS%20Harvest%20Process.png)
figure 2. 

*  Other addtional pathways to consider for registering new data using the issues in this repository.  Is this feasible?
  * Service queries to other catalogs e.g. CSW or OAI-PMH call to NODC geoportal for GHRSST.

## How do I check the results?
* Review metadata and assessments of metadata in EMMA 
  * http://www.ngdc.noaa.gov/docucomp/page?view=wafsInGroup&title=Metrics%20and%20Collections%20for%20Group%20IOOS&groupName=IOOS
* Search or Browse metadata in Geoportal
  * http://www.ngdc.noaa.gov/geoportal
* Search for metadata in IOOS Catalog

## How do I remove previously harvested metadata?
The registry augments today's harvest with all previous harvests and sometimes this can result in old out-of-date records that are no longer applicable. 

1. If the content of the service has changed, but the service should still be registered then: 
  * send an email to ioos.catalog@noaa.gov requesting a 'clean out' of a particular service or WAF. 
  * The administrator at NGDC will then manually set a flag that will remove ALL previous metadata records before harvest. This will result in an entirely new refresh of the content for that web accessible folder. 
  
2. If the service is out date and should no longer be registered with IOOS then: 
  * send an email to ioos.catalog@noaa.gov requesting that the service be removed. 
  * Rob will change the status of the service to 'For Removal' 
  * The administrator at NGDC will then manually set a flag that will remove ALL previous metadata records before harvest. This will result in an entirely new refresh of the content for that web accessible folder. 
  * The admin at NGDC will then change the status of the servic in the Collection Source table to 'Removed'. 

## How to resolve catalogRefs?
If a THREDDS endpoint contains catalogRefs, it cannot be harvested by the Registry.  An example of an endpoint that contains catalogRefs is: 
 http://opendap.co-ops.nos.noaa.gov/thredds/dbofs_agg.xml
 
The datasets have to be aggregated at a lower level in the xml file to be harvested, as in the example:

http://opendap.co-ops.nos.noaa.gov/thredds/catalog/DBOFS/fmrc/catalog.xml

This example of how to aggregate a catalog is borrowed from Rich Signell's [Github page] (https://github.com/rsignell-usgs).  The example shows what Rich did to aggregate a CariCoos catalog https://github.com/rsignell-usgs/xml/commit/829cda2381bb904a25578fc1743168bf794b71ff.     

# ESRI Geoportal

Main link http://www.ngdc.noaa.gov/geoportal and the dataset browsing page is at http://www.ngdc.noaa.gov/geoportal/catalog/search/browse/browse.page.  

[Master UUID list](https://github.com/ioos/registry/blob/master/uuid.csv)
The [master uuid listing for geoportal collections (e.g. RAs)](https://github.com/ioos/registry/blob/master/uuid.csv)


## Geoportal Search Examples (REST API)
Below are selected example searches that can be issued against the NGDC Geoportal to search for IOOS Regional Association assets.   Additional information on the Geoportal in general can be found on the [ESRI Sourceforge site](https://sourceforge.net/apps/mediawiki/geoportal/index.php?title=REST_API_Syntax)

### Date Search Example
Search within the PacIOOS collection for records with start date 2009-02-01 to end date 2012-02-01 with JSON response: [PacIOOS Date Search Example Link](http://www.ngdc.noaa.gov/geoportal/rest/find/document?rid=local&ridName=NOAA%27s%20Geophysical%20Data%20Center&searchText=startDate%3A%5B1800-01-01%20TO%202012-02-01%5D%20AND%20endDate%3A%5B2009-02-01%20TO%202100-01-01%5D%20%20AND%20sys.siteuuid%3A%22%68FF11D8-D66B-45EE-B33A-21919BB26421%7D%22&start=1&max=10&orderBy=relevance&maxSearchTimeMilliSec=10000&f=pjson)

Decoded Parameters: 
```
rid=local&ridName=NOAA's Geophysical Data Center&searchText=startDate:[1800-01-01 TO 2012-02-01] AND endDate:[2009-02-01 TO 2100-01-01] AND sys.siteuuid:"{68FF11D8-D66B-45EE-B33A-21919BB26421}"&start=1&max=10&orderBy=relevance&maxSearchTimeMilliSec=10000&f=pjson
```

### Keyword Example
Search within the PacIOOS WAF for keywords `sea_water_salinity` with JSON response: [PacIOOS Keyword Example Link](http://www.ngdc.noaa.gov/geoportal/rest/find/document?rid=local&ridName=NOAA%27s%20Geophysical%20Data%20Center&searchText=keywords%3A%20sea_water_salinity%20AND%20sys.siteuuid%3A%22%68FF11D8-D66B-45EE-B33A-21919BB26421%7D%22&start=1&max=1000&orderBy=relevance&maxSearchTimeMilliSec=10000&f=pjson)

Decoded Parameters: 
```
rid=local&ridName=NOAA's Geophysical Data Center&searchText=keywords: sea_water_salinity AND sys.siteuuid:"{68FF11D8-D66B-45EE-B33A-21919BB26421}"&start=1&max=1000&orderBy=relevance&maxSearchTimeMilliSec=10000&f=pjson
```

### Geographic Search Example
Search within the PacIOOS WAF for metadata records within -164.9246,16.6012,-149.4899,25.3959 with JSON response: [PacIOOS Geo Example Link](http://www.ngdc.noaa.gov/geoportal/rest/find/document?rid=local&ridName=NOAA%27s%20Geophysical%20Data%20Center&searchText=sys.siteuuid%3A%22%68FF11D8-D66B-45EE-B33A-21919BB26421%7D%22&start=1&max=1000&orderBy=relevance&spatialRel=esriSpatialRelWithin&bbox=-164.9246,16.6012,-149.4899,25.3959&maxSearchTimeMilliSec=10000&f=pjson)

Decoded Parameters: 
```
rid=local&ridName=NOAA's Geophysical Data Center&searchText=sys.siteuuid:"{68FF11D8-D66B-45EE-B33A-21919BB26421}"&start=1&max=1000&orderBy=relevance&spatialRel=esriSpatialRelWithin&bbox=-164.9246,16.6012,-149.4899,25.3959&maxSearchTimeMilliSec=10000&f=pjson
```

### Multi-Criteria Example
Search within the PacIOOS WAF for metadata records with all of the above with JSON response: [PacIOOS Multi Example Link](http://www.ngdc.noaa.gov/geoportal/rest/find/document?rid=local&ridName=NOAA%27s%20Geophysical%20Data%20Center&rids=local&searchText=keywords%3A%20sea_water_salinity%20AND%20endDate%3A%5B2009-02-01%20TO%202100-01-01%5D%20AND%20startDate%3A%5B1800-01-01%20TO%202012-02-01%5D%20AND%20sys.siteuuid%3A%22%68FF11D8-D66B-45EE-B33A-21919BB26421%22&start=1&max=1000&orderBy=relevance&spatialRel=esriSpatialRelWithin&bbox=-164.9246,16.6012,-149.4899,25.3959&maxSearchTimeMilliSec=10000&f=pjson)

Decoded Parameters: 
```
rid=local&ridName=NOAA's Geophysical Data Center&rids=local&searchText=keywords: sea_water_salinity AND endDate:[2009-02-01 TO 2100-01-01] AND startDate:[1800-01-01 TO 2012-02-01] AND sys.siteuuid:"{68FF11D8-D66B-45EE-B33A-21919BB26421}"&start=1&max=1000&orderBy=relevance&spatialRel=esriSpatialRelWithin&bbox=-164.9246,16.6012,-149.4899,25.3959&maxSearchTimeMilliSec=10000&f=pjson
```

### WMS Example
Search within the PacIOOS WAF for metadata records with a WMS service endpoint with GeoRSS response: [PacIOOS WMS Example Link](http://www.ngdc.noaa.gov/geoportal/rest/find/document?rid=local&ridName=NOAA%27s%20Geophysical%20Data%20Center&rids=local&searchText=sys.siteuuid%3A%22%68FF11D8-D66B-45EE-B33A-21919BB26421%22%20AND%20wms.resource.url%3A*&start=1&max=1000&orderBy=relevance&maxSearchTimeMilliSec=10000&f=georss)

Decoded Parameters: 
```
rid=local&ridName=NOAA's Geophysical Data Center&rids=local&searchText=wms.resource.url:* AND sys.siteuuid:"{68FF11D8-D66B-45EE-B33A-21919BB26421}"&start=1&max=1000&orderBy=relevance&maxSearchTimeMilliSec=10000&f=georss
```

### WCS Example
Search within the PacIOOS WAF for metadata records with a WCS service endpoint with html response: [PacIOOS WCS Example Link](http://www.ngdc.noaa.gov/geoportal/rest/find/document?rid=local&ridName=NOAA%27s%20Geophysical%20Data%20Center&rids=local&searchText=wcs.resource.url:*%20AND%20sys.siteuuid%3A%22%68FF11D8-D66B-45EE-B33A-21919BB26421%22&start=1&max=1000&orderBy=relevance&maxSearchTimeMilliSec=10000&f=html)

Decoded Parameters: 
```
rid=local&ridName=NOAA's Geophysical Data Center&rids=local&searchText=wcs.resource.url:* AND sys.siteuuid:"{68FF11D8-D66B-45EE-B33A-21919BB26421}"&start=1&max=1000&orderBy=relevance&maxSearchTimeMilliSec=10000&f=html
```

### SOS Example 
Search within the PacIOOS WAF for metadata records with an SOS service endpoint with html response: [PacIOOS SOS Example Link](http://www.ngdc.noaa.gov/geoportal/rest/find/document?rid=local&ridName=NOAA%27s%20Geophysical%20Data%20Center&rids=local&searchText=sos.resource.url:*%20AND%20sys.siteuuid%3A%22%68FF11D8-D66B-45EE-B33A-21919BB26421%22&start=1&max=1000&orderBy=relevance&maxSearchTimeMilliSec=10000&f=html)

Decoded Parameters: 
```
rid=local&ridName=NOAA's Geophysical Data Center&rids=local&searchText=sos.resource.url:* AND sys.siteuuid:"{68FF11D8-D66B-45EE-B33A-21919BB26421}"&start=1&max=1000&orderBy=relevance&maxSearchTimeMilliSec=10000&f=html
```

### OPeNDAP Example 
Search within the PacIOOS WAF for metadata records with a OpenDAP service endpoint with html response: [PacIOOS OPeNDAP Example Link](http://www.ngdc.noaa.gov/geoportal/rest/find/document?rid=local&ridName=NOAA%27s%20Geophysical%20Data%20Center&rids=local&searchText=odp.resource.url:*%20AND%20sys.siteuuid%3A%22%68FF11D8-D66B-45EE-B33A-21919BB26421%22&start=1&max=1000&orderBy=relevance&maxSearchTimeMilliSec=10000&f=html)

Decoded Parameters: 
```
rid=local&ridName=NOAA's Geophysical Data Center&rids=local&searchText=odp.resource.url:* AND sys.siteuuid:"{68FF11D8-D66B-45EE-B33A-21919BB26421}"&start=1&max=1000&orderBy=relevance&maxSearchTimeMilliSec=10000&f=html
```



## Geoportal Search Examples (CSW)

NGDC CSW Service Endpoint: http://www.ngdc.noaa.gov/geoportal/csw
The examples below must be made as an XML POST request.

PacIOOS collection


### WMS Search Example
```xml
<?xml version="1.0"?>	
<csw:GetRecords xmlns:csw="http://www.opengis.net/cat/csw/2.0.2" version="2.0.2" service="CSW" resultType="results" 
outputSchema="http://www.isotc211.org/2005/gmd" startPosition="1" maxRecords="1000">
  <csw:Query typeNames="csw:Record" xmlns:ogc="http://www.opengis.net/ogc" xmlns:gml="http://www.opengis.net/gml">
  <csw:ElementSetName>full</csw:ElementSetName> 
  <csw:Constraint version="1.1.0">
  <ogc:Filter>
    <ogc:And>
      <ogc:PropertyIsEqualTo>
        <ogc:PropertyName>sys.siteuuid</ogc:PropertyName>
        <ogc:Literal>{68FF11D8-D66B-45EE-B33A-21919BB26421}</ogc:Literal>
      </ogc:PropertyIsEqualTo>
      <ogc:PropertyIsLike wildCard="*" escape="\" singleChar="?"> 
        <ogc:PropertyName>apiso:ServiceType</ogc:PropertyName>
        <ogc:Literal>*wms*</ogc:Literal>
      </ogc:PropertyIsLike>
    </ogc:And> 
  </ogc:Filter> 
</csw:Constraint> 
</csw:Query>
</csw:GetRecords>
```

### WCS Search Example
```xml
<?xml version="1.0"?>	
<csw:GetRecords xmlns:csw="http://www.opengis.net/cat/csw/2.0.2" version="2.0.2" service="CSW" resultType="results" 
outputSchema="http://www.isotc211.org/2005/gmd" startPosition="1" maxRecords="1000">
  <csw:Query typeNames="csw:Record" xmlns:ogc="http://www.opengis.net/ogc" xmlns:gml="http://www.opengis.net/gml">
  <csw:ElementSetName>full</csw:ElementSetName> 
  <csw:Constraint version="1.1.0">
  <ogc:Filter>
    <ogc:And>
      <ogc:PropertyIsEqualTo>
        <ogc:PropertyName>sys.siteuuid</ogc:PropertyName>
        <ogc:Literal>{68FF11D8-D66B-45EE-B33A-21919BB26421}</ogc:Literal>
      </ogc:PropertyIsEqualTo>
      <ogc:PropertyIsLike wildCard="*" escape="\" singleChar="?"> 
        <ogc:PropertyName>apiso:ServiceType</ogc:PropertyName>
        <ogc:Literal>*wcs*</ogc:Literal>
      </ogc:PropertyIsLike>
    </ogc:And> 
  </ogc:Filter> 
</csw:Constraint> 
</csw:Query>
</csw:GetRecords>
```

### OPeNDAP Search Example


```xml
<?xml version="1.0"?>	
<csw:GetRecords xmlns:csw="http://www.opengis.net/cat/csw/2.0.2" version="2.0.2" service="CSW" resultType="results"
outputSchema="http://www.isotc211.org/2005/gmd" startPosition="1" maxRecords="1000">
  <csw:Query typeNames="csw:Record" xmlns:ogc="http://www.opengis.net/ogc" xmlns:gml="http://www.opengis.net/gml">
  <csw:ElementSetName>full</csw:ElementSetName> 
  <csw:Constraint version="1.1.0">
  <ogc:Filter>
    <ogc:And>
      <ogc:PropertyIsEqualTo>
        <ogc:PropertyName>sys.siteuuid</ogc:PropertyName>
        <ogc:Literal>{68FF11D8-D66B-45EE-B33A-21919BB26421}</ogc:Literal>
      </ogc:PropertyIsEqualTo>
      <ogc:PropertyIsLike wildCard="*" escape="\" singleChar="?"> 
        <ogc:PropertyName>apiso:ServiceType</ogc:PropertyName>
        <ogc:Literal>*opendap*</ogc:Literal>
      </ogc:PropertyIsLike>
    </ogc:And> 
  </ogc:Filter> 
</csw:Constraint> 
</csw:Query>
</csw:GetRecords>
```

### SOS Search Example

```xml
<?xml version="1.0"?>	
<csw:GetRecords xmlns:csw="http://www.opengis.net/cat/csw/2.0.2" version="2.0.2" service="CSW" resultType="results"
outputSchema="http://www.isotc211.org/2005/gmd" startPosition="1" maxRecords="1000">
  <csw:Query typeNames="csw:Record" xmlns:ogc="http://www.opengis.net/ogc" xmlns:gml="http://www.opengis.net/gml">
  <csw:ElementSetName>full</csw:ElementSetName> 
  <csw:Constraint version="1.1.0">
  <ogc:Filter>
    <ogc:And>
      <ogc:PropertyIsEqualTo>
        <ogc:PropertyName>sys.siteuuid</ogc:PropertyName>
        <ogc:Literal>{68FF11D8-D66B-45EE-B33A-21919BB26421}</ogc:Literal>
      </ogc:PropertyIsEqualTo>
      <ogc:PropertyIsLike wildCard="*" escape="\" singleChar="?"> 
        <ogc:PropertyName>apiso:ServiceType</ogc:PropertyName>
        <ogc:Literal>*sos*</ogc:Literal>
      </ogc:PropertyIsLike>
    </ogc:And> 
  </ogc:Filter> 
</csw:Constraint> 
</csw:Query>
</csw:GetRecords>
```

### ISO Date Modified Search

```xml
<csw:GetRecords xmlns:csw="http://www.opengis.net/cat/csw/2.0.2" version="2.0.2" service="CSW" resultType="results" startPosition="1" maxRecords="11" outputSchema="http://www.isotc211.org/2005/gmd"> <csw:Query typeNames="csw:Record" xmlns:ogc="http://www.opengis.net/ogc" xmlns:gml="http://www.opengis.net/gml" > 
<csw:ElementSetName>full</csw:ElementSetName> 
<csw:Constraint version="1.1.0"> 
  <ogc:Filter>
    <ogc:And>
      <ogc:PropertyIsGreaterThan> 
      <ogc:PropertyName>apiso:modified</ogc:PropertyName> <ogc:Literal>2011-09-30</ogc:Literal>
      </ogc:PropertyIsGreaterThan> 
      <ogc:PropertyIsLessThan> 
      <ogc:PropertyName>apiso:modified</ogc:PropertyName> <ogc:Literal>2011-10-02</ogc:Literal>
      </ogc:PropertyIsLessThan> 
    </ogc:And>
  </ogc:Filter>
</csw:Constraint> 
</csw:Query> 
</csw:GetRecords> 
```













