# Towards Automatic Structuring and Semantic Indexing of Legal Documents

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

### Extractor
#### Text Extract
Text is acquired either form pdf files or from an ocr program.
#### Image Extract
We obtain images directly from the pdf.

### Parser
Our parser implementation can handle the following types of documents

| TYPE  | URI | STRUCTURAL ANALYSIS|
| ------------- | ------------- | ------------- |
| Law| act | YES |
| Presidential  Decree| pd |YES |
| Regulatory Act  |ap | YES/NO |
| Information circulars| egk| NO |
| Other Legal Docs| other | NO |



The Structural Analysis refers to whether the hierarchical structure of a document is considered by the parser e.g., the contents analyzed at structural hierarchical levels (article, paragraph) or not.

#### Law
Laws strictly follow the structure offered by the [National Printing Service](http://www.et.gr). Because of the strict hierarchical structuring of laws, files not conforming to the pdf layout of the National Printing Service are currently not supported.

### Presidential Decree
Presidential Decrees must follow the structure offered by the [National Printing Service](http://www.et.gr). Files not conforming to the pdf layout of the National Printing Service are currently not supported. Since a Presidential Decree may actually contain several others, a single pdf will lead to the creation of multiple legal documents. The parser detects from the Table of Contents and both the number and the type, i.e. Presidential Decree or Ministerial Decision, of the decisions contained within. 

### Regulatory Acts   
Regulatory Acts may follow:
 - [National Printing Service](http://www.et.gr) structure, as noted above
 - General form: In this case due to the lack of proper guidelines, the text is not hierarchically structured, only metadata are identified.

### Information circulars - Other Legal Docs
In this case due to the lack of proper guidelines, the text is not hierarchically structured, only metadata are identified.

## Categorizer

### Semantic analysis

The semantic analysis includes, among others, the detection of: 

* Issuing Authority: Specially customized rules expressed as regular expressions that model the hierarchical organization of the issuing agency.

* Signer: Supported levels are:
  1. PRESIDENT OF THE GREEK REPUBLIC
  2. PRIME MINISTER
  3. VICE PRESIDENT OF GOVERNMENT
  4. MEMBERS OF THE CABINET OF MINISTERS
  5. MINISTERS
  6. DEPUTY MINISTER
  7. GENERAL SECRETARY
  8. SPECIAL SECRETARY
  9. OTHER

* Categorization/ classification:
Document classification works with customized rules closely related to the issuing authority.

* Keywords:
The most frequent words, except for stop words.

### Linkage analysis
In alignment with the EU proposed standard for a European Legislation Identifier (ELI) that provides, among others, a solution to uniquely identify and access national and European legislation online, our approach offers the minimum set of metadata required by the ELI standard and assigns a URI at each different legal block modeled in Akoma Ntoso. For example the '.../gr/act/2008/3643/main/art/1/...' URI identifies article 1 of the main part of the act with no 3643, published in 2008 by the Greek Parliament. In this way the mark-up of each structural unit of the document complies with the ELI standard, as to facilitate the precise linkage of legal citations for each respective structural unit.

## DSL Language for legal documents

Domain-specific modeling is a software engineering methodology for designing and developing systems directly from
the domain-specific models, offering tailor-made solutions to problems in a particular domain. For the identification of the syntax rules, we heavily rely on domain knowledge from the legal experts who provide with feedback on the structural parts and their relationships (nesting, succession, etc.) within the legal documents.  

A visual aid of the context-free grammar (CFG), described in Extended Backus-Naur Form,  is given in the following figure, where nonterminals such as body and conclusions are defined separately:
![greek legal documents EBNF](https://raw.githubusercontent.com/mkoniari/LawParser/master/figures/fig2.png "greek legal documents EBNF")

## Parsing Process

### Document Structure Parser
Based on the defined GFG and the set of syntax rules defined, we employ [ANTLR](http://www.antlr.org/) parser generator as to implement the lexer, parser, and tree walker. 
An overview of the document structure parser, is given in the following figure:
![Overview of Document Structure Parser](https://raw.githubusercontent.com/mkoniari/LawParser/master/figures/fig3.png "Overview of Document Structure Parser")

### Parsing Strategy
The main steps of our approach are:
* identify the structure of the legal documents
* identify legal documents metadata
* validate produced files against the selected schema

We follow a pipeline strategy, utilizing a top-down approach, that can be summarized into the following 5 steps:

1. Document Type Identification. 
2. Structural Analysis. 
3. Legal Blocks Isolation
4. Legal Modeling
5. Semantic check and validation

## Samples
File [Greek law 4330](https://github.com/mkoniari/LawParser/blob/master/samples/law_4330_2015.pdf) contains Greek law 4330 as published in the official government gazette (volume 59 / year 2015). The resulting Akoma Ntoso file is [xml Greek law 4330](https://github.com/mkoniari/LawParser/blob/master/samples/law_4330_2015.xml)

## Acknowledgements
Within this work we utilize 
* [Akoma Ntoso](http://www.akomantoso.org/) a popular XML schemas for representing legal documents
* [akomantoso-lib](http://kohsah.github.io/akomantoso-lib/) a Java API for creating and editing Akoma Ntoso XML documents
* [ANTLR](http://www.antlr.org/) ANother Tool for Language Recognition is a powerful parser generator widely used to build languages, tools, and frameworks. From a grammar, ANTLR generates a parser that can build and walk parse trees.

We wish to thank the [General Secretariat of Public Revenue](www.publicrevenue.gr) for providing  a corpus of more than 600 legal documents, such as laws, presidential decrees and regulatory acts, in pdf format, on which iterative tests have been carried out, aiming at modeling and refining our method.

Finally our parsing mechanism has been deployed in a real-world web platform [e-Lib](http://www.publicrevenue.gr/elib/), aiming to provide semantic access to Greek tax legislation, under the supervision of the [General Secretariat of Public Revenue](www.publicrevenue.gr).
