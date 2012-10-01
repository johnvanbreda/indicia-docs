Butterfly Transect Walks Example Setup
######################################

Indicia includes a prebuilt form designed for inputting flying insect counts 
along a transect walk which has been divided into sections. This form was 
designed for the `UKBMS <http://www.ukbms.org/>`_'s online recording facilities
but can also be used to record other flying insects such as Odonata.

Some details on how you might set this form up follow, including the setup of 
a calendar grid to break walks down into weeks through the season. Before 
following these steps you will need an installation of Instant Indicia (or 
Drupal plus the Indicia Forms module if you are also able to setup features
such as Easy Login) as well as an account on a warehouse.

Here are some files you may like to have available to import into the system:

+------------+-----------------------------------------------------------------+
| File       | Description                                                     |
+============+=================================================================+
| Counties   | If not using the list of counties provided on the warehouse then|
| CSV        | provide a list of the counties that sites will be allocated to. |
|            | This should be provided as a CSV file with a single column      |
|            | containing the county names, including a column title (which can|
|            | be anything you like).                                          |
+------------+-----------------------------------------------------------------+
| Habitats   | Provide a list of the habitats that will be available for       |
| CSV        | describing sites in your recording scheme.                      |
|            | This should be provided as a CSV file with a single column      |
|            | containing the county names, including a column title (which can|
|            | be anything you like).                                          |
+------------+-----------------------------------------------------------------+
| Management | Provide a list of the management terms that will be available   |
| CSV        | for describing sites in your recording scheme.                  |
|            | This should be provided as a CSV file with a single column      |
|            | containing the county names, including a column title (which can|
|            | be anything you like).                                          |
+------------+-----------------------------------------------------------------+
|            | This file is only required if your recording scheme is divided  |
|            | into regional branches. An ESRI SHP file format list of the     |
|            | boundaries of each branch, containing the branch names and the  |
|            | polygon defined in projection EPSG:27700 or EPSG:4326.          |
+------------+-----------------------------------------------------------------+
| Transects  | If you want to prepopulate the list of sites available for      |
| CSV        | transect walks, then provide a list of the transects as a       |
|            | spreadsheet in CSV format, including column titles. The         |
|            | spreadsheet should include the following columns:               |
|            |                                                                 |
|            | * **Site code**. Provide a unique numeric code for each site.   |
|            | * **Site name**. Provide a name for each site.                  |
|            | * **Grid ref**. Grid reference for each site's centroid.        |
|            | * **County**. Name of the county for each site. This must match |
|            |   one of the counties in the Counties CSV file.                 |
|            | * **Number of sections**. The number of sections the transect   |
|            |   is divided into. If this column is ommitted then you will     |
|            |   need to manually set the number of sections and input the     |
|            |   section details before inputting count data.                  |
|            | * **Overall transect length**. The length of the transect in    |
|            |   metres, can be omitted if not known.                          |
|            | * **Transect width**. The width of the transect in metres, can  |
|            |   be omitted if not known. You will need to know the distinct   |
|            |   list of widths in use so that you can later set up the        |
|            |   appropriate entries for the list of available widths.         |
|            | * **Year established**. Specify the 4 digit year each transect  |
|            |   was established if known.                                     |
|            | * **All or single species**. A column containing the word *All* |
|            |   or *Single* for each transect, depending on the transect type.|
|            | * **Sensitive**. A column containing *t* for sensitive sites    |
|            |   and *f* for all others.                                       |
|            |                                                                 |
|            | The exact column titles you use does not matter as long as you  |
|            | remember which is which.                                        |
+------------+-----------------------------------------------------------------+
| Sections   | If you want to prepopulate the list of sites available for      |
| CSV        | transect walks, then provide a list of the transects as a       |
|            | spreadsheet in CSV format, including column titles. The         |
|            | spreadsheet should include the following columns:               |
|            |                                                                 |
|            | * **Grid ref** (if not importing a SHP file). This can be the   |
|            |   same as the grid reference for the parent transect but the    |
|            |   recorders will need to draw their routes on a map before      |
|            |   inputting records.                                            |
+------------+-----------------------------------------------------------------+
| Sections   |                                                                 |
| SHP        |                                                                 |
+------------+-----------------------------------------------------------------+

Termlists
---------

Before setting up the form, you will need to have certain terms and termlists
configured on the warehouse as follows. All the termlists you create should be
owned by your website registration on the warehouse.

* Ensure that the existing Location Types termlist includes terms called
  **Branch**, **Transect** and **Transect Section**. Create them if they don't
  already exist. 
* If not using the existing list of counties, then create a termlist called 
  **BMS Counties** or similar. Use the warehouse import tool to import the CSV 
  file of counties into the list, setting the language to English for all rows
  and mapping your column to the **term** field.
* Create a termlist called **Transect width (m)** and setup terms called **5**,
  **6**, **10** and **other**. You can modify this list if your scheme has a
  different set of possible transect widths. 
* Create a termlist called **All or single species** with terms for **All** and
  **Single**. Set both terms to English language, with their sort order set to
  **0** and **1** respectively.
* Create a termlist called **Climate of transect** with terms for **Lowland** 
  and **Upland**. Set both terms to English language, with their sort order set
  to **0** and **1** respectively.
* Create a termlist called **Habitats**. Use the warehouse import tool to import
  the CSV file of habitats into the list, setting the language to English for 
  all rows and mapping your column to the **term** field.
* Create a termlist called **Management**. Use the warehouse import tool to
  import the CSV file of management terms into the list, setting the language to
  English for all rows and mapping your column to the **term** field.

.. todo::
  
  Custom attributes & remaining setup

