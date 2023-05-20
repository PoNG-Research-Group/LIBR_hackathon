# UKBiobank Data Exploration GUI

## Genetic Data

The genetic data is only available through LIBR server, will send out the info via secured channel accordingly.&#x20;

### Raw Imaging NIFTI

The bulk imaging nifti files are only available through LIBR server under controlled folder. We are in process of cross-study harmonization. For interested user, please contact us for further information. Meanwhile, for [the derived imaging variables from the official data release](https://biobank.ctsu.ox.ac.uk/crystal/crystal/docs/brain\_mri.pdf), you can find it in the LIBR UKB data protal [#tabular-data](ukbiobank-data-exploration-gui.md#tabular-data "mention").&#x20;

## Tabular Data

### URL

Provided through secure channel.&#x20;

### Login

The following screenshot is the login page, please use the appropriate login credential.&#x20;

<figure><img src="../../.gitbook/assets/image (1).png" alt=""><figcaption></figcaption></figure>

### Data Portal

#### Variable Selection

The following screenshot is the data portal page, where you can explore and download the tabular data.

<figure><img src="../../.gitbook/assets/image (5).png" alt=""><figcaption></figcaption></figure>

The header section lists the current session ID and data version. The password can be changed as well if you want.



The figure on the left side is an interactive ontology tree with clickable nodes. The tree structure follows the UKB categories ([https://biobank.ndph.ox.ac.uk/ukb/browse.cgi](https://biobank.ndph.ox.ac.uk/ukb/browse.cgi)). Two search bars on the right side support regex match (case sensitive) and keywords match (case-insensitive) for variable searching, respectively.&#x20;

The bottom search bar on the right side lists the currently selected variables. There are three ways to add variable into your selection:

1. ontology tree:&#x20;
   * _Leaf node with a pattern of "xxx: xxxxx" in its name_: click on the node, and it will be added to your selection.
   * _Enable "Group selection"_: click on any node, and all variables in that branch will be added to your selection.
2. regex match: type in a regular expression and click on "Search", a table with all matched variables will show under the search bar. Click on a single cell will add that variable into your selection, or click "Add all" to include all matched results.
3. search bar:&#x20;
   * has a drop-down menu for adding variables; variables listed in the drop-down menu will be restricted by entered keywords.
   * shows all currently selected variables.

Some basic information of selected variables will be list in the table underneath.

<figure><img src="../../.gitbook/assets/image (3).png" alt=""><figcaption></figcaption></figure>

#### Visit/Instance Selection

Some variables have multiple measures. This section is for choosing which measure(s) to use for each variable. If no visit/instance is selected, it will include all visits/instances by default.

After the visit/instance selection is finished, click "Finish selection".

<figure><img src="../../.gitbook/assets/image (2).png" alt=""><figcaption></figcaption></figure>

#### Visualization (Optional) & Download

After clicking on "Finish selection", there will a new section underneath, which tells you how many subjects have all data over the selected variables and visits/instances, as well as a button "Download data" allows to download the selected data. _**For the downloaded data, it will include all subjects even if some of them may have missing values in some variables.**_

_Optional_: if the "Show plots" is enabled before clicking on "Finish selection", the histogram/bar-plot will be presented.

<figure><img src="../../.gitbook/assets/image (8).png" alt=""><figcaption></figcaption></figure>

#### Downloaded Zip File

There will be four files inside the zip file.

* _txt_: the file contains basic information of the download, which are: session ID, user name, timestamp of clicking "Finish selection", data version, number and IDs of downloaded variables.
* _xlsx_: basic information of each downloaded variable (ID, description, type, coding scheme ID), as well as the coding schemes.
* _parquet_: [https://arrow.apache.org/docs/index.html](https://arrow.apache.org/docs/index.html)
  1. _tabular\_data\_raw:_ the raw data extracted from database, before translating from the code to its meaning.
  2. _tabular\_data\_decoded: the data with_ translation from the code to its meaning.

<figure><img src="../../.gitbook/assets/image (4).png" alt=""><figcaption></figcaption></figure>
