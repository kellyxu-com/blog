---
layout: post
title: Apache Jena - A free and open source Java framework for building Semantic Web and Linked Data applications.
author: kelly
tags: [RDF]
date: 2022-02-02 01:33:33 +0800
published: true
excerpt_separator: <!--more-->
---


<!--more-->

# Prepare RDF data; Save as triple.xml
```xml
<?xml version="1.0"?>
<rdf:RDF xmlns:rdf="http://www.w3.org/1999/02/22-rdf-syntax-ns#" xmlns:cd="http://www.recshop.fake/cd#">
<rdf:Description rdf:about="http://www.recshop.fake/cd/Empire Burlesque">
  <cd:artist>Bob Dylan</cd:artist>
  <cd:country>USA</cd:country>
  <cd:company>Columbia</cd:company>
  <cd:price>10.90</cd:price>
  <cd:year>1985</cd:year>
</rdf:Description>
<rdf:Description rdf:about="http://www.recshop.fake/cd/Hide your heart">
  <cd:artist>Bonnie Tyler</cd:artist>
  <cd:country>UK</cd:country>
  <cd:company>CBS Records</cd:company>
  <cd:price>9.90</cd:price>
  <cd:year>1988</cd:year>
</rdf:Description>
</rdf:RDF>
```

# Apache jena
Jena provides: a JAVA programming API; engine for RDFS; TDB; command-line tools; Fuseki(combined with UI for admin and query)
After unzipping the file, we can get several JAVA examples.

## Install
```bash
[root@localhost Downloads]# wget https://dlcdn.apache.org/jena/binaries/apache-jena-4.3.2.tar.gz

[root@localhost Downloads]# tar zxvf apache-jena*.gz -C /usr/local/jena/

# Add PATH：export PATH=${JAVA_HOME}/bin:/usr/local/jena/apache-jena-4.3.2/bin:$PATH
[root@localhost Downloads]# vim /etc/profile

[root@localhost Downloads]# source /etc/profile
```
## Run
```bash
[root@localhost jena]# tdb1.xloader --loc /home/kelly/jena/ds triple.xml

# All TDB1 datasets are stored in home/kelly/ds
[root@localhost jena]# tdb1.xloader --loc /home/kelly/jena/ds triple.xml
14:40:37 INFO -- TDB1 Bulk Loader Start
14:40:37 INFO Data Load Phase
14:40:37 INFO Got 1 data files to load
14:40:37 INFO Data file 1: /home/kelly/jena/triple.xml
14:40:39 INFO  loader          :: Total: 10 tuples : 0.38 seconds : 26.46 tuples/sec [2022/01/26 14:40:39 CST]
14:40:39 INFO Data Load Phase Completed
14:40:40 INFO Index Building Phase
14:40:40 INFO Creating Index SPO
14:40:40 INFO Sort SPO
14:40:40 INFO Sort SPO Completed
14:40:40 INFO Build SPO
14:40:40 INFO Build SPO Completed
14:40:40 INFO Creating Index POS
14:40:41 INFO Sort POS
14:40:41 INFO Sort POS Completed
14:40:41 INFO Build POS
14:40:41 INFO Build POS Completed
14:40:41 INFO Creating Index OSP
14:40:41 INFO Sort OSP
14:40:41 INFO Sort OSP Completed
14:40:41 INFO Build OSP
14:40:42 INFO Build OSP Completed
14:40:42 INFO Index Building Phase Completed
14:40:42 INFO -- TDB1 Bulk Loader Finish
14:40:42 INFO -- 5 seconds

# tdb2 functions in the same way
[root@localhost jena]# tdb2.xloader --loc /home/kelly/jena/ds triple.xml
Command jq not found
14:43:16 INFO  Setup:
14:43:16 INFO    Database: /home/kelly/jena/ds
14:43:16 INFO    Data:     triple.xml
14:43:16 INFO    TMPDIR:   /home/kelly/jena/ds
14:43:16 INFO
14:43:16 INFO  Load node table
14:43:17 INFO  Nodes           :: Build node table
14:43:17 INFO  Nodes           ::   Database   = /home/kelly/jena/ds
14:43:17 INFO  Nodes           ::   TMPDIR     = /home/kelly/jena/ds
14:43:17 INFO  Nodes           ::   Data files = triple.xml
14:43:19 INFO  Terms           :: Sort finished
14:43:19 INFO  Nodes           :: == Parse: 0.341 seconds : 10 triples/quads 29 TPS
14:43:20 INFO  Terms           :: ==  Index: 0.613 seconds : 17 indexed RDF terms : 28 PerSecond
14:43:20 INFO  Nodes           :: NodeTable - 2.884 seconds - 0h 00m 02s at 3 terms per second
14:43:20 INFO
14:43:20 INFO  Ingest data
14:43:21 INFO  Data            :: Ingest data
14:43:21 INFO  Data            :: Total: 10 tuples : 0.25 seconds : 39.68 tuples/sec [2022/01/26 14:43:21 CST]
14:43:21 INFO  Data            :: Triples = 10 ; Quads = 0
/usr/local/jena/apache-jena-4.3.2/bin/tdb2.xloader: line 391: jq: command not found
```

# Jena fuseki - the Jena SPARQL server

## Download Jena fuseki
```bash
[root@localhost Downloads]# wget https://dlcdn.apache.org/jena/binaries/apache-jena-fuseki-4.3.2.tar.gz

[root@localhost Downloads]# tar zxvf apache-jena-fuseki*.gz -C /usr/local/jena/

# Add PATH：export PATH=${JAVA_HOME}/bin:/usr/local/jena/apache-jena-4.3.2/bin:/usr/local/jena/apache-jena-fuseki-4.3.2:$PATH
[root@localhost Downloads]# vim /etc/profile

[root@localhost Downloads]# source /etc/profile
```

## Run
```bash
[root@localhost jena]# fuseki-server --loc /home/kelly/jena/ds /myds
14:58:57 INFO  Server          :: Running in read-only mode for /myds
14:58:57 INFO  Server          :: Apache Jena Fuseki 4.3.2
14:58:57 INFO  Config          :: FUSEKI_HOME=/usr/local/jena/apache-jena-fuseki-4.3.2
14:58:57 INFO  Config          :: FUSEKI_BASE=/home/kelly/jena/run
14:58:57 INFO  Config          :: Shiro file: file:///home/kelly/jena/run/shiro.ini
14:58:57 INFO  Config          :: Template file: templates/config-tdb-dir
14:58:59 INFO  Server          :: Database: TDB1 dataset: location=/home/kelly/jena/ds
14:58:59 INFO  Server          :: Path = /myds
14:58:59 INFO  Server          :: System
14:58:59 INFO  Server          ::   Memory: 4.0 GiB
14:58:59 INFO  Server          ::   Java:   17.0.2
14:58:59 INFO  Server          ::   OS:     Linux 3.10.0-1160.31.1.el7.x86_64 amd64
14:58:59 INFO  Server          ::   PID:    19217
14:58:59 INFO  Server          :: Started 2022/01/26 14:58:59 CST on port 3030
```
![](/img/2022-01-26_15-36-31.png)  
![](/img/jena-fuseki-01.png)  

# Problems & Troubleshooting
## Problem 1: error exists: <rdf:Description rdf:about="http://www.recshop.fake/cd/Empire Burlesque">. Try replacing the space with "%20".
```bash
[root@localhost jena]# tdb2.xloader --loc /home/kelly/jena/ds triple.xml
Command jq not found
14:28:50 INFO  Setup:
14:28:50 INFO    Database: /home/kelly/jena/ds
14:28:50 INFO    Data:     triple.xml
14:28:50 INFO    TMPDIR:   /home/kelly/jena/ds
14:28:50 INFO
14:28:50 INFO  Load node table
14:28:51 INFO  Nodes           :: Build node table
14:28:51 INFO  Nodes           ::   Database   = /home/kelly/jena/ds
14:28:51 INFO  Nodes           ::   TMPDIR     = /home/kelly/jena/ds
14:28:51 INFO  Nodes           ::   Data files = triple.xml
14:28:53 ERROR riot            :: Bad character in IRI (space): <http://www.recshop.fake/cd/Empire[space]...>
Exception in thread "AsyncParser" org.apache.jena.riot.RiotException: Bad character in IRI (space): <http://www.recshop.fake/cd/Empire[space]...>
        at org.apache.jena.riot.system.ErrorHandlerFactory$ErrorHandlerStd.error(ErrorHandlerFactory.java:156)
        at org.apache.jena.riot.lang.ReaderRIOTRDFXML$HandlerSink.convert(ReaderRIOTRDFXML.java:251)
        at org.apache.jena.riot.lang.ReaderRIOTRDFXML$HandlerSink.convert(ReaderRIOTRDFXML.java:274)
        at org.apache.jena.riot.lang.ReaderRIOTRDFXML$HandlerSink.statement(ReaderRIOTRDFXML.java:224)
        at org.apache.jena.rdfxml.xmlinput.impl.XMLHandler.triple(XMLHandler.java:74)
        at org.apache.jena.rdfxml.xmlinput.impl.ParserSupport.triple(ParserSupport.java:233)
        at org.apache.jena.rdfxml.xmlinput.states.WantDescription.aPredAndObj(WantDescription.java:109)
        at org.apache.jena.rdfxml.xmlinput.states.WantPropertyElement.theObject(WantPropertyElement.java:200)
        at org.apache.jena.rdfxml.xmlinput.states.WantLiteralValueOrDescription.endElement(WantLiteralValueOrDescription.java:85)
        at org.apache.jena.rdfxml.xmlinput.impl.XMLHandler.endElement(XMLHandler.java:121)
        at java.xml/com.sun.org.apache.xerces.internal.parsers.AbstractSAXParser.endElement(AbstractSAXParser.java:618)
        at java.xml/com.sun.org.apache.xerces.internal.impl.XMLDocumentFragmentScannerImpl.scanEndElement(XMLDocumentFragmentScannerImpl.java:1728)
        at java.xml/com.sun.org.apache.xerces.internal.impl.XMLDocumentFragmentScannerImpl$FragmentContentDriver.next(XMLDocumentFragmentScannerImpl.java:2899)
        at java.xml/com.sun.org.apache.xerces.internal.impl.XMLDocumentScannerImpl.next(XMLDocumentScannerImpl.java:605)
        at java.xml/com.sun.org.apache.xerces.internal.impl.XMLNSDocumentScannerImpl.next(XMLNSDocumentScannerImpl.java:112)
        at java.xml/com.sun.org.apache.xerces.internal.impl.XMLDocumentFragmentScannerImpl.scanDocument(XMLDocumentFragmentScannerImpl.java:542)
        at java.xml/com.sun.org.apache.xerces.internal.parsers.XML11Configuration.parse(XML11Configuration.java:889)
        at java.xml/com.sun.org.apache.xerces.internal.parsers.XML11Configuration.parse(XML11Configuration.java:825)
        at java.xml/com.sun.org.apache.xerces.internal.parsers.XMLParser.parse(XMLParser.java:141)
        at java.xml/com.sun.org.apache.xerces.internal.parsers.AbstractSAXParser.parse(AbstractSAXParser.java:1224)
        at java.xml/com.sun.org.apache.xerces.internal.jaxp.SAXParserImpl$JAXPSAXParser.parse(SAXParserImpl.java:637)
        at org.apache.jena.rdfxml.xmlinput.impl.RDFXMLParser.parse(RDFXMLParser.java:95)
        at org.apache.jena.rdfxml.xmlinput.ARP.load(ARP.java:118)
        at org.apache.jena.riot.lang.ReaderRIOTRDFXML.parse(ReaderRIOTRDFXML.java:186)
        at org.apache.jena.riot.lang.ReaderRIOTRDFXML.read(ReaderRIOTRDFXML.java:84)
        at org.apache.jena.riot.RDFParser.read(RDFParser.java:366)
        at org.apache.jena.riot.RDFParser.parseURI(RDFParser.java:335)
        at org.apache.jena.riot.RDFParser.parse(RDFParser.java:310)
        at org.apache.jena.riot.RDFParserBuilder.parse(RDFParserBuilder.java:552)
        at org.apache.jena.tdb2.xloader.ProcNodeTableBuilderX.lambda$exec$0(ProcNodeTableBuilderX.java:156)
        at java.base/java.util.ArrayList.forEach(ArrayList.java:1511)
        at org.apache.jena.tdb2.xloader.ProcNodeTableBuilderX.lambda$exec$1(ProcNodeTableBuilderX.java:152)
        at java.base/java.lang.Thread.run(Thread.java:833)
```
## Problem 2：Failed to access <http://192.168.253.129:3030/myds> (192.168.253.129 is the service address for Jena).
```bash
# Check the firewalld status. Disable Firewalld.
[root@localhost Downloads]# systemctl status firewalld
● firewalld.service - firewalld - dynamic firewall daemon
   Loaded: loaded (/usr/lib/systemd/system/firewalld.service; enabled; vendor preset: enabled)
   Active: active (running) since Thu 2022-01-27 09:44:27 CST; 35s ago
     Docs: man:firewalld(1)
 Main PID: 856 (firewalld)
    Tasks: 2
   CGroup: /system.slice/firewalld.service
           └─856 /usr/bin/python2 -Es /usr/sbin/firewalld --nofork --nopid

Jan 27 09:44:26 localhost.localdomain systemd[1]: Starting firewalld - dynamic firewall daemon...
Jan 27 09:44:27 localhost.localdomain systemd[1]: Started firewalld - dynamic firewall daemon.
Jan 27 09:44:27 localhost.localdomain firewalld[856]: WARNING: AllowZoneDrifting is enabled. This is co...ow.
Hint: Some lines were ellipsized, use -l to show in full.

# Disable Firewalld
[root@localhost Downloads]# systemctl stop firewalld
```

# Reference
- [Apache Jena](https://jena.apache.org/index.html)
- [Jena + fuseki 安装配置教程](https://blog.csdn.net/vincent_duan/article/details/100879595)
- [An Introduction to RDF and the Jena RDF API](https://jena.apache.org/tutorials/rdf_api.html)
- [vCard Ontology - for describing People and Organizations](https://www.w3.org/TR/vcard-rdf/)
