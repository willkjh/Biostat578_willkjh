Homework 1: William Koh (willkjh)
========================================================
width: 1440
height: 900
transition: none
font-family: 'Helvetica'
css: my_style.css
author: William Koh
date: January 27,2014

First Slide
========================================================


```r
library(knitr)
library(pander)
Update <- FALSE
if(Update){
  source("http://bioconductor.org/biocLite.R")
  # Install all core packages and update all installed packages
  biocLite()
  biocLite(c("GEOmetadb", "GEOquery"))
}
#opts_chunk$set(cache=TRUE)
```

Caching first

Question 1
========================================================

Use the GEOmetabd package to find all HCV gene expression data using the Illumina platform submitted by an investigator at Yale. This should be done with a single query, showing the title, the GSE accession number, the GPL accession number and the manufacturer and the description of the platform used.



Description of method
========================================================
Here I search from where the data comes from Homo Sapiens and selected summaries table that contain the name HCV. 
In addition, I filter out contact authors who are not from Yale Institute or authors whose correspondence has email which is like yale.edu.

Code + Output 
========================================================


```r
query <- "SELECT DISTINCT  gse.title, gse.gse, gpl.gpl,gpl.manufacturer,gpl.description 
          FROM gse JOIN gse_gpl ON gse.gse=gse_gpl.gse 
          JOIN gpl ON gse_gpl.gpl=gpl.gpl
          WHERE gpl.organism LIKE '%Homo sapiens%' AND gpl.manufacturer LIKE '%Illumina%'AND gse.summary LIKE '%HCV%' AND gse.contact LIKE '%@yale.edu%'"
          
rs <- dbGetQuery(geo_con, query)
```


Output for Question 1
========================================================


```r
pandoc.table(rs, style="grid")
```

```


+--------------------------------+----------+----------+----------------+
|             title              |   gse    |   gpl    |  manufacturer  |
+================================+==========+==========+================+
|   The blood transcriptional    | GSE40223 | GPL10558 | Illumina Inc.  |
|    signature of chronic HCV    |          |          |                |
| [Illumina data]                |          |          |                |
|                                |          |          |                |
+--------------------------------+----------+----------+----------------+
| Impaired TLR3-mediated immune  | GSE40812 | GPL10558 | Illumina Inc.  |
| responses from macrophages of  |          |          |                |
| patients chronically infected  |          |          |                |
|     with Hepatitis C virus     |          |          |                |
+--------------------------------+----------+----------+----------------+

Table: Table continues below

 

+-----------------------------------------------------------------+
|                           description                           |
+=================================================================+
|                  The HumanHT-12 v4 Expression                   |
|                     BeadChip provides high                      |
|                   throughput processing of 12                   |
|                  samples per BeadChip without                   |
|                     the need for expensive,                     |
|                   specialized automation. The                   |
|                     BeadChip is designed to                     |
|                  support flexible usage across                  |
|                       a wide-spectrum of                        |
|                   experiments.; ; The updated                   |
|                  content on the HumanHT-12 v4                   |
|                  Expression BeadChips provides                  |
|                  more biologically meaningful                   |
|                   results through genome-wide                   |
|                   transcriptional coverage of                   |
|                 well-characterized genes, gene                  |
|                     candidates, and splice                      |
|                 variants.; ; Each array on the                  |
|                    HumanHT-12 v4 Expression                     |
|                   BeadChip targets more than                    |
|                   31,000 annotated genes with                   |
|                     more than 47,000 probes                     |
|                    derived from the National                    |
|                    Center for Biotechnology                     |
|                 Information Reference Sequence                  |
|                    (NCBI) RefSeq Release 38                     |
|                  (November 7, 2009) and other                   |
|                 sources.; ; Please use the GEO                  |
|                 Data Submission Report Plug-in                  |
|                 v1.0 for Gene Expression which                  |
|                     may be downloaded from                      |
|       https://icom.illumina.com/icom/software.ilmn?id=234       |
|                  to format the normalized and                   |
|                   raw data.  These should be                    |
|                     submitted as part of a                      |
|                  GEOarchive.  Instructions for                  |
|                 assembling a GEOarchive may be                  |
|                            found at                             |
| http://www.ncbi.nlm.nih.gov/projects/geo/info/spreadsheet.html; |
|                 ; October 11, 2012: annotation                  |
|                       table updated with                        |
|                HumanHT-12_V4_0_R2_15002873_B.txt                |
+-----------------------------------------------------------------+
|                  The HumanHT-12 v4 Expression                   |
|                     BeadChip provides high                      |
|                   throughput processing of 12                   |
|                  samples per BeadChip without                   |
|                     the need for expensive,                     |
|                   specialized automation. The                   |
|                     BeadChip is designed to                     |
|                  support flexible usage across                  |
|                       a wide-spectrum of                        |
|                   experiments.; ; The updated                   |
|                  content on the HumanHT-12 v4                   |
|                  Expression BeadChips provides                  |
|                  more biologically meaningful                   |
|                   results through genome-wide                   |
|                   transcriptional coverage of                   |
|                 well-characterized genes, gene                  |
|                     candidates, and splice                      |
|                 variants.; ; Each array on the                  |
|                    HumanHT-12 v4 Expression                     |
|                   BeadChip targets more than                    |
|                   31,000 annotated genes with                   |
|                     more than 47,000 probes                     |
|                    derived from the National                    |
|                    Center for Biotechnology                     |
|                 Information Reference Sequence                  |
|                    (NCBI) RefSeq Release 38                     |
|                  (November 7, 2009) and other                   |
|                 sources.; ; Please use the GEO                  |
|                 Data Submission Report Plug-in                  |
|                 v1.0 for Gene Expression which                  |
|                     may be downloaded from                      |
|       https://icom.illumina.com/icom/software.ilmn?id=234       |
|                  to format the normalized and                   |
|                   raw data.  These should be                    |
|                     submitted as part of a                      |
|                  GEOarchive.  Instructions for                  |
|                 assembling a GEOarchive may be                  |
|                            found at                             |
| http://www.ncbi.nlm.nih.gov/projects/geo/info/spreadsheet.html; |
|                 ; October 11, 2012: annotation                  |
|                       table updated with                        |
|                HumanHT-12_V4_0_R2_15002873_B.txt                |
+-----------------------------------------------------------------+
```


Solution for Question 2
========================================================
Reproduce your above query using the data.table package. Again, try to use a single line of code. [Hint: You first need to convert all db tables to data.table tables].





```r
## Set key 
setkeyv(gse, "gse")
setkeyv(gpl, "gpl")
setkeyv(gse_gpl, c("gse",  "gpl"))

res2 <- merge(gse[gse_gpl][grepl("HCV", summary)&grepl("yale.edu", contact), j=list(title, gse, gpl)],gpl[grepl("Illumina", manufacturer), j=list(gpl, manufacturer, description)], by="gpl")
```


Output for Question 2
========================================================


```


+----------+--------------------------------+----------+----------------+
|   gpl    |             title              |   gse    |  manufacturer  |
+==========+================================+==========+================+
| GPL10558 |   The blood transcriptional    | GSE40223 | Illumina Inc.  |
|          |    signature of chronic HCV    |          |                |
|          | [Illumina data]                |          |                |
|          |                                |          |                |
+----------+--------------------------------+----------+----------------+
| GPL10558 | Impaired TLR3-mediated immune  | GSE40812 | Illumina Inc.  |
|          | responses from macrophages of  |          |                |
|          | patients chronically infected  |          |                |
|          |     with Hepatitis C virus     |          |                |
+----------+--------------------------------+----------+----------------+

Table: Table continues below

 

+-----------------------------------------------------------------+
|                           description                           |
+=================================================================+
|                  The HumanHT-12 v4 Expression                   |
|                     BeadChip provides high                      |
|                   throughput processing of 12                   |
|                  samples per BeadChip without                   |
|                     the need for expensive,                     |
|                   specialized automation. The                   |
|                     BeadChip is designed to                     |
|                  support flexible usage across                  |
|                       a wide-spectrum of                        |
|                   experiments.; ; The updated                   |
|                  content on the HumanHT-12 v4                   |
|                  Expression BeadChips provides                  |
|                  more biologically meaningful                   |
|                   results through genome-wide                   |
|                   transcriptional coverage of                   |
|                 well-characterized genes, gene                  |
|                     candidates, and splice                      |
|                 variants.; ; Each array on the                  |
|                    HumanHT-12 v4 Expression                     |
|                   BeadChip targets more than                    |
|                   31,000 annotated genes with                   |
|                     more than 47,000 probes                     |
|                    derived from the National                    |
|                    Center for Biotechnology                     |
|                 Information Reference Sequence                  |
|                    (NCBI) RefSeq Release 38                     |
|                  (November 7, 2009) and other                   |
|                 sources.; ; Please use the GEO                  |
|                 Data Submission Report Plug-in                  |
|                 v1.0 for Gene Expression which                  |
|                     may be downloaded from                      |
|       https://icom.illumina.com/icom/software.ilmn?id=234       |
|                  to format the normalized and                   |
|                   raw data.  These should be                    |
|                     submitted as part of a                      |
|                  GEOarchive.  Instructions for                  |
|                 assembling a GEOarchive may be                  |
|                            found at                             |
| http://www.ncbi.nlm.nih.gov/projects/geo/info/spreadsheet.html; |
|                 ; October 11, 2012: annotation                  |
|                       table updated with                        |
|                HumanHT-12_V4_0_R2_15002873_B.txt                |
+-----------------------------------------------------------------+
|                  The HumanHT-12 v4 Expression                   |
|                     BeadChip provides high                      |
|                   throughput processing of 12                   |
|                  samples per BeadChip without                   |
|                     the need for expensive,                     |
|                   specialized automation. The                   |
|                     BeadChip is designed to                     |
|                  support flexible usage across                  |
|                       a wide-spectrum of                        |
|                   experiments.; ; The updated                   |
|                  content on the HumanHT-12 v4                   |
|                  Expression BeadChips provides                  |
|                  more biologically meaningful                   |
|                   results through genome-wide                   |
|                   transcriptional coverage of                   |
|                 well-characterized genes, gene                  |
|                     candidates, and splice                      |
|                 variants.; ; Each array on the                  |
|                    HumanHT-12 v4 Expression                     |
|                   BeadChip targets more than                    |
|                   31,000 annotated genes with                   |
|                     more than 47,000 probes                     |
|                    derived from the National                    |
|                    Center for Biotechnology                     |
|                 Information Reference Sequence                  |
|                    (NCBI) RefSeq Release 38                     |
|                  (November 7, 2009) and other                   |
|                 sources.; ; Please use the GEO                  |
|                 Data Submission Report Plug-in                  |
|                 v1.0 for Gene Expression which                  |
|                     may be downloaded from                      |
|       https://icom.illumina.com/icom/software.ilmn?id=234       |
|                  to format the normalized and                   |
|                   raw data.  These should be                    |
|                     submitted as part of a                      |
|                  GEOarchive.  Instructions for                  |
|                 assembling a GEOarchive may be                  |
|                            found at                             |
| http://www.ncbi.nlm.nih.gov/projects/geo/info/spreadsheet.html; |
|                 ; October 11, 2012: annotation                  |
|                       table updated with                        |
|                HumanHT-12_V4_0_R2_15002873_B.txt                |
+-----------------------------------------------------------------+
```


End of file

