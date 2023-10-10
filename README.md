
<!-- README.md is generated from README.Rmd. Please edit that file -->
<!-- badges: start -->

[![Lifecycle:
experimental](https://img.shields.io/badge/lifecycle-experimental-orange.svg)](https://www.tidyverse.org/lifecycle/#experimental)
[![CodeFactor](https://www.codefactor.io/repository/github/roblbaker/emleditor/badge)](https://www.codefactor.io/repository/github/roblbaker/emleditor)
<!-- badges: end -->

# EMLeditor

##### v0.1.2

##### “Mukooda Trail”

<!-- badges: start -->
<!-- badges: end -->

# Overview

The goal of EMLeditor is to edit EML-formatted xml files. Specifically,
EMLeditor provides many functions that will be useful to the U.S.
National Park Service when generating metadata for statistical data
packages uploaded to DataStore. NPS affiliation is assumed as default.
However, some of the functions for viewing and editing metadata may be
useful to people outside the NPS.

# Workflow outline

EMLeditor’s primary objective is to edit and view EML formatted files,
not to generate them from scratch. A suggested workflow is:

1) Use the `EMLassemblyline::make_eml()` to generate an initial EML document.
2) Use EMLeditor functions to edit the metadata in R and evaluate whether your metadata is acceptable (don't forget to use `EML::eml_validate()` to make sure you are generating valid EML).
3) Use the `EML::write_eml()` function to write the R object back to XML (remember the NPS naming convention for metadata files is *_metadata.xml).
4) Check your EML and data package using `DPchecker::run_congruence_checks()`.
5) Upload to DataStore (via web or `EMLeditor::uplad_datapackage()`).
6) Extract metadata on DataStore.
7) Please *DO NOT ACTIVATE* the DataStore reference: prior to activation, data packages need to be reviewed via a yet-to-be-created process.

If you use EMLeditor functions to alter your metadata (e.g. “set” class
functions) they will also silently add the National Park Service as a
publisher (including location, [ROR id](https://ror.org/), etc) to your
metadata unless you set NPS=FALSE. If you leave the default setting as
NPS=TRUE, EMLeditor will also assume the data package is being created
“by or for the NPS” and add that information to the metadata.

EMLeditor will also add information about the version of EMLeditor you
used to edit your metadata (for instance if you used “set” class
functions).

# Installation

You can install the development version of EMLeditor from
[GitHub](https://github.com/) with:

``` r
# install.packages("devtools")
devtools::install_github("nationalparkservice/EMLeditor")
```

To install all the packages in the
[NPSdataverse](https://github.com/nationalparkservice/NPSdataverse)
(including EMLeditor):

``` r
devtools::install_github("nationalparkservice/NPSdataverse")
```

It is recommended to always load EMLeditor via 
```r
library(NPSdataverse)
```
loading the entire NPSdataverse library is the preferred method as it will check whether all of your NPSdataverse packages are up to date (including EMLeditor) and provide instructions on how to update them if you are not working with the latest version.

# Details

For a detailed description of how to use EML editor functions and a
guide on which functions are required to complete EML metadata creation
to construct an NPS data package for uploading to DataStore, see the
complete guide.

## Example:

This is a basic workflow for adding a Digital Object Identifier:

``` r
library(EMLeditor)
library(EML)

# load a pre-existing EML-formated xml file:
my_eml <- EML::read_eml("EML_metadata.xml", from = "xml")

# the 7-digit number is your DataStore Reference number.
# It is automatically generated when you initiate a draft Reference.
# Your DOI is reserved but will not be registered/activated until publication.
my_eml2 <- set_doi(my_eml, "1234567")

# make sure your EML is valid.
EML::eml_validate(my_eml2)

# write the new R object back to XML:
write_eml(my_eml2, "EML_metadata.xml")
```
