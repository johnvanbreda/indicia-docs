Using the advanced pagination facility
--------------------------------------

At this stage, the pagination at the footer of the report grid is a simple 
*previous* and *next* link which is not really up to the standards expected of a 
modern user interface. This is because we haven't structured our report in a way 
that lets Indicia run a ``COUNT`` version of the query to find the total 
records, therefore it does not know how many pages there are. The best way to 
rectify this is to replace the list of fields in the query with a tag which 
Indicia can replace with the field list, then provide the SQL for each field in 
the column definition. The tag needed is called ``#columns#`` and the attribute 
required to specify the SQL is simply called ``sql``, meaning that our query 
must be changed to: 

.. code-block:: sql

  select #columns#
    from cache_occurrences o
    where o.cache_created_on&gt;now()-'1 month'::interval

An example of a column definition using the ``sql`` attribute is:

.. code-block:: xml

  <column name="id" visible="false" sql="o.id"/>

Indicia will simply join all the column SQL with commas and replace the 
``#columns#`` tag with the result when running the report query. When Indicia 
needs the total record count in order to build the paginator, it simply replaces 
the ``#columns#`` tag with ``count(*)``. We also need to know how to handle the 
special date column, since this is a construct of the start, end and type fields 
of the vague date. The best approach to this is to declare columns for each of 
the 3 vague date fields and set visible to false, then to declare another column 
named *date* which allows us to control the position of the date column in the 
report output. Hence the report should now look like the following: 

.. code-block:: xml

  <?xml version="1.0" encoding="UTF-8"?>
  <report title="Tutorial" description="Display some records for the report writing tutorial">
    <query>
    select #columns#
    from cache_occurrences o
    where o.cache_created_on&gt;now()-'1 month'::interval
    </query>
    <columns>
      <column name="id" sql="o.id" visible="false"/>
      <column name="public_entered_sref" sql="o.public_entered_sref" display="Grid Ref"/>
      <column name="preferred_taxon" sql="o.preferred_taxon" display="Species"/>
      <column name="default_common_name" sql="o.default_common_name" display="Common Name"/>
      <column name="date_start" sql="o.date_start" visible="false" />
      <column name="date_end" sql="o.date_end" visible="false" />
      <column name="date_type" sql="o.date_type" visible="false" />
      <column name="date" display="date" />
    </columns>
  </report>

Update your tutorial report file and save it, then reload the report in the 
warehouse. You should notice the the report grid now has a much more useful 
paginator including a total record count at the bottom. 