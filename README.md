# Beyond POS PO Package

[![N|Solid](https://www.beyondtechnologies.com/wp-content/themes/beyond/images/beyond.png)](https://www.beyondtechnologies.com/)

This package contains everything in order to setup GK POS integration.

  - GK TLog mapping with [XSLT]
  - Documentation for Master Data replication with [IDocs]
  - Documentation to setup [SOAP] Web Services used by GK

### GK TLog mapping with XSLT

GK TLog mapping is done with XSLT. There are 2 files imported in mapping 
- GKPOSLog2POSDM_PQ5 : Main Logic
- GKPOSLog2POSDMValueMapping_PQ5 : Value Mappings

Make sure that proper version of GK's TLog version is used in the main XSLT stylesheet file. Here's an example of using version 2.2:

```xml
<xsl:stylesheet xmlns:xsl="http://www.w3.org/1999/XSL/Transform" xmlns:SES="http://www.gk-software.com/storeweaver/sdc/pos_upload/ses_pos_upload/2.2" xmlns:datatype="http://www.gk-software.com/storeweaver/sdc/pos_upload/ixr_datatypes/2.2" xmlns:ns0="http://www.gk-software.com/storeweaver/sdc/pos_upload/pos_upload/2.2" xmlns:xs="http://www.w3.org/2001/XMLSchema" version="2.0" extension-element-prefixes="ns0 SES">
```

Content of XSLT mapping should be reviewed with functional team - there are very high chances that content will need to be updated depending on business needs.

RFC used on CAR's side is a standard function module : 
```abap
/POSDW/CREATE_TRANSACTIONS_EXT 
```

If you ever need to do some logic on CAR's side, the RFC can be copied into a custom one. Changes will need to be done in PO but also in the XSLT. 
```xml
<ns1:_-POSDW_-CREATE_TRANSACTIONS_EXT xmlns:ns1="urn:sap-com:document:sap:rfc:functions">
```
Target RFC would need to be changed for the custom one you created, example : 
```xml
<ns1:_ZCREATE_TRANSACTIONS_EXT xmlns:ns1="urn:sap-com:document:sap:rfc:functions">
```


### Documentation for Master Data replication

Master Data replication to GK can be done two ways, depending on the complexity of data sent. 
 - Standard IDocs
 - GK's internal structure

For most projects, Standard IDocs will be used. Complexity needs to be evaluated prior to evaluation and setting those up, as using GK's internal structure requires mapping and takes a lot more time to setup.

All IDocs are going to the same url on GK's side @ **http:port**/swee-ucon/tenants/100/sappi/import/

| IDoc | Direction | Description | 
| ------ | ------ | ------ |
| CREMAS.CREMAS05 | Outbound | Vendor master data distribution |
| DEBMAS.DEBMAS07 | Outbound | Customer master data distribution |
| WBBDLD.WBBDLD05 | Outbound | HPR Assortment List: Material Data with Article Hierarchy |
| WPDWGR.WPDWGR01 | Outbound | POS interface: Download material group master |
| STATUS.SYSTAT01 | Inbound | CA-EDI: Transfer from status records |

Status IDoc is used by GK to send confirmation when receiving Inbound IDocs. It needs to be setup in WE20 on S/4 side with these values



### Documentation to setup SOAP Web Services used by GK

On the S/4HANA side, [SOAP] Web Services need to be activated. This is done through transaction [SOAMANAGER].
SOAP Runtime needs to be properly configured prio to testing those services. Transaction SRT_UTIL can be used to check on the configuration. 
If this isn't done, Basis resources needs to do the setup.

In SOAMANAGER, **Simplified Web Service Configuration** will be used for creating endpoints. A user for GK needs to be created in S/4 in order to be able to call the web services.

Here is a non-exhausting list of potential web services used by GK - confirm with functional team which services are used.


| External Name| Internal Name | Endpoint (HTTP/HTTPS) |
| ------ | ------ | ------ |
| InventoryByLocationAndMaterialQueryResponse_In | 	ECC_INVENTORY002QR | /sap/bc/srt/scs/sap/ecc_inventory002qr |
| SalesOrderERPBasicDataByElementsQueryResponse_In | ECC_SALESORDER009QR | /sap/bc/srt/scs/sap/ecc_salesorder009qr |
| SalesOrderERPByIDQueryResponse_In_V3 | ECC_SALESORDERERPIDQR3 | /sap/bc/srt/scs/sap/ecc_salesordererpidqr3 |
| SalesOrderERPCreateCheckQueryResponse_In | ECC_SALESORDERERPCRTCHKQR | /sap/bc/srt/scs/sap/ecc_salesordererpcrtchkqr |
| SalesOrderERPCreateRequestConfirmation_In_V2 | ECC_SALESORDERERPCRTRC2 | /sap/bc/srt/scs/sap/ecc_salesordererpcrtrc2 |
| TradeReceivablesPayablesAccountERPSplitItemGroupByCustomerIDQueryResponse_In | ECC_TRPACC_SP_ITEM_GRP_BYCUSQR | /sap/bc/srt/scs/sap/ecc_trpacc_sp_item_grp_bycusqr |
| SalesOrderERPChangeRequestConfirmation_In | ECC_SALESORDERERPCHGRC | /sap/bc/srt/scs/sap/ecc_salesordererpchgrc |
| SRS:RFC for billing doc detail read | ZSSB_RFC_BILL_DOC_READ | /sap/bc/srt/scs/sap/zssb_rfc_bill_doc_read |
| SRS:RFC for sales order detail read service | ZSSO_RFC_SALES_DOC_READ | /sap/bc/srt/scs/sap/zsso_rfc_sales_doc_read | 

The two last services starting with ZSS* needs to be created from standard function modules. (WSSB_RFC_BILL_DOC_READ and WSSO_RFC_SALES_DOC_READ respectively)
Unfortunately, those do not work in S/4HANA anymore as they are created in a different namespace than what GK is calling them with. At the time of writing this documentation, a test to call these web services through PO and changing namespaces hasn't been done.

### Todos

 - [ ] Extract and describe all content of XSLT

License
----

SAP
Beyond Technologies


[//]: # (These are reference links used in the body of this note and get stripped out when the markdown processor does its job. There is no need to format nicely because it shouldn't be seen. Thanks SO - http://stackoverflow.com/questions/4823468/store-comments-in-markdown-syntax)


   [XSLT]: <https://developer.mozilla.org/en-US/docs/Web/XSLT>
   [SOAP]: <https://www.service-architecture.com/articles/web-services/soap.html>
   [SOAMANAGER]: <https://help.sap.com/doc/saphelp_nw73ehp1/7.31.19/en-US/b0/787748cf3a4200bb1ba32a62aa8519/frameset.htm>
   [IDocs]: <http://saptechnical.com/Tutorials/ALE/Guide/Index.htm>
   [df1]: <http://daringfireball.net/projects/markdown/>


