---
title: 'LitReach: A Shiny App for Assessing the Global Reach and Usage of Published Work'
tags:
  - Shiny
  - Global reach
  - Literature
authors:
  - name: Ben W Rowland
    orcid: 0000-0001-8810-7870
    affiliation: 1 
  - name: Theodora Commandeur
    affiliation: 1
  - name: Aileen Mill
    affiliation: 1
affiliations:
 - name: Newcastle University
   index: 1
citation_author: Rowland et. al.
date: "`r Sys.Date()`"
year: 2025
bibliography: paper.bib
output: rticles::joss_article
journal: JOSS
includes:
  - \usepackage{float}
---

```{r setup, include=FALSE}
knitr::opts_chunk$set(echo = FALSE)
knitr::opts_chunk$set(message = FALSE)
knitr::opts_chunk$set(warning = FALSE)
knitr::opts_chunk$set(fig.pos = "H")

```

## Summary

LitReach is a Shiny app developed in R [@rcoreteamLanguageEnvironmentStatistical2018] that allows authors to assess the global reach and usage of their published work.

The app provides a range of visualisations that allow authors to explore the scope of their work in a variety of ways, from citations in types of literature to global reach. The app also includes a data tidying tool that formats their citation data for use by the app. We provide exemplar data from our assessment of the global reach and usage of one of the outputs from the second study of infectious intestinal disease (IID2) [@obrienMethodsDeterminingDisease2010] in the UK [@tamLongitudinalStudyInfectious2012; @tamSecondStudyInfectious2012]. 

One problem often faced by researchers is that citation metrics are easy to obtain, but knowing the range of citations and global reach of your work requires more in-depth analysis and a good knowledge of R (or another language) and the use of a variety of packages. LitReach aims to provide a platform for authors to upload data from sources such as Scopus, Web of Science, PubMed and Google Scholar that can then be summarised and viewed in an easy-to-understand and interactive way. Other platforms like Connected Papers (https://www.connectedpapers.com/) can provide some information about citations and papers on a similar topic but do not extend into global reach. LitReach aims to streamline this process for authors by providing a simple-to-use, all-in-one interface that can be used by individuals with little to no experience in R or other programming languages.

## Statement of Need

Authors of published scientific literature want to track citations and the uptake of information and are often required to demonstrate it for their work. This can be challenging, as the uptake of information is frequently measured in multiple different ways, including citations, downloads, social media engagement, effect on policy and global reach and across multiple platforms. The need for LitReach in the form of a readily accessible shiny application is:

1. To provide researchers at academic institutions with an easy method for determining the global reach and usage of published works and provide them with clear and comprehensible visuals.
2. To help support further funding bids that relate to or are follow-on works from the original publication.
3. To provide a tool that can be used by researchers to demonstrate their research usage to a broader audience.
4. To facilitate the further development of methods to measure and visualise the scale of the reach of scientific literature both in the scientific community and in policy and public engagement.

## Data Sources and Structure

LitReach uses data from Scopus, Web of Science, PubMed, and Google Scholar (through the Publish or Perish software [@harzingPublishPerish2007]), and the data are uploaded by the user in the form of a CSV files. The user can upload raw CSV files from those sources to the app's inbuilt data tidying tool, which will concatenate the search results into one usable CSV file, the process of which is described in Figure 1. After data tidying, the CSV file involves some manual input of the data as not all sources export the same information when exporting citation information, particularly the type of literature and country of affiliation if using data from Google Scholar.    

```{r fig.cap="The process of citation data preparation and tidying"}
library(magick)
library(ggplot2)
library(cowplot)

ggdraw() +
  draw_image("images/Data Processing.png") 

```

The app requires data to be in the following format (Table 1) which is also provided in the app as an example of the data format needed:

```{r}
library(tidyverse)
library(here)
library(flextable)

data <- read_csv(here::here("example data", "Citation Data.csv")) %>% 
  select(ref_id:year, country) %>% 
  mutate(year = as.character(year))

head(data, n = 5) %>%
  as.data.frame() %>% 
  flextable() %>% 
  width(j  = 1:6, width = c(1, 1, 2, 4, 2, 2), unit = "cm") %>% 
  set_caption("An example of the data format needed by the app.")

```

Where ref_id is the unique identifier, author is the last name of the first author, title is the title of the publication, year is the year of the publication, and country is the country the first author is affiliated with. 

The data also includes columns that identify the citation as one of your papers of interest in the event you are looking at multiple outputs, the number of citations, policy mentions, mentions in other places, views and pdf downloads of the piece of literature. 

## Application Structure

The application is divided up into five tabs:  

1. The 'Instructions and Data Upload' tab, where the user can upload raw data and have it prepared for the app, and where the user uploads the data for the app to display.  
2. The 'Research Outputs' tab displays the number of citations, policy mentions, mentions in other places, views and pdf downloads of the literature.
3. The 'Uses in Literature' tab where the app displays information on how the piece of work has been cited in the literature.  
4. The 'Global Reach' tab is where the app displays information on the number of citations from other countries the literature has received.  
5. A 'FAQ' (frequently asked questions) tab that provides information on app development. 
  
## Limitations

The data need by LitReach is limited by the data that can be extracted from literature sources. One limitation of the app is that Google Scholar does not offer a mass download of citation data, so the user must use a different software. The data that is downloaded this way also does not include the first country of affiliation nor the type of literature the reference is (article, thesis, book etc.) so this must be added manually. The app however does not require data from all sources, so if the user only has data from a selection of sources the app will still work.

## Ongoing Research

LitReach was developed in part to help assess the impact of the ongoing Third Study of Infectious Intestinal Disease [(IID3)](https://www.food.gov.uk/research/foodborne-pathogens/the-third-study-of-infectious-intestinal-disease-in-the-uk-iid3) in the UK. It was used to support the bid for further funding of the IID3 study. 

## Link to Software Archive

The LitReach repository is available [here](https://github.com/BRowland-git/LitReach)

## Acknowledgements

We acknowledge the Newcastle University Policy team for funding the work that lead to the idea of LitReach. We want to thank Jessica Ward and Emily Stevenson for their help in beta testing the package during development. 

## References
