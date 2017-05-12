# Introducing Solon: Modelling, Managing and Mining Legal Sources

This page is a companion for the paper on [Introducing Solon: Modelling, Managing and Mining Legal Sources], written by Koniaris Marios (me), George Papastefanatos, Marios Meimarids and Giorgos Alexiou. This page hosts additional info, as to encourage progress on legal document management systems and  to improve access to legal sources.

## Intro
Solon main goal is to assist users locate and retrieve legal and regulatory documents within the exact context of a conceptual reference. It consists of several different components, exposed as REST services. We consider Solon as an advanced legal document management platform, operating on legal sources that are automatically discovered and collected from portals by means of Web Crawlers/Harvesters on a scheduled basis, automatically extracting a machine readable semantic representation of legislation, interlinking them based on discovered references and classifying them  according to a set of rules, offering, among others fine-grained search results and  enabling users to organize legal information according to individual needs.

## Solon in Depth

### General Characteristics

* Support for automatic and manual import of unstructured documents from predefined legal sources 
* Automatic structural analysis and semantic representation of textual data and metadata
* Automatic discovery and resolution of legal citations, following standards, for each respective structural unit.
* Automatic classification of legal sources based on custom rules.
* Support for manual curation of the automatically discovered, structured and semantically enriched content.
* Support for multi criteria and multi faceted search using all metadata identified in documents
* Support for structured content retrieval.

Solon is based on a set of software components integrated through well-defined APIs, that communicate through REST HTTP interfaces and can be utilized not only as parts of the overall architecture, but also as individual services.

### Data Model
Our data model models follows  [Akoma Ntoso](http://www.akomantoso.org/), 
a popular legal meta-schema, appropriately extended to accommodate Greek legal documents i.e., laws, presidential decrees, regulatory acts of the Council of minister and administrative acts.

### Architecture

![High-level view of Solon logical architecture](https://raw.githubusercontent.com/mkoniari/Solon/master/figures/arc.png "High-level view of Solon logical architecture")
Solon is composed of different components exposed to the remaining platform as REST services:
* The Document Repository provides functionality for storing and managing complex legal sources. 
* The Crawler module harvests remote information sources as input data 
* The Text Mining module transforms input data to a semantically rich data structure. 
* The Search module is responsible for the efficient indexing and retrieval of legal information.
* The Collaborative Semantic Interlinking module allows users to connect, organize and explore legal resources according to individual needs. 
* The Administration module provides support for the manual curation of content and general administrative functionality.


#### Legal Document Repository

#### Crawler/ harvester

#### Text Mining
##### Structuring Legal Documents
....
##### Interlinking legal sources
....
##### Document Classification
....

#### Collaborative semantic interlinking

#### Search

## Samples

## Acknowledgements
Within this work we utilize 
* [Akoma Ntoso](http://www.akomantoso.org/) a popular XML schemas for representing legal documents
* [akomantoso-lib](http://kohsah.github.io/akomantoso-lib/) a Java API for creating and editing Akoma Ntoso XML documents
* [ANTLR](http://www.antlr.org/) ANother Tool for Language Recognition is a powerful parser generator widely used to build languages, tools, and frameworks. From a grammar, ANTLR generates a parser that can build and walk parse trees.

Finally our architecture has been deployed in a real-world web platform [e-Lib](http://www.publicrevenue.gr/elib/), aiming to provide semantic access to Greek tax legislation, under the supervision of the [General Secretariat of Public Revenue](www.publicrevenue.gr).
