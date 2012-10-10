Integrating with custom attributes
----------------------------------

Our report is starting to take shape now. We could easily extend the query to 
include other fields, but how should we approach the issue of custom attributes. 
We are trying to write our report code to be as generic as possible but each 
survey set up using Indicia has its own set of attributes so needs a different 
query to get all the data out. We don't want to have to write new report files 
for every survey we set up. Fortunately the Indicia reporting engine supports 
special parameter types **smpattrs**, **occattrs** and **locattrs** which allow 
the report to be called with a list of sample, occurrence or location custom 
attributes to be added to the report output as an input parameter. 

.. note::

  Normally, you would not expect the user to input a list of custom attributes 
  to report against, but the report controls and the Drupal prebuilt forms for 
  reporting allow you to set preconfigured parameter values for any parameter 
  and therefore for this parameter to be hidden from the parameters form. So 
  configuring the list of attributes included in the report becomes a task for 
  the person setting up the report page. 

Since our report does not support location data we are going to add the 
following parameters to the ``<params>`` block to allow sample and occurrence 
custom attributes to be included in the report:

.. code-block:: xml

  <param name="smpattrs" display="Sample attribute list" description="Comma separated list of sample attribute IDs to include" datatype="smpattrs" />
  <param name="occattrs" display="Occurrence attribute list" description="Comma separated list of occurrence attribute IDs to include" datatype="occattrs" />
  
Note that in order to use the special custom attribute parameter types we 
must fulfil certain criteria. 

* We need to add a ``#joins#`` tag to the report SQL to tell Indicia where to 
  put the additional joins required to include the attribute data. 
* The query must include a table which contains the ID attribute that the 
  attribute values are linked to, for example the sample ID, occurrence ID 
  or location ID.
* If the ID fields can be referred to in the SQL using s.id, o.id and l.id 
  then no further changes are required. You can override these defaults, for 
  example if you have a query listing occurrences which does not join in the 
  samples table but need to be able to add sample attribute values. The query 
  element needs an attribute samples_id_field which identifies the field 
  reference that can be used in the SQL to join to the sample, in this case 
  o.sample_id. 

The ``#joins#`` tag can normally go into the query in the same place as the 
``#agreements_join#`` but on occasion it might be necessary to put these join 
tags in different parts of the query, which is why there is a separate tag 
rather than just one. For our purposes the tag can go on the line after the 
existing ``#agreements_join#`` tag, so our report now: 

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
    </columns>
  </report>

Update your report to match and run the report. If you don't already have any 
custom attributes configured for one of the surveys accessible to your client 
website then this would be a good time to configure one. You can then try 
specifying the custom attribute using the ID or attribute caption in the 
parameter to check it works. 