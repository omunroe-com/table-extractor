# The table extractor - GSoC 2017 project 

## Abstract
 _Wikipedia is full of data hidden in tables. The aim of this project is to explore the possibilities of exploiting all the data represented with the appearance of tables in Wiki pages, in order to populate the different chapters of DBpedia through new data of interest. The Table Extractor has to be the engine of this data “revolution”: it would achieve the final purpose of extracting the semi structured data from all those tables now scattered in most of the Wiki pages._

## Project
Idea's project is to analyze resources chosen by user and to create related RDF triples. First of all you have to run `pyDomainExplorer`, passing right arguments to it. This script will create a settings file (named `domain_settings.py`) that you have to fill: it is commented to help you in this work.
Then run `pyTableExtractor` that read previous filled file and start to map all resources so that you can obtain RDF triples saved in `Extractions` folder.

## Get Started
This section is splitted in three parts:
* Requirements: libraries that you have to install to use this project.
* pyDomainExplorer: instructions step by step to run this module.
* pyTableExtractor: guide to run this script.

### Requirements
First of all you must have libraries that I used to develop code.
You can install requirements using requirements.txt `pip install -r requirements.txt`
* Python 2.7
* [RDFlib library](http://rdflib.readthedocs.io/en/stable/gettingstarted.html "RDFlib homepage") (v. >= 4.2)
* [lxml library](http://lxml.de/lxmlhtml.html "lxml homepage") (v. 3.6 Tested)
* Stable internet connection

### How to run table-extractor

* Clone repository.

* Choose a language ((e.g. `en`, `it`, `fr` ...).

* Choose a domain to analyze.  that could be:
   * Single resource (e.g. `Kobe_Bryant`, `Roberto_Baggio`, ..). Remember to let underscore instead of space in resource name.
   * DBpedia mapping class (e.g. `BasketballPlayer`, `SoccerPlayer`,..), you have a complete list [there](http://mappings.dbpedia.org/server/ontology/classes/).
   * Where clause (e.g. "?film <http://www.w3.org/1999/02/22-rdf-syntax-ns#type> <http://dbpedia.org/ontology/Film>.  ?film <http://dbpedia.org/ontology/director> ?s" is used to collect all film directors of a wiki chapter. Note: please ensure that the set you want to collect is titled as ?s.
   
* Choose a value for output organization, that could be 1 or 2. See below to understand how this value influence `domain_settings.py` file.

* Now you can run pyDomainExplorer.py:

`python pyDomainExplorer.py [--chapter --verbose (--where|--single|--topic)]`

This module will take resources in language defined by user and will analyze each table that are in wikipedia pages. At the end of execution, it creates a file named `domain_settings.py`.

What's function of this file?
`domain_settings.py` contains all sections and headers found in exploration of the domain. You will observe a dictionary structure and some fields that have to be filled. Below there is an example of output.

* Next step is to fill `domain_settings.py`. Remember that you are writing _mapping rules_, so you are making an association between a table's header (or table's section) with a dbpedia ontology property.

* When you have compiled `domain_settings.py`, you can easily run `pyTableExtractor.py` in this way:

`python pyTableExtractor.py`

This script read all parameters in `domain_settings.py` and print a .ttl file that contains RDF triples obtained by domain's analysis.

If it goes well, you will get a dataset in `Extraction` folder!

Read below something more about arguments passed to `pyDomainExplorer`.

### pyDomainExplorer arguments
* `-c`, `--chapter` : Required. 2 letter long string representing the desidered endpoint/Wikipedia language (e.g. `en`, `it`, `fr` ...) Default value: 'en'.
* `-v`, `--verbose` : Required. One number that can be 1 or 2. Each value correspond to a different organization of output file.

#### Verbose
* 1 - Output file will contain new data to map and mapping rules that are in the table extractor's dictionary.
* 2 - Output file will contain new data to map (shown only one time) and mapping rules saved in table extractor's dictionary

### Example of verbose usage

In a domain like basketball player, you can observe these `domain_settings.py` files. The first one refers to verbose 1 while the second one is related to verbose 2. You can use this parameter to simplify your work over all different domains.
```
### VERBOSE VALUE: 1
# Example page where it was found this section: Kobe_Bryant
SECTION_Playoffs = {
'sectionProperty': '', 
'Year': 'Year', 
'Team': 'team', 
'GamesPlayed': '', 
'GamesStarted': ''
} 
# Example page where it was found this section: Kobe_Bryant
SECTION_Regular_season = {
'sectionProperty': '', 
'Year': 'Year', 
'Team': 'team', 
'GamesPlayed': '', 
'GamesStarted': ''
} 

# END OF FILE 
```
```
### VERBOSE VALUE: 2
# Example page where it was found this section: Kobe_Bryant
SECTION_Playoffs = {
'sectionProperty': '', 
'Year': 'Year', 
'Team': 'team', 
'GamesPlayed': '', 
'GamesStarted': ''
} 
# Example page where it was found this section: Kobe_Bryant
SECTION_Regular_season = {
'sectionProperty': '',
} 

# END OF FILE 
```

### Usage examples

* `python pyDomainExplorer.py -c it -v 1 -w "?s a <http://dbpedia.org/ontology/SoccerPlayer>"` ---> chapter = 'it', verbose= '1', tries to collect resources (soccer players) which answer to this sparql query from DBpedia.

* `python pyDomainExplorer.py -c en -v 2 -t BasketballPlayer` ---> chapter='en', verbose='1', topic='BasketballPlayer', collect resources that are in DBpedia ontology class 'BasketballPlayer'.

* `python pyDomainExplorer.py -c it -v 2 -s "Kobe_Bryant"` ---> the script will works only one [wiki page](https://it.wikipedia.org/wiki/Kobe_Bryant "Kobe Bryant") of 'it' chapter. It's important to use the same name of wikipedia page.

Notes:
* If you choose a topic (-t) or you pass to the script a custom where clause, a list of resources (.txt files) are created in /Resource_lists . 
* If everything is ok, three files are created in /Extractions : two log file (one for pyDomainExplorer and one for pyTableExtractor) and a .ttl file containing the serialized rdf data set.

## Results
In this page: [Results page](https://github.com/dbpedia/table-extractor/tree/master/Extractions/GSoC%202017%20Results) you can observe dataset (english and italian) extracted using `table extractor` . Furthermore you can read log file created in order to see all operations made up for creating RDF triples.

I recommend to also see [this page](https://github.com/dbpedia/table-extractor/wiki), that contains idea behind project and an example of result extracted from log files and .ttl dataset.

Note that effectiveness of the mapping operation mostly depends on how many rules user has wrote in `domain_settings.py`.

---

## statistics.py

This script, written in collaboration with Federica Baiocchi (@github/Feddie), is useful to know the number of tables or lists contained in Wikipedia pages from a given topic, and was created in collaboration with Feddie who is working on the [List Extractor](https://github.com/dbpedia/list-extractor). We both used it in the beginning of our projects to choose a domain to start from.
### Get started
#### How to run statistics.py

`python statistics.py language struct_type topic`
* `language` : a two letter long prefix representing the desidered endpoint/Wikipedia language to search (e.g. `en`, `it`, `fr` ...)
* `struct_type` : `t` for tables, `l` for lists
* `topic ` : can be either a where clause of a sparql query specifying the requested features of a ?s subject, or a keyword from the following: _dir_ for all DBpedia directors with a Wikipedia pages,  _act_ for actors, _soccer_ for soccer players,_writer_ for writers

#### Usage examples

*`python statistics.py it t "?s a <http://dbpedia.org/ontology/SoccerPlayer>.?s <http://dbpedia.org/ontology/wikiPageID> ?f"`
*`python statistics.py en l writer`


## Progress pages
 *  [GSoC 2017 progress page](https://github.com/dbpedia/table-extractor/wiki/GSoC-2017:-Luca-Virgili-progress)
 *  [GSoC 2016 progress page](https://github.com/dbpedia/extraction-framework/wiki/GSoC_2016_Progress_Simone "Progress")

For any questions please feel to contact:
* Luca Virgili, lucav48@gmail.com , GSoC 2017 student.
* Simone papalini, papalini.simone.an@gmail.com , GSoC 2016 student.

