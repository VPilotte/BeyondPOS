# Beyond POS PO Package

[![N|Solid](https://www.beyondtechnologies.com/wp-content/themes/beyond/images/beyond.png)](https://www.beyondtechnologies.com/)

This package contains everything in order to setup GK POS integration.

  - GK TLog mapping with [XSLT]
  - Documentation for Master Data replication
  - Documentation to setup [SOAP] Web Services used by GK

### GK TLog mapping with XSLT

GK TLog mapping is done with XSLT. There are 2 files imported in mapping 
- GKPOSLog2POSDM_PQ5 : Main Logic
- GKPOSLog2POSDMValueMapping_PQ5 : Value Mappings

Make sure that proper version of GK's TLog version is used in the main XSLT stylesheet file. Here's an example of using version 2.2:

```sh
<xsl:stylesheet xmlns:xsl="http://www.w3.org/1999/XSL/Transform"
xmlns:SES="http://www.gk-software.com/storeweaver/sdc/pos_upload/ses_pos_upload/2.2"
xmlns:datatype="http://www.gk-software.com/storeweaver/sdc/pos_upload/ixr_datatypes/2.2"
xmlns:ns0="http://www.gk-software.com/storeweaver/sdc/pos_upload/pos_upload/2.2" 
xmlns:xs="http://www.w3.org/2001/XMLSchema" 
version="2.0" 
extension-element-prefixes="ns0 SES">
```

RFC used on CAR's side is a standard function module : 
```sh
/POSDW/CREATE_TRANSACTIONS_EXT 
```

If you ever need to do some logic on CAR's side, the RFC can be copied into a custom one. Changes will need to be done in PO but also in the XSLT. 
```sh
<ns1:_-POSDW_-CREATE_TRANSACTIONS_EXT xmlns:ns1="urn:sap-com:document:sap:rfc:functions">
```
Target RFC would need to be changed for the custom one you created, example : 
```sh
<ns1:_ZCREATE_TRANSACTIONS_EXT xmlns:ns1="urn:sap-com:document:sap:rfc:functions">
```

* [] - HTML enhanced for web apps!
* [Ace Editor] - awesome web-based text editor


And of course Dillinger itself is open source with a [public repository][dill]
 on GitHub.

### Documentation for Master Data replication

Master Data replication to GK can be done two ways, depending on the complexity of data sent. 
 - Standard IDocs
 - GK's internal structure

For most projects, Standard IDocs will be used. Complexity needs to be evaluated prior to evaluation and setting those up, as using GK's internal structure requires mapping and takes a lot more time to setup.

Install the dependencies and devDependencies and start the server.

```sh
$ cd dillinger
$ npm install -d
$ node app
```

For production environments...

```sh
$ npm install --production
$ NODE_ENV=production node app
```

### Documentation to setup SOAP Web Services used by GK

On the S/4HANA side, [SOAP] Web Services need to be activated. This is done through transaction [SOAMANAGER].
SOAP Runtime needs to be properly configured prio to testing those services. Transaction SRT_UTIL can be used to check on the configuration. 
If this isn't done, Basis resources needs to do the setup.

| Web Service | Endpoint |
| ------ | ------ |
| Dropbox | [plugins/dropbox/README.md][PlDb] |
| Github | [plugins/github/README.md][PlGh] |
| Google Drive | [plugins/googledrive/README.md][PlGd] |
| OneDrive | [plugins/onedrive/README.md][PlOd] |
| Medium | [plugins/medium/README.md][PlMe] |
| Google Analytics | [plugins/googleanalytics/README.md][PlGa] |


### Todos

 - This will be the list of todos

License
----

SAP
Beyond Technologies


[//]: # (These are reference links used in the body of this note and get stripped out when the markdown processor does its job. There is no need to format nicely because it shouldn't be seen. Thanks SO - http://stackoverflow.com/questions/4823468/store-comments-in-markdown-syntax)


   [XSLT]: <https://developer.mozilla.org/en-US/docs/Web/XSLT>
   [SOAP]: <https://www.service-architecture.com/articles/web-services/soap.html>
   [SOAMANAGER]: <https://help.sap.com/doc/saphelp_nw73ehp1/7.31.19/en-US/b0/787748cf3a4200bb1ba32a62aa8519/frameset.htm>
   [df1]: <http://daringfireball.net/projects/markdown/>


