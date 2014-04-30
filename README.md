Updates
=============================

IOOS Service Registry Webinar
Anna Milan gave a presentation on the IOOS Service Registry on March 13 for the IOOS data management community.  A recording of the webinar is available from the url:  https://mmancusa.webex.com/mmancusa/ldr.php?RCID=1ed739d35c7dc0de30ec03a3a7d0e086.

In this webinar, Anna covered the processes that takes place behind the scenes when a service is submitted to the registry.  She described NGDC's Enterprise Metadata Management Architecture (EMMA).  There was a great discussion.  Please view the Webinar if you are interested in learning more.   

IOOS Service Registry at NGDC
=============================

The Service Registry (aka the registry) provides the master list of data sets available via DMAC data access services.  The registry is the official record of what is included in U.S. IOOS.  It is hosted and operated by NGDC and provides a web based discovery interface for IOOS data. 

Functionally the service registry comprises the metadata harvesting process, the WAF generation and maintenance, and the publication of metadata records into the NGDC Geoportal. The registry work is carried out at NGDC. The Geoportal CSW interface is the main interface to the metadata. I think the purpose of this repo is to primarily serve as the documentation home and as the issue tracker. The issue tracker should be used for all metadata and harvesting related issues.

The catalog repository serves as the software repository for all code supporting the generation of the catalog.ioos.us web site. This web site is intended to summarize the data that is published on the services visible in the Servie Registry. The interface between the registry and the catalog is the CSW interface to the NGDC geoportal. The repository and issue tracker for the catalog should probably be limited to issues with the code and not procedural issues related to harvesting RA or other partner metadata. Also, let's move the uuid.csv file from the catalog repository to the regsitry repository.

TODO: Describe the layout using the figure below (Harvestors, XSLT, ncISO, WAF, Metrics and Tools, Geoportal)  

TODO: Include a section on the Geoportal including examples of querying the server that are listed here. https://geo-ide.noaa.gov/wiki/index.php?title=ESRI_Geoportal#IOOS_WAFs

TODO: Decide where the master list of Geoportal collection UUIDs will be hosted.  Currently on the wiki page above AND on the github.com/ioos/catalog repo.  Pick one as master. https://github.com/ioos/catalog/blob/master/uuid.csv

  
![Registeration Process Workflow](https://raw.githubusercontent.com/ioos/registry/master/doc/images/IoosWorkFlow12172013(8).png)

figure 1. IOOS service registration steps

###How do I submit datasets for harvesting?

Send a message to  ioos.catalog@noaa.gov including these 3 pieces of information:

1. Service URL or WAF
   * WAF: A web accessible folder containing ISO metadata XML documents.  This offers the most control over the metadata that will appear in the registry, but requires effort to create and maintain. Example: http://www.neracoos.org/WAF/iso/
   * THREDDS: A THREDDS catalog using the .xml extension.  The catalog tree is crawled for all child datasets, but other catalogs referenced by CatalogRed are not followed.   Thus registering a catalog that just points to other catalogs will not work.  The individual catalogs must be registered.  Example: http://dm2.caricoos.org/thredds/catalog/swan/catalog.xml
   * SOS: A single getCapabilities file. Example: http://sos.maracoos.org/stable/sos/wflow1127-agg.ncml
   * ERDDAP: The list of ISO records provided by ERDDAP. Example: http://erddap.secoora.org/erddap/metadata/iso19115/xml/
   * WMS: A single getCapabilities file. Example: http://www.neracoos.org/thredds/wms/WW3/fine.nc?service=WMS&version=1.3.0&request=GetCapabilities
2. Point of contact: The person responsible for maintaining the service, or that person's supervisor
3. Affilitation: Regional Association (NERACOOS) or Federal Partner (e.g. NOAA CO-OPS).


### What happens then?
* The URL is manually added to the Service Registry [collection source table](https://www.ngdc.noaa.gov/docucomp/collectionSource/list?&layout=fluid).  It is listed as "submitted"  
* The "submitted" service metadata will be automatically harvested into a [test Web Accessible Folder (WAF)](http://www.ngdc.noaa.gov/metadata/published/test/NOAA/IOOS/). This harvest begins each evening around 1930 MT. 
* If successfully harvested, the test WAF is populated with an ISO metadata record by 0715 MT.  The WAF is manually checked, the next day, to verify the harvest has been successful.  The service status is changed from "submitted" to "approved" in the collection source table. Harvest is considered 'successful' when the metadata records appear in the */iso WAF and pass ISO 19139 schema validation. 
* The [production WAF](http://www.ngdc.noaa.gov/metadata/published/NOAA/IOOS/) in EMMA is automatically populated on day 3 (day after the status has been changed to "approved ") by 0715.  
* The [NGDC Geoportal] (http://www.ngdc.noaa.gov/geoportal/catalog/main/home.page) automatically harvests records from the WAF around 0900. 
* The [IOOS Catalog](http://catalog.ioos.us/) will automatically harvest records from the NGDC Geoportal every 8 hours. 

![Registration process](https://raw.github.com/ioos/registry/master/doc/images/IOOS%20Harvest%20Process.png)
figure 2. 

*  Other addtional pathways to consider for registering new data using the issues in this repository.  Is this feasible?

### How do I check the results?
* Review metadata and assessments of metadata in EMMA - http://www.ngdc.noaa.gov/docucomp/page?view=wafsInGroup&title=Metrics%20and%20Collections%20for%20Group%20IOOS&groupName=IOOS
* Search or Browse metadata in Geoportal - http://www.ngdc.noaa.gov/geoportal
* Search for metadata in IOOS Catalog

### How do I remove previously harvested metadata?
The registry augments today's harvest with all previous harvests and sometimes this can result in old out-of-date records that are no longer applicable. 

1. If the content of the services has changed, but the service should still be registered then: 
  * send an email to ioos.catalog@noaa.gov requesting a 'clean out' of a particular service or WAF. 
  * The administrator at NGDC will then manually set a flag that will remove ALL previous metadata records before harvest. This will result in an entirely new refresh of the content for that web accessible folder. 
  
2. If the service is out date and should no longer be registered with IOOS then: 
  * send an email to ioos.catalog@noaa.gov requesting that the service be removed. 
  * Rob will change the status of the service to 'For Removal' 
  * The administrator at NGDC will then manually set a flag that will remove ALL previous metadata records before harvest. This will result in an entirely new refresh of the content for that web accessible folder. 
  * The admin at NGDC will then change the status of the servic in the Collection Source table to 'Removed'. 


ESRI Geoportal
=

How to find data through common, simple searches: 
@amilan17: Thanks. Good to know both how to expose the RA name/acronym to common, simple searches, and to see that the GeoPortal Browse section has provider/collection browsing. @robragsdale, both pieces of information would be helpful on the registry documentation.



[Master UUID list](https://github.com/ioos/registry/blob/master/uuid.csv)
=

IOOS WAF Search Examples: 

Date Search Example: Search within the PacIOOS WAF for records with start date 2009-02-01 to end date 2012-02-01 with JSON response: PacIOOS DateSearchExample Link
Decoded Parameters: rid=local&ridName=NOAA's Geophysical Data Center&searchText=startDate:[1800-01-01 TO 2012-02-01] AND endDate:[2009-02-01 TO 2100-01-01] AND sys.siteuuid:"{68FF11D8-D66B-45EE-B33A-21919BB26421}"&start=1&max=10&orderBy=relevance&maxSearchTimeMilliSec=10000&f=pjson

Keyword Example: Search within the PacIOOS WAF for keywords 'sea_water_salinity' with JSON response: PacIOOS KeywordExample Link
Decoded Parameters: rid=local&ridName=NOAA's Geophysical Data Center&searchText=keywords: sea_water_salinity AND sys.siteuuid:"{68FF11D8-D66B-45EE-B33A-21919BB26421}"&start=1&max=1000&orderBy=relevance&maxSearchTimeMilliSec=10000&f=pjson

Geographic Search Example: Search within the PacIOOS WAF for metadata records within -164.9246,16.6012,-149.4899,25.3959 with JSON response: PacIOOS GeoExample Link
Decoded Parameters: rid=local&ridName=NOAA's Geophysical Data Center&searchText=sys.siteuuid:"{68FF11D8-D66B-45EE-B33A-21919BB26421}"&start=1&max=1000&orderBy=relevance&spatialRel=esriSpatialRelWithin&bbox=-164.9246,16.6012,-149.4899,25.3959&maxSearchTimeMilliSec=10000&f=pjson

Multi-Criteria Example: Search within the PacIOOS WAF for metadata records with all of the above with JSON response: PacIOOS MultiExample Link
Decoded Parameters: rid=local&ridName=NOAA's Geophysical Data Center&rids=local&searchText=keywords: sea_water_salinity AND endDate:[2009-02-01 TO 2100-01-01] AND startDate:[1800-01-01 TO 2012-02-01] AND sys.siteuuid:"{68FF11D8-D66B-45EE-B33A-21919BB26421}"&start=1&max=1000&orderBy=relevance&spatialRel=esriSpatialRelWithin&bbox=-164.9246,16.6012,-149.4899,25.3959&maxSearchTimeMilliSec=10000&f=pjson

WMS Example: Search within the PacIOOS WAF for metadata records with a WMS service endpoint with GeoRSS response: PacIOOS WMSExample Link
Decoded Parameters: rid=local&ridName=NOAA's Geophysical Data Center&rids=local&searchText=wms.resource.url:* AND sys.siteuuid:"{68FF11D8-D66B-45EE-B33A-21919BB26421}"&start=1&max=1000&orderBy=relevance&maxSearchTimeMilliSec=10000&f=georss

WCS Example: Search within the PacIOOS WAF for metadata records with a WCS service endpoint with html response: PacIOOS WCSExample Link
Decoded Parameters: rid=local&ridName=NOAA's Geophysical Data Center&rids=local&searchText=wcs.resource.url:* AND sys.siteuuid:"{68FF11D8-D66B-45EE-B33A-21919BB26421}"&start=1&max=1000&orderBy=relevance&maxSearchTimeMilliSec=10000&f=html

SOS Example: Search within the PacIOOS WAF for metadata records with an SOS service endpoint with html response: PacIOOS SOSExample Link
Decoded Parameters: rid=local&ridName=NOAA's Geophysical Data Center&rids=local&searchText=sos.resource.url:* AND sys.siteuuid:"{68FF11D8-D66B-45EE-B33A-21919BB26421}"&start=1&max=1000&orderBy=relevance&maxSearchTimeMilliSec=10000&f=html

OPeNDAP Example: Search within the PacIOOS WAF for metadata records with a OpenDAP service endpoint with html response: PacIOOS OPeNDAPExample Link
Decoded Parameters: rid=local&ridName=NOAA's Geophysical Data Center&rids=local&searchText=odp.resource.url:* AND sys.siteuuid:"{68FF11D8-D66B-45EE-B33A-21919BB26421}"&start=1&max=1000&orderBy=relevance&maxSearchTimeMilliSec=10000&f=html


The [Master uuid document](https://github.com/ioos/registry/blob/master/uuid.csv)

GeoPortal CSW Documentation
=

NGDC CSW Service Endpoint
http://www.ngdc.noaa.gov/geoportal/csw
The examples below must be made as an XML POST request.

PacIOOS WAF
=
WMS Search Example:
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

WCS Search Example:
=
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

OPeNDAP Search Example:
=
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

SOS Search Example:
=
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

ISO Date Modified Search
=
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

* How long does it take for a service to show up in a WAF after it is approved?
   * A new service should show up within 24 hours after it is approved.

* Is the harvesting process manual or automatic?
   * Harvesting runs automatically every night. 

* When does tne metadata harvest take place?
   * The metadata should be in the test WAF by 0715 on day 2.  DEFINE WHAT THIS MEANS.
   * 
* What is the relationship between the Service Registry and the [IOOS Catalog](http://catalog.ioos.us)? 
   * A web based catalog of IOOS services and datasets, available at  ....

* What is the relationship between the IOOS Service Registry and data.gov, GEOSS, and other national or international catalog efforts?
* What is the difference between a data set and data service?
   * TODO: Ask for input from the group.














# Old Material

IOOS Catalog Registration Process from EDM Wiki

The IOOS Catalog provides access to data from observing system platforms and data services. Take these steps to include your data and services in the catalog.
The Process

Use a Sensor Observation Service (SOS), Web Map Service (WMS), Unidata's THREDDS or ERDDAP to provide access to your data.
Review your metadata and add/update content as appropriate - Guidance for IOOS Data Providers
After updating your metadata, you may register your service by sending an email to ioos.catalog@noaa.gov.
provide Service Endpoint URL, Service Type, IOOS Region
The IOOS Registry/Catalog/Viewer team will add your service to Collection Source Table.
Then your service will be tested, the translated metadata will be reviewed and change requests may be sent back. Once the service is approved routine harvesting will begin.
Upon harvest ISO 19115-2 compliant metadata records are generated and a copy of the individual records are stored and assessed at EMMA.
The individual records are also loaded into the NGDC instance of the ESRI Geoportal Server.
The data or service is added to the IOOS Data Catalog and System Viewer
The service is added to the NOAA GEO-IDE Unified Access Framework catalog hosted here. Discussion item?
THREDDS Data Servers

Upgrade to a version of THREDDS with ncISO support (v >= 4.2.?)
Enable the ncISO service documented here.
For THREDDS based services, we require your NetCDF files and THREDDS metadata follow CF conventions and the Attribute Conventions for Dataset Discovery. Additional global attributes within the files are allowed, but the ACDD conventions are the minimal requirements for automatic generation of ISO 19115-2 metadata documents.
If your data contains in situ observations we recommend using the Climate and Forecast Conventions (9v.16 or later) Discrete Sampling Geometries.
OGC Sensor Observation Service

For SOS based services, we recommend the GetCapabilities document include metadata documented here.
A stylesheet is available for translating the Sensor Observation Service capabilities document into ISO 19139. Comments or suggestions for improvements should be addressed to EMMA email support.
