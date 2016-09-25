# ICASA OWL Ontology

This is an initial rendering of the [ICASA Master Variable List](https://docs.google.com/spreadsheets/d/1MYx1ukUsCAM1pcixbVQSu49NU-LfXg-Dtt-ncLBzGAM/pub?output=html#) "Management_Info" sheet as an OWL ontology.

The ontology can also be viewed as HTML using the Live Owl Documentation Environment (LODE):
[http://www.essepuntato.it/lode/https://raw.githubusercontent.com/craig-willis/icasa/master/icasa.owl](http://www.essepuntato.it/lode/https://raw.githubusercontent.com/craig-willis/icasa/master/icasa.owl)

See the [Design Notes](docs/design.md) for more information on the basic requirements, recommendations, and design considerations.

## Conversion
Files:
* icasa.csv: Management_Info sheet downloaded as CSV
* icasa.owl: OWL ontology (output of icasa.py)
* icasa.py: Python script that reads icasa.csv, subset-map.csv and generates icasa.owl
* subset-map.csv: Manual mapping of dataset/subset/group information to RDF Class Names. Descriptions were taken from White et al (2013).

To run:
```
python icasa.py > icasa.owl
```

Each dataset/subset/group is added as an RDF Class. Each variable/code is added as a datatype property with domain as the associated class (dataset/subset/group) and range xsd:string. For example:

```
<!-- http://purl.org/icasa#Experiment -->
<owl:Class rdf:about="http://purl.org/icasa#Experiment">
   <rdfs:label>Experiment</rdfs:label> 
   <rdfs:comment xml:lang="en">Complete description of management and initial conditions for a real or synthetic experiment (or very closely linked set of experiments). Data measured during or at the end of the experiment. The information presented should be sufficient to allow thorough interpretation or analysis of the results and for simulation of the experiment</rdfs:comment>
   <rdfs:subClassOf rdf:resource="http://www.w3.org/2002/07/owl#Thing"/>
</owl:Class>

<!-- http://http://purl.org/icasa#data_source -->
<owl:DatatypeProperty rdf:about="http://purl.org/icasa#data_source">
    <rdfs:label>data_source</rdfs:label>
    <rdfs:comment xml:lang="en">Original format of  data (DSSAT, APSIM, CIMMYT, field log, etc)</rdfs:comment>     
    <rdfs:domain rdf:resource="http://purl.org/icasa#Experiment"/>
    <rdfs:range rdf:resource="http://www.w3.org/2001/XMLSchema#string"/>
    <rdfs:subPropertyOf rdf:resource="http://www.w3.org/2002/07/owl#topDataProperty"/>
</owl:DatatypeProperty>
```

# Notes
* Assumes PURL created at purl.org/icasa (login currently disabled on purl.org site)
* Object properties have not yet been added (relations.csv)
* Some classes are duplicated (Person/Institution/Document) for experiment, soil, weather station, etc.  These can likely be consolidated to a single class.
* Some of the terms in the original vocabulary are ID fields and relational keys intended for use in an RDBMS.
* Some classes and variables are not included in the V 2.0 documentation (Suite, AgMIP variables, Dome simulation)
* Some codes sometimes contain % or #
* Some codes in the AgMIP JSON Objects documentation do not exist in the spreadsheet (people, tr_name, icrzno, icbl, elev)
* AgMIP JSON Objects examples sometimes use variable name instead of code (crop_model_version versus model_ver)

# TODO
* Add object properties (relations)
* Add support for measured data
* Demonstrate use with AgMIP JSON Objects and JSON-LD
