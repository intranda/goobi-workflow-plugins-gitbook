---
description: >-
  Administration plugins for migrating from one Goobi workflow system to another Goobi workflow system
---

# Goobi-to-Goobi

## Overview

Name                     | Wert
-------------------------|-----------
Identifier               | intranda_administration_goobi2goobi_export intranda_administration_goobi2goobi_import_infrastructure intranda_administration_goobi2goobi_import_data
Repository               | [https://github.com/intranda/goobi-plugin-administration-goobi2goobi-import](https://github.com/intranda/goobi-plugin-administration-goobi2goobi-import)
Licence              | GPL 2.0 or newer 
Last change    | 25.07.2024 11:11:13


## Introduction
The two plugins described here can be used to transfer data from one Goobi workflow system to another Goobi workflow system (`Goobi-to-Goobi`). This documentation explains how to install, configure and use the associated plugins.


## Installation and configuration
Before the export and import mechanism can be used, various installation and configuration steps must be completed. These are described in detail here:

[Installation](goobi-plugin-administration-goobi2goobi-import_page_01_00_en.md)


## Mode of operation
The mechanism for transferring data from one Goobi workflow system to another Goobi workflow system (`Goobi-to-Goobi`) is divided into three major steps.

![How Goobi-to-Goobi data exchange works](images/goobi-plugin-administration-goobi2goobi-import_screen_description_en.png)

These three steps are as follows:


### a) Creation of the export directories
The first step involves enriching the data within the file system on the source system with the information that Goobi has stored internally in the database for each process. When this step is performed, an additional xml file containing the database information on the workflow and some other necessary data is written to the folder for each Goobi process.

[Erzeugung der Export-Verzeichnisse](goobi-plugin-administration-goobi2goobi-import_page_02_00_en.md)


### b) Transfer of the export directories
After the complete creation and enrichment of the export directories on the source system, they can be transferred to the server of the target system. This can be done in different ways. Due to the amount of data involved, a transfer using `rsync` has proven to be the most suitable.

[Transfer der Export-Verzeichnisse](goobi-plugin-administration-goobi2goobi-import_page_03_00_en.md)


### c) Importing the export directories
After the export directories have been successfully transferred to the target system, the data can be imported there. To do this, the data must be stored in the correct place in the system and some further precautions regarding the infrastructure must also be prepared.

[Einspielen der Export-Verzeichnisse](goobi-plugin-administration-goobi2goobi-import_page_04_00_en.md)