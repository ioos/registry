IOOS Service Registry at NGDC
=============================

The Service Registry (aka the registry) provides the master list of data sets available via DMAC data access services.  The registry is the official record of what is included in U.S. IOOS.  The service registry is hosted and operated by NGDC and provides a web based discovery interface for IOOS data.    

TODO: Describe the layout using the figure below (Harvestors, XSLT, ncISO, WAF, Metrics and Tools, Geoportal)
TODO: Include a section on the Geoportal including examples of querying the server that are listed here. https://geo-ide.noaa.gov/wiki/index.php?title=ESRI_Geoportal#IOOS_WAFs
TODO: Decide where the master list of Geoportal collection UUIDs will be hosted.  Currently on the wiki page above AND on the github.com/ioos/catalog repo.  Pick one as master. https://github.com/ioos/catalog/blob/master/uuid.csv

# Service Registry FAQs
The IOOS Catalog Registration Process on NOAA EDM Wiki:  https://geo-ide.noaa.gov/wiki/index.php?title=IOOS_Catalog_Registration_Process

Registration Process Image from Anna Milan: https://drive.google.com/file/d/0B0uvBG1lfvfMd1ZtTWR4UUY2Q2M/edit?usp=sharing


![Registration process](https://raw.github.com/ioos/registry/master/doc/images/IOOS%20Harvest%20Process.png)


![More registration process](https://github.com/ioos/registry/blob/master/doc/images/IoosWorkFlow20140312.png)


*  How do I contribute my data? -OR- How is a service submitted to the registry?  
   * An email with the service url (or WAF location), point of contact and organization should be sent to ioos.catalog@noaa.gov
   * TODO: consider including an additional pathway for registering new data using the Issues in this repo.  Is this feasible today?
* What happens after I submit my service URL?
   * TODO: Verify that the list below is exactly describing whichever figure we choose to insert above.

1. Submit new data service via email to IOOS.Catalog@noaa.gov and include the service URL, point of contact and service affilitation.
2. The servicThe status of the service will be manually changed from "submitted" to "approved".e URL is manually added to the Service Registry [collection source table] (https://www.ngdc.noaa.gov/docucomp/collectionSource/list?&layout=fluid).  It is listed as "submitted"  
3. The "submitted" service metadata will be automatically harvested into a [test Web Accessible Folder (WAF)} (http://www.ngdc.noaa.gov/metadata/published/test/NOAA/IOOS/).  
4. If successfully harvested, the test WAF is populated with an ISO metadata record by 0715.  The WAF is manually checked to verify the harvest has been successful.  The service status is changed from "submitted" to "approved" in the collection source table.  
5. The production WAF in EMMA is automatically populated on day 3 (day after the status has been changed to "approved") by 0715.  
6. The NGDC Geoportal automatically harvests records from the WAF by 0900.  
5. 
5. The 
5. 
6. The service
4. 
5. by Test metadata harvest happens - by 7:15 am on Day 2, automatic
3. Approval of new data service check status in EMMA - Day 2, manual
4. WAF is populated with new ISO record - Day 3 by 7:15 am, automatic
5. Geoportal harvests records from WAF - Day 3 by 9 am, automatic
6. IOOS Catalog queries Geoportal for all IOOS service URLs -?
7. IOOS Catalog harvests from individual services and generates dataset records. - ?
8. After a service URL is approved, steps 4 and 5 occur daily at the same time.

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
