Images and maps
---------------

Including photos in report grids
================================

As well as output of textual information into the grid columns, report output 
can include photos which the report grid control will automatically display as 
thumbnails with links to the full size image. Simply include a column containing 
the image file name (relative to the upload path on the warehouse) and you can 
include multiple images in the column simply by comma separating the file names. 
Handily the **cache_occurrences** table includes a column called **images** 
which uses exactly that format. Also, you must declare the column with an 
attribute called ``img`` set to true for the column to behave as an image. Try 
adding the following to the relevant section of your report: 

.. code-block:: xml

  <column name="images" display="Images" sql="o.images" img="true" />
   
Outputting your records onto a map
==================================

Instead of using the Report Grid prebuilt form, if you use the Report Map 
prebuilt form (also in the Reporting category) you can output a map of the 
records in your report. So, set up a new prebuilt form for a Report Map, give it 
a title and add it to the primary links menu as before. Configure it to use the 
report you have been creating by setting the **Report Name** option under 
**Report Settings**. To make it easier, specify the following in the **Preset 
parameter values** so you don't need to keep filling in the parameters form:: 

  smpattrs=
  occattrs=
  quality=!D
  
Now, go to the **Base Map Layers** section and select the Google Hybrid layer. 
There are other options, in particular those under **Report Map Settings** but 
the defaults are fine for now. Save the page and view the results. You should 
see the map, but with a message output at the top:: 

  The report's configuration does not output any mappable data
  
Ok, so we need to extend our report to include a column which includes mappable 
information. PostGIS (the spatial database extension for PostgreSQL which we use 
to store the locality of the records) stores grid references or other boundary 
and point data internally using a binary format which Indicia cannot use 
directly, so we need to pass the geometry binary data for each record through 
PostGIS's ``st_astext`` function. Don't worry if this does not make sense you 
can just copy this technique to make other reports mappable! So, add the 
following column definition to the report, noting the use of the **mappable** 
attribute to tell Indicia that it contains mapping data. 

.. code-block:: xml

  <column name="geom" visible="false" mappable="true" sql="st_astext(o.public_geom)" />
  
Save that and try reloading the Drupal Report Map page you created. You should see the dots now appear on the map. For reference, here is the entire report XML document you should have created:

.. code-block:: xml

  <?xml version="1.0" encoding="UTF-8"?>
  <report title="Tutorial" description="Display some records for the report writing tutorial">
    <query samples_id_field="o.sample_id">
    select #columns#
    from cache_occurrences o
    #agreements_join#
    #joins#
    where #sharing_filter# 
    and o.cache_created_on&gt;now()-'1 month'::interval
    and quality_check('#quality#', o.record_status, o.certainty)=true
    </query>
    <params>
      <param name="quality" display="Data quality" description="Quality level required of data to be included in the map." datatype="lookup" 
              lookup_values="V:Data must be verified,C:Data must be verified or certain,L:Data must be at least likely,!D:Include anything not dubious or rejected,!R:Include anything not rejected" />
      <param name="smpattrs" display="Sample attribute list" description="Comma separated list of sample attribute IDs to include" datatype="smpattrs" />
      <param name="occattrs" display="Occurrence attribute list" description="Comma separated list of occurrence attribute IDs to include" datatype="occattrs" />
    </params>
    <columns>
      <column name="id" sql="o.id" visible="false" datatype="integer" />
      <column name="public_entered_sref" sql="o.public_entered_sref" display="Grid Ref" datatype="text" />
      <column name="preferred_taxon" sql="o.preferred_taxon" display="Species" datatype="text" />
      <column name="default_common_name" sql="o.default_common_name" display="Common Name" datatype="text" />
      <column name="date_start" sql="o.date_start" visible="false" />
      <column name="date_end" sql="o.date_end" visible="false" />
      <column name="date_type" sql="o.date_type" visible="false" />
      <column name="date" display="Date" datatype="date"  />
      <column name="images" display="Images" sql="o.images" img="true" />
      <column name="geom" visible="false" mappable="true" sql="st_astext(o.public_geom)" />
    </columns>
  </report>

.. tip::

  If you are coding using PHP directly instead of prebuilt forms in Drupal, you 
  can achieve the same mapping output from your report data using the 
  report_helper::report_map control. 

The options you've covered for building this report file are not exhaustive but 
hopefully you now have a good taste for what can be achieved. 