# Citations
Testing for Causal Evidence on Data Sharing and Citations

See https://osf.io/cdt8y/ for more info.

To update the research log, click [here](https://docs.google.com/a/berkeley.edu/document/d/1XC20bRYeNCIrH-XqrU8tqs2K5fdqaGenZ41ZlpTg2QM/edit?usp=sharing).

Draft of the entire data collection process is [here](https://docs.google.com/document/d/1zPoa1W5Ysd-5aFIp1Qz_j2zJKnSaVzC072d625MkjFc/edit?usp=sharing).

# Project Status
## Data Collection

| |AJPS|APSR|QJE|AER|
|---|:---:|:---:|---|---|
|Scraping|Done|Done|In Progress|Not started|
|References|Done|Done|Not started|Not started|
|Links|Done|Done| NS | NS|
|Author websites|Done|Done|NS|NS|
|Article types|Done |Done|NS|NS|
|Dataverse|Done|Done|NS|NS|
|Citations|In progress-Evey|Next up-Evey|NS|NS|

## Other Tasks
+ Define final file format for collected data
+ Polish code
  + Refactor functions
  + Improve documentation
+ Update pre-analysis plan

## Draft Schedule:
Feb 10-16: Article topic & type, finish up and resolve differences

Feb 17-23: T&R: APSR Link coding only 13 links
Dataverse: AJPS (118), APSR (26-more resolution differences

Feb 24: 
Meet with Ted
T&R-plan how to combine datasets
E&B-code to scrape citations

April 21: Meet with Ted
Evey--complete citations
Baiyue--gather QJE text
T&R--merge author websites, links, dataverse, references later

# Folder Structure
The folder structure of this repository grew organically over the development of the project. There is also a Box folder `URAPshared` that contains several files not contained in this repository. The purpose of the Box folder is to manage the raw data from scraping and data entry. In the following, if a folder `x` is contained in the Box folder, then it will be denoted as `URAPshared/x`.

## Octoparse
Data collection started with the journals AJPS and APSR and with the use of Octoparse. The folder `octoparse` contains the `otd` files to run this data collection. Octoparse uses `otd` files as instructions on how to collect data.

The output from the AJPS and APSR data collection is stored in `URAPshared/octoparse` in the following files

+ `apsr_article_content_2006_2014.csv`
+ `apsr_issue_toc.csv`
+ `ajps_article_content_2003_2007.csv`
+ `ajps_article_content_2008_2012.csv`
+ `ajps_article_content_2013_2016.csv`
+ `ajps_issue_toc.csv`


We collected article information from the page of each journal issue's table, and then separately collected article content from the individual article pages. This is what `article_content` and `issue_toc` refers to in the file names.

The collection with Octoparse has a few issues.

For AJPS, there are two issues. First, memory issues required us to split the data collection into several year ranges. Second, the output files sometimes contain multiple rows for the same article, breaking the csv format. The script `data_preparation/combine_octoparse_outputs.py` fixes these issues by combining the files `ajps_article_content_2003_2007.csv`, `ajps_article_content_2008_2012.csv` and `ajps_article_content_2013_2016.csv` into `bld/ajps_articles_2003_2016.csv`. Analogously, `data_preparation/create_article_info_from_issue_toc.py` cleans `ajps_issue_toc.csv` into `bld/ajps_article_info_from_issue_toc.csv`.
 
 APSR had a centennial edition which we missed in the first run of data collection (we believe due to a different page layout), and hence collected separately later on. The corresponding file is `apsr_article_content_centennial.csv`. The script `data_preparation/clean_apsr_octoparse_output.py` cleans  `apsr_issue_toc.csv`, `apsr_article_content_2006_2014.csv` and `apsr_article_content_centennial.csv` and writes them into `bld/apsr_article_info_from_issue_toc.csv`, `bld/apsr_article_content_2006_2014.csv` and `bld/apsr_centennial_article_content.csv`.

## Data Entry
There are five categories of data entry.

+ Article: What is the research field of an article, what type of data does it use?
+ Author website: Does an article author's website contain any data or code for the article?
+ Dataverse: Are there any files corresponding to the article on Dataverse?
+ Reference: Does the article content contain a reference to data or code that is useful to reproduce the article's results?
+ Link: Do URLs in the article content that reference code or data, actually lead to such?

The folder `data_entry` contains an `md` file describing the protocol on how to enter data for each of these categories. It also contains `reference_coding_protocol_questions.md`, a collection of clarifying questions on the protocol.

The process for data entry works as follows:

1. Every combination of journal and data entry category has an `ods` template.
2. Several people can work on filling out a template by copying it to their local machine and adding their initials to the file name.
3. The data entered from several people on the same template is then compared. Differences in entry will be detected, and marked in a difference resolution template.
4. Pairs with differing data entry then meet to resolve differences, and record those in a single copy of the difference resolution file.
5. The resolved differences are then "harmonized" across difference resolution pairs into a single file.  

Each of these steps has corresponding scripts and folders:

1. Create templates
    + Article: `data_preparation/create_article_coding_template.py`
    + Author: `data_preparation/create_ajps_author_website_coding_template.py`, `data_preparation/create_apsr_author_website_coding_template.py`
    + Dataverse: `data_preparation/extract_from_dataverse.py`
    + Reference: `data_preparation/create_ajps_reference_coding_template.py`, `data_preparation/create_apsr_reference_coding_template.py`, `data_preparation/create_econ_reference_coding_template.py`
    + Link: `data_preparation/create_link_coding_template.py`
2. Upload template. Individual versions of filled out templates are stored and synchronized in `URAPShared/Data`.