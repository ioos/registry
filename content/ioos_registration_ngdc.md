+++
weight = "20"
type = "post"
draft = false
sidebar = true
Title = "IOOS Service Registration Process Guidelines"
date = 2014-08-04T07:56:39Z
+++

<!-- For a single homepage put in FrontMatter url = "index.html"; for a summary of multiple documents on one page do not put url parameter there at all -->

The registration of the IOOS services is a process that allows the wide range of various clients to efficiently discover U.S. IOOS data. Currently, all U.S. IOOS DMAC services must be registered with the [Service Registry ("Registry")](https://www.ngdc.noaa.gov/docucomp/collectionSource/list?&layout=fluid), which is hosted and operated by the NGDC. 
<!--more-->

The Registry is the official list of service URLs that provide access to U.S. IOOS data through DMAC-compliant services. The Registry performs the following operations to support the registration process:
 
 1. harvesting metadata from submitted URLs of the DMAC services or Web Accessible Folders (WAFs); 
 2. transforming metadata to ISO, generate and maintain WAFs for the ISO metadata records;
 3. injecting the ISO metadata into NGDC Geoportal.  
  
Upon completion of the Registry's operations, the IOOS data can be discovered through the Web-based [NGDC Geoportal interface](http://www.ngdc.noaa.gov/geoportal/catalog/main/home.page).  

The primary client for the service registry is the [IOOS Catalog](http://catalog.ioos.us).  The IOOS Catalog fetches metadata from the Geoportal through CSW interface.  For more about the Catalog, go to the [catalog repository on github](http://github.com/ioos/catalog) for information and code supporting the generation of the catalog.   

This repository is the primary source for documentation for the registration process and contains steps, process descriptions, and examples. 

The [IOOS Service Registration Webinar](https://mmancusa.webex.com/mmancusa/ldr.php?RCID=1ed739d35c7dc0de30ec03a3a7d0e086) was presented by Anna Milan from NGDC, and recorded in February 2014.  

# Steps for registering IOOS Service Metadata

The following resources can be registered with the Registry ([**Figure 1**](#Fig_1)): 

   * _**SOS**_ and _**WMS**_ --  by means of a single getCapabilities request, e.g.
     *  http://sos.aoos.org/sos/service?service=SOS&request=GetCapabilities&AcceptVersions=1.0.0)
     *  http://www.neracoos.org/thredds/wms/WW3/fine.nc?service=WMS&version=1.3.0&request=GetCapabilities
   * _**THREDDS**_ -- by means of an XML catalog document (e.g. http://dm2.caricoos.org/thredds/catalog/swan/catalog.xml). 

   > **NOTE**: The individual catalogs must be registered; a catalog that just points to other catalogs cannot be registered --- although the catalog tree is crawled for all child datasets, the other catalogs referenced by `CatalogRef` are ignored.

   * _**ERDDAP**_ -- by means of an XML list of ISO records, e.g. http://erddap.secoora.org/erddap/metadata/iso19115/xml/.
   * _**WAF**_ -- a Web Accessible Folder that contains ISO metadata XML documents, e.g. http://www.neracoos.org/WAF/iso/
  
   > **NOTE**: WAFs offer the most control over the metadata that will appear in the registry, but require some efforts to create and maintain.

<br> 
<a name="Fig_1"></a>![Registration process](/images/catalog_proposed_architecture_luke_web.png) 
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**Figure 1: Proposed IOOS Registry and Catalog Architecture**


 
## 1. Submitting URL of a Service or WAF to the Collection Source Table
  
The registration process starts with submitting a request on the [**issue tracker**](https://github.com/ioos/registry/issues?state=open) in the IOOS Registry Github Repository.  

For each request, the following information should be included:
    
* A URL of the Service or the Web Accessible Folder (WAF), for example: 
  * SOS URL: http://sos.aoos.org/sos/sos/kvp?service=SOS&request=GetCapabilities&AcceptVersions=1.0.0 
  * THREDDS URL: http://sos.maracoos.org/stable/agg_catalogs/weatherflow_agg_catalog.xml
  * ISO WAF: http://www.neracoos.org/WAF/UMaine/iso/
* Service Point of Contact - the person responsible for maintaining the service or that person's supervisor. 
* Service Organization, i.e. IOOS Regional Association (e.g. NERACOOS) or Federal Partner (e.g. NOAA CO-OPS).

The URL submitted is manually added to the [Collection Source Table](https://www.ngdc.noaa.gov/docucomp/collectionSource/list?&layout=fluid), and the service is listed as "**submitted**". 

## 2. Harvesting and Converting Metadata

* The **submitted** URL or WAF is harvested each night at 19:30 MT/21:30 ET into the first of two NGDC WAFs.  The first WAF is the [_**test**_ WAF](http://www.ngdc.noaa.gov/metadata/published/test/NOAA/IOOS/).
* On day 2 (a day after the initial harvesting), the source metadata is transformed to ISO 19115-2 and will populate the  `*/iso` [_**test**_ WAF](http://www.ngdc.noaa.gov/metadata/published/test/NOAA/IOOS/) by 07:15 MT/ 09:15 ET the next day.    
* The WAF is manually checked to verify the harvest has been successful, and the service status in the [Collection Source Table](https://www.ngdc.noaa.gov/docucomp/collectionSource/list?&layout=fluid) is manually changed from **submitted** to **approved** .  
* Harvesting is considered completed when the metadata records appear in the RA's `*/iso` WAF, and pass ISO 19139 schema validation.

    >**NOTE**: the variety of the transformation XSL style sheets used for metadata conversion to ISO 19115-2 are also available for download and local use:
 
    > * SOS Get Capabilities to ISO: http://www.ngdc.noaa.gov/metadata/published/xsl/SensorObservationService/SOS2ISO_MI.xsl  
    > * WMS Get Capabilities to ISO: http://www.ngdc.noaa.gov/metadata/published/xsl/wms1.3_to_isoSV.xsl   
    > * ncISO 
    >    * GitHub (Master): https://github.com/Unidata/threddsIso/blob/master/src/main/resources/xsl/nciso/UnidataDD2MI.xsl
    >    * NGDC copy: http://www.ngdc.noaa.gov/metadata/published/xsl/nciso2.0/UnidataDD2MI.xsl
  


## 3. Publishing Metadata

* On day 3 (a day after the status has been changed to "approved ") by 07:15 MT/ 09:15 ET, the ISO metadata records are automatically posted to a [**production** WAFs](http://www.ngdc.noaa.gov/metadata/published/NOAA/IOOS/) in EMMA. 
* The [NGDC Geoportal](http://www.ngdc.noaa.gov/geoportal/catalog/main/home.page) automatically harvests ISO records from the  [**production** WAFs](http://www.ngdc.noaa.gov/metadata/published/NOAA/IOOS/) by 09:00 MT/ 11:00 ET.   
 
The Service Registration process ends when a valid ISO record has been created and added to the production WAF.  The metadata should be able to be found in the [NGDC Geoportal](http://www.ngdc.noaa.gov/geoportal/catalog/main/home.page).  

## 4. Harvesting Metadata by the IOOS Catalog 

The [IOOS Catalog](http://catalog.ioos.us/) queries the  [NGDC Geoportal](http://www.ngdc.noaa.gov/geoportal/catalog/main/home.page) daily for a list of services registered to IOOS.  These services are then individually queried by the Catalog and their metadata is harvested and indexed.

Briefly, the current (03 October 2014) harvest schedule for the catalog is:

  * harvests start nightly at 7:10am UTC (2:10am eastern)
  * cleanups start nightly at 8:10am UTC (3:10am eastern)
  * reindex daily daily at 6:30am UTC (1:30am eastern)
  * daily status emails at 6:20am UTC (1:20am eastern)

See the Catalog [documentation](http://github.com/ioos/catalog) for more information on what is indexed and how.  

# Registration Process FAQ

## How do I check to see if my metadata has been registered?

* Review metadata and assessments of metadata in [EMMA](http://www.ngdc.noaa.gov/docucomp/page?view=wafsInGroup&title=Metrics%20and%20Collections%20for%20Group%20IOOS&groupName=IOOS)
* [Search](http://www.ngdc.noaa.gov/geoportal/catalog/search/search.page) or [Browse](http://www.ngdc.noaa.gov/geoportal/catalog/search/browse/browse.page) metadata in [Geoportal](http://www.ngdc.noaa.gov/geoportal).
* Look up the relevant data in [IOOS Catalog](http://catalog.ioos.us/).   

## How do I update or remove metadata in the service registry?

The registry augments today's harvest with all previous harvests and sometimes this can result in old out-of-date records that are no longer applicable.
 
 1. If the content of the service has changed, but the service should still be registered then: 
  * Create an issue in the [github IOOS/ Registry repository](https://github.com/ioos/registry/issues) or send an email to ioos.catalog@noaa.gov requesting a 'clean out' of a particular service or WAF. 
  * The NGDC administrator will manually set a flag to remove ALL previous metadata records before harvest. This will result in an entirely new refresh of the content for that web accessible folder (WAF).


 2. If the service is outdated, and should no longer be registered with IOOS then: 
  * Send an email to ioos.catalog@noaa.gov requesting that the service be removed. 
  * The service status will be changed to 'For Removal'. 
  * The NGDC administrator will manually set a flag to remove ALL previous metadata records before harvest. This will result in an entirely new refresh of the content for that web accessible folder. 
  * The NGDC admin will change the service status to 'Removed'. 


## How do I register a THREDDS catalog that contain `catalogRef` elements?

In THREDDS catalogs it is common to use `catalogRef` elements to reference other catalogs, especially in the [top level catalog](http://gis.stackexchange.com/questions/70919/setting-up-thredds-catalogs-for-ocean-model-data).  The NGDC crawler is currently not following `catalogRef` links, however.  So if you had a top level catalog that looked like this:

```
<catalog xmlns="http://www.unidata.ucar.edu/namespaces/thredds/InvCatalog/v1.0"
    xmlns:xlink="http://www.w3.org/1999/xlink"
    name="THREDDS Top Catalog, points to other THREDDS catalogs" version="1.0.1">

    <dataset name="NCSU MEAS THREDDS catalogs">
        <catalogRef xlink:href="gomtox_catalog.xml" xlink:title="GOMTOX (Gulf of Maine) Ocean Model" name=""/>
        <catalogRef xlink:href="sabgom_catalog.xml" xlink:title="SABGOM (South Atlantic Bight and Gulf of Mexico) Ocean Model" name=""/>
    </dataset>

</catalog>
```

and you wanted all the content to be harvested, you should not submit the top level catalog (which would harvest nothing). 
 
So, if a top level URL looks something like
 
  * http://ra.org/thredds/catalog/catalog.xml,  

then Instead of that top level URL, you should submit the child catalog URLs for harvesting.  An example of a child catalog URL would be like this

  * http://ra.org/thredds/catalog/content/catalog.xml.


## Where can I find the UUIDs of the registered metadata collections?

To search for metadata on Geoportal, you must submit the metadata collection `sys.siteuuid` parameter for each query. The Master UUID listing for Geoportal metadata collections (e.g. RAs) is located in [IOOS/Registry repository](https://github.com/ioos/registry/blob/master/uuid.csv) on GitHub.


## How can I search for the registered metadata on the NGDC Geoportal?

The metadata stored in the WAFs can be searched through via either the NGDC Geoportal's REST API or OGC Catalog Service for Web interface. Below are selected examples of the search queries that can be issued to the NGDC Geoportal to search for IOOS Regional Association assets.   
Additional information on the Geoportal in general can be found on the [ESRI Sourceforge site](https://sourceforge.net/apps/mediawiki/geoportal/index.php?title=REST_API_Syntax)

### Search via Geoportal REST interface  

#### Date Search Example

Search within the PacIOOS collection for records with start date 2009-02-01 to end date 2012-02-01 with JSON response: 

[PacIOOS Date Search Example Link](http://www.ngdc.noaa.gov/geoportal/rest/find/document?rid=local&ridName=NOAA%27s%20Geophysical%20Data%20Center&searchText=startDate%3A%5B1800-01-01%20TO%202012-02-01%5D%20AND%20endDate%3A%5B2009-02-01%20TO%202100-01-01%5D%20%20AND%20sys.siteuuid%3A%22%68FF11D8-D66B-45EE-B33A-21919BB26421%7D%22&start=1&max=10&orderBy=relevance&maxSearchTimeMilliSec=10000&f=pjson)

Decoded Parameters: 

```
rid=local&ridName=NOAA's Geophysical Data Center&searchText=startDate:[1800-01-01 TO 2012-02-01] AND endDate:[2009-02-01 TO 2100-01-01] AND sys.siteuuid:"{68FF11D8-D66B-45EE-B33A-21919BB26421}"&start=1&max=10&orderBy=relevance&maxSearchTimeMilliSec=10000&f=pjson
```

#### Keyword Example

Search within the PacIOOS WAF for keywords `sea_water_salinity` with JSON response: 

[PacIOOS Keyword Example Link](http://www.ngdc.noaa.gov/geoportal/rest/find/document?rid=local&ridName=NOAA%27s%20Geophysical%20Data%20Center&searchText=keywords%3A%20sea_water_salinity%20AND%20sys.siteuuid%3A%22%68FF11D8-D66B-45EE-B33A-21919BB26421%7D%22&start=1&max=1000&orderBy=relevance&maxSearchTimeMilliSec=10000&f=pjson)

Decoded Parameters: 

```
rid=local&ridName=NOAA's Geophysical Data Center&searchText=keywords: sea_water_salinity AND sys.siteuuid:"{68FF11D8-D66B-45EE-B33A-21919BB26421}"&start=1&max=1000&orderBy=relevance&maxSearchTimeMilliSec=10000&f=pjson
```

#### Geographic Search Example

Search within the PacIOOS WAF for metadata records within -164.9246,16.6012,-149.4899,25.3959 with JSON response: 

[PacIOOS Geo Example Link](http://www.ngdc.noaa.gov/geoportal/rest/find/document?rid=local&ridName=NOAA%27s%20Geophysical%20Data%20Center&searchText=sys.siteuuid%3A%22%68FF11D8-D66B-45EE-B33A-21919BB26421%7D%22&start=1&max=1000&orderBy=relevance&spatialRel=esriSpatialRelWithin&bbox=-164.9246,16.6012,-149.4899,25.3959&maxSearchTimeMilliSec=10000&f=pjson)

Decoded Parameters: 

```
rid=local&ridName=NOAA's Geophysical Data Center&searchText=sys.siteuuid:"{68FF11D8-D66B-45EE-B33A-21919BB26421}"&start=1&max=1000&orderBy=relevance&spatialRel=esriSpatialRelWithin&bbox=-164.9246,16.6012,-149.4899,25.3959&maxSearchTimeMilliSec=10000&f=pjson
```

#### Multi-Criteria Example

Search within the PacIOOS WAF for metadata records with all of the above with JSON response: 

[PacIOOS Multi Example Link](http://www.ngdc.noaa.gov/geoportal/rest/find/document?rid=local&ridName=NOAA%27s%20Geophysical%20Data%20Center&rids=local&searchText=keywords%3A%20sea_water_salinity%20AND%20endDate%3A%5B2009-02-01%20TO%202100-01-01%5D%20AND%20startDate%3A%5B1800-01-01%20TO%202012-02-01%5D%20AND%20sys.siteuuid%3A%22%68FF11D8-D66B-45EE-B33A-21919BB26421%22&start=1&max=1000&orderBy=relevance&spatialRel=esriSpatialRelWithin&bbox=-164.9246,16.6012,-149.4899,25.3959&maxSearchTimeMilliSec=10000&f=pjson)

Decoded Parameters: 

```
rid=local&ridName=NOAA's Geophysical Data Center&rids=local&searchText=keywords: sea_water_salinity AND endDate:[2009-02-01 TO 2100-01-01] AND startDate:[1800-01-01 TO 2012-02-01] AND sys.siteuuid:"{68FF11D8-D66B-45EE-B33A-21919BB26421}"&start=1&max=1000&orderBy=relevance&spatialRel=esriSpatialRelWithin&bbox=-164.9246,16.6012,-149.4899,25.3959&maxSearchTimeMilliSec=10000&f=pjson
```

#### WMS Example

Search within the PacIOOS WAF for metadata records with a WMS service endpoint with GeoRSS response: 

[PacIOOS WMS Example Link](http://www.ngdc.noaa.gov/geoportal/rest/find/document?rid=local&ridName=NOAA%27s%20Geophysical%20Data%20Center&rids=local&searchText=sys.siteuuid%3A%22%68FF11D8-D66B-45EE-B33A-21919BB26421%22%20AND%20wms.resource.url%3A*&start=1&max=1000&orderBy=relevance&maxSearchTimeMilliSec=10000&f=georss)

Decoded Parameters: 

```
rid=local&ridName=NOAA's Geophysical Data Center&rids=local&searchText=wms.resource.url:* AND sys.siteuuid:"{68FF11D8-D66B-45EE-B33A-21919BB26421}"&start=1&max=1000&orderBy=relevance&maxSearchTimeMilliSec=10000&f=georss
```

#### WCS Example

Search within the PacIOOS WAF for metadata records with a WCS service endpoint with html response: 
[PacIOOS WCS Example Link](http://www.ngdc.noaa.gov/geoportal/rest/find/document?rid=local&ridName=NOAA%27s%20Geophysical%20Data%20Center&rids=local&searchText=wcs.resource.url:*%20AND%20sys.siteuuid%3A%22%68FF11D8-D66B-45EE-B33A-21919BB26421%22&start=1&max=1000&orderBy=relevance&maxSearchTimeMilliSec=10000&f=html)

Decoded Parameters: 

```
rid=local&ridName=NOAA's Geophysical Data Center&rids=local&searchText=wcs.resource.url:* AND sys.siteuuid:"{68FF11D8-D66B-45EE-B33A-21919BB26421}"&start=1&max=1000&orderBy=relevance&maxSearchTimeMilliSec=10000&f=html
```

#### SOS Example 

Search within the PacIOOS WAF for metadata records with an SOS service endpoint with html response: 
[PacIOOS SOS Example Link](http://www.ngdc.noaa.gov/geoportal/rest/find/document?rid=local&ridName=NOAA%27s%20Geophysical%20Data%20Center&rids=local&searchText=sos.resource.url:*%20AND%20sys.siteuuid%3A%22%68FF11D8-D66B-45EE-B33A-21919BB26421%22&start=1&max=1000&orderBy=relevance&maxSearchTimeMilliSec=10000&f=html)

Decoded Parameters: 

```
rid=local&ridName=NOAA's Geophysical Data Center&rids=local&searchText=sos.resource.url:* AND sys.siteuuid:"{68FF11D8-D66B-45EE-B33A-21919BB26421}"&start=1&max=1000&orderBy=relevance&maxSearchTimeMilliSec=10000&f=html
```

#### OPeNDAP Example 

Search within the PacIOOS WAF for metadata records with a OpenDAP service endpoint with html response: 

[PacIOOS OPeNDAP Example Link](http://www.ngdc.noaa.gov/geoportal/rest/find/document?rid=local&ridName=NOAA%27s%20Geophysical%20Data%20Center&rids=local&searchText=odp.resource.url:*%20AND%20sys.siteuuid%3A%22%68FF11D8-D66B-45EE-B33A-21919BB26421%22&start=1&max=1000&orderBy=relevance&maxSearchTimeMilliSec=10000&f=html)

Decoded Parameters: 

```
rid=local&ridName=NOAA's Geophysical Data Center&rids=local&searchText=odp.resource.url:* AND sys.siteuuid:"{68FF11D8-D66B-45EE-B33A-21919BB26421}"&start=1&max=1000&orderBy=relevance&maxSearchTimeMilliSec=10000&f=html
```

### Search via CSW interface 

The CSW queries must be issued to the [NGDC CSW Service Endpoint](http://www.ngdc.noaa.gov/geoportal/csw) as XML POST requests.

#### ISO Date Modified Search

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

#### WMS Search Example

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

#### WCS Search Example

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

#### OPeNDAP Search Example

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

#### SOS Search Example

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
