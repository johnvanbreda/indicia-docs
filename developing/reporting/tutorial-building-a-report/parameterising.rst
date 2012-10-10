Parameterising the report
-------------------------

As well as the simple column filtering, it is possible to declare named 
parameters that must be input into a form before the report is run. To do this 
requires a ``<params>`` element in the XML document at the same level as the 
``<columns>`` and ``<query>`` elements. This new element contains a single 
``<param>`` element for each parameter you want to declare. Each parameter has 
attributes for the **name**, **display** (the display label), an optional 
**description** and a **datatype**. There are other attributes but these are the 
ones required for basic parameterisation. The parameter's name is wrapped with a 
# either side to make a replacement token which is then applied to the report 
query. As an example add the following parameters block to your report file in 
the appropriate place, plus update your where statement in the SQL to include 
the following filter using the following parameter and try it out: 

.. code-block:: xml

  <params>
    <param name="quality" display="Data quality" description="Quality level required of data to be included in the map." datatype="lookup" 
            lookup_values="V:Data must be verified,C:Data must be verified or certain,L:Data must be at least likely,!D:Include anything not dubious or rejected,!R:Include anything not rejected" />
  </params>
  
The parameter uses a lookup datatype to create a drop-down selection in the parameters 
form for the quality required of the records to include. Each entry is separated 
by a comma, with the actual value used preceding the colon and the label 
displayed to the user after the colon. 

.. code-block:: sql
  
  and quality_check('#quality#', o.record_status, o.certainty)=true
  
Note that we are using the ``quality_check`` function, which Indicia adds to the 
functions available in PostgreSQL in order to provide an easy way to filter 
records by the verification status and record certainty. You should find now 
that when you load the report in your Drupal site you are presented with a 
parameters input form as follows: 

.. todo::

  Screenshot of parameters form

Try selecting different options for the Data quality parameter to see what 
happens when you run the report. If you are stuck, here is the full text of the 
report at this stage: 

.. code-block:: xml

  <?xml version="1.0" encoding="UTF-8"?>
  <report title="Tutorial" description="Display some records for the report writing tutorial">
    <query>
    select #columns#
    from cache_occurrences o
    #agreements_join#
    where #sharing_filter# 
    and o.cache_created_on&gt;now()-'1 month'::interval
    and quality_check('#quality#', o.record_status, o.certainty)=true
    </query>
    <params>
      <param name="quality" display="Data quality" description="Quality level required of data to be included in the map." datatype="lookup" 
            lookup_values="V:Data must be verified,C:Data must be verified or certain,L:Data must be at least likely,!D:Include anything not dubious or rejected,!R:Include anything not rejected" />
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
    </columns>
  </report>
  
.. tip::
  
  It's also possible to use the population_call attribute to create a list of 
  options for the parameter drop-down via a query against the database. 