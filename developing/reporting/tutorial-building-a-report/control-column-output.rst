Controlling the column output
-----------------------------

Columns are defined by creating an element in the XML document called 
``<columns>`` at the same level as the ``<query>`` element. This contains 
optional definitions of each column in the report in ``<column>`` sub-elements
with the attributes of each column definition defining which database field it
links to as well as the output behaviour. Therefore the structure might look 
like: 

.. code-block:: xml

  <?xml version="1.0" encoding="UTF-8"?>
  <report title="Tutorial" description="Display some records for the report writing tutorial">
    <query>
    ...
    </query>
    <columns>
      <column ... />
      <column ... />
      <column ... />
    </columns>
  </report>

The first thing we are going to do with the columns is to add a new column for 
the ID of the record, but to then hide it from the default grid output leaving 
the id available for code to use but not for the user to see. For example we 
might want to add a link to the rows in the table which is to a page which 
accept's the ID as a URL parameter and displays some information about that 
record. First, edit the SQL to include the column called ``id`` at the start of 
the list of columns. Then, create a ``<columns>`` section in your report file 
with a single ``<column>`` defined as follows: 

.. code-block:: xml

  <column name="id" visible="false" />
  
Save your report file and reload the report in the warehouse. You shouldn't 
notice any difference at this stage because the id field has been set to 
``visible="false"`` but this field is available for use in the various 
configuration and templating options available in the report grid. 

Now, add 3 new columns to your report for the other 3 standard columns 
(excluding date which already has a proper caption), using the **caption** 
attribute of the column definition to set their captions to **Grid Ref**, 
**Species** and **Common Name** respectively. Your report file should look like 
this: 

.. code-block:: xml

  <?xml version="1.0" encoding="UTF-8"?>
  <report title="Tutorial" description="Display some records for the report writing tutorial">
    <query>
    select o.id, o.public_entered_sref, o.preferred_taxon, o.default_common_name, 
        o.date_start, o.date_end, o.date_type 
    from cache_occurrences o
    where o.cache_created_on&gt;now()-'1 month'::interval
    </query>
    <columns>
      <column name="id" visible="false"/>
      <column name="public_entered_sref" display="Grid Ref"/>
      <column name="preferred_taxon" display="Species"/>
      <column name="default_common_name" display="Common Name"/>
    </columns>
  </report>
  
Reload the report to check that the column titles have been updated.