# WikiDev HOWTO

This note describes the steps necessary to setup a container based environment for working with a semantic wiki instances. In this not we use NATO Interoperability Standards and Profiles (NISP) as an example.

## Installing a Semantic Wiki container

The Wiki Development environment container exists in a Docker repository at TIDE. In order to install it, you first need to fetch the installation script, using the command 

    wget http://goosje.com/wikidev

Before installing the container, you first need a TIDE account and be authorised to install the container. Subsequently you can install the Wikidevelopment environment, using the command

    sh ./wikidev

Note, that besides the wikidev environment, a small container management tool called [portainer](https://portainer.io/) is also installed.

## Creating a Wiki Instance

The Wiki management environment are able to handle multiple wiki instances. Before you work with a Wiki instance, you need to create it. The Wiki management consol is available via the URI

    http://localhost:8000/admin/

Press the Create button and fill in the *ID* and *Name* fields. For the NISP project potential values for id and name could be **nisp12** and **NISP**.

When the wiki has been created, you can access it

    http://localhost:8000/

## Populating the Wiki

The Wiki instance with application pages and data is done using the Wiki Automation Tool (WAT), which create a bunch of application pages, form, reports etc based on a semantic datamodel, which descripes all concepts and properties of the datamodel.

To setup the a NISP Wiki, you need a development directory, where you create three subdirectories on for the wat tool, one for the NISP domain and a Data directory, where credentials and an the semantic representation of the data model and eventually data, which needs to imported into the wiki.

      --wikidev
           |------ nisp
           |------ wat
           |------ Data

This can be done in the following steps:

1. mkdir wikidev
2. cd wikidev
3. git clone https://tide.act.nato.int/git/tide/nisp
4. git clone https://tide.act.nato.int/git/tide/wat
5. mkdir Data

And finally you should copy the file *LocalSettings.py* to the Data directory.

### Publishing the datamodel

The data model located in *nisp/datamodel.xml* describes the concepts and properties of the NISP datamodel.

It is published in the following way. From the nisp directory run the following command

    ../wat/wat.sh --wiki nisp12

After the first publication, Tomasz recommends that the following command is run a couple of times

    ../wat/wat.sh --wiki nisp12 --clearCache

### Publishing the data

Pages containing data complying with the datamodel should be stored in the folder

    Data/NISP/data/pages/Main/

The stylesheet located in nisp/tools is a XSLT 2 stylesheet, which utilizes the [SAXON XSLT processor](http://saxon.sourceforge.net/) ablity to serialize processing of XML. The stylesheet generate pages for all concepts an properties taken from the NISP XML database. The stylesheet has been tested with Saxon version 9.1.

The pages are generated with the command

    saxon standards.xml tools/makePages.xsl

The generated pages can be uploaded to the wiki instance using the command

    ../wat/wat.sh --wiki nisp12 --deployData
    
## Usefull pages in the wiki

The Wiki comes installed with a number of useful pages, which can be found from the search box.

* Project:Data Model - Describes the conceptual data model for the specific domain
* Project:Data Browser - List the packages and concepts in the datamode with links to the actual data
* Project:Wiki Model - A copy of the data model file
* Project:Software - Lists the application pages generated from the Wiki model
* Help:Developer Manual - Descripes the Wiki development process
* Special:Version - Lists installed software
