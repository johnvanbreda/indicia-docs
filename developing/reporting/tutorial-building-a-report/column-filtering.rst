Setting up column filtering
--------------------------- 

The next step is to enable filtering on a per-column basis. Indicia has not 
automatically done this for you since it does not know the type of data in each 
field yet so it does not know how to write the SQL to filter the records. We can 
do this using the ``datatype`` attribute of a column definition, which can be 
set to date, text, integer or float. Try this by adding an attribute 
``datatype="text"`` to the definition of the Species column, save the report 
file and reload the report in the warehouse. You should have an extra box at the 
top of the Species column so try typing the first few characters of a species 
name followed by * then tabbing out of the box to check it works. 

Next step, of course, is to apply the datatype attribute to the other column 
definitions. Your XML document should now look like: 

.. code-block:: xml

  <?xml version="1.0" encoding="UTF-8"?>
  <report title="Tutorial" description="Display some records for the report writing tutorial">
    <query>
    select #columns#
    from cache_occurrences o
    where o.cache_created_on&gt;now()-'1 month'::interval
    </query>
    <columns>
      <column name="id" sql="o.id" visible="false" datatype="integer" />
      <column name="public_entered_sref" sql="o.public_entered_sref" display="Grid Ref" datatype="text" />
      <column name="preferred_taxon" sql="o.preferred_taxon" display="Species" datatype="text" />
      <column name="default_common_name" sql="o.default_common_name" display="Common Name" datatype="text" />
      <column name="date_start" sql="o.date_start" visible="false" />
      <column name="date_end" sql="o.date_end" visible="false" />
      <column name="date_type" sql="o.date_type" visible="false" />
      <column name="date" display="Date" datatype="date"  />
    </columns>
  </report>
