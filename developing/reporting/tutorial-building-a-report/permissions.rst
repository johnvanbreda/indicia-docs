Integrating with website permissions
------------------------------------

The report we been writing has, to this point, one major flaw. There is nothing 
in the report which respects the permissions given to each client website 
regarding which records can be viewed. So, the report will allow a client 
website to view any record in the system, obviously not good. We need to 
implement a way of letting Indicia control the report to filter the records 
returned by website ID. Before going any further, its a good idea to set up a 
new Indicia Forms page on your Drupal website which uses this report as its 
source, leaving all the other settings to their default values. Give the report 
page a title and add it to the Drupal primary links menu so you can easily find 
it again. This gives us a report which we can use to test behaviour from the 
client website's perspective. 

There are 2 things you need to do to your report in order to get it to respect 
website permissions. Firstly, insert the tag ``#agreements_join#`` into the 
query in a suitable place for Indicia to insert more SQL join statements. This 
is because Indicia is going to need to join to the 
**index_websites_website_agreements** table to allow the report to include 
records from any website which has agreed to share records with your client 
website. So, the tag will need to go on a new line in the query, before the 
``where`` statement. 

.. tip::

  Indicia has a notion of **website agreements**, which are in effect like 
  contracts which declare how data are shared between the various client 
  websites. For example, a website can join the iRecord contract and opt to 
  provide records to iRecord for the purposes of verification, but not 
  reporting. 

We also need to tell Indicia where we would like any SQL inserted that affects 
the where clause of our query. So, we need to add a tag ``#sharing_filter#`` 
into the SQL statement's where clause. The result is an XML document which looks 
like the following: 

.. code-block:: xml

  <?xml version="1.0" encoding="UTF-8"?>
  <report title="Tutorial" description="Display some records for the report writing tutorial">
    <query>
    select #columns#
    from cache_occurrences o
    #agreements_join#
    where #sharing_filter# 
    and o.cache_created_on&gt;now()-'1 month'::interval
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
  
Try running the report again on your Drupal site. You will see only the records 
that your client website has permission to view for reporting purposes. 

.. tip::
  
  It can be difficult to be certain that things like the website permissions are 
  running correctly; if you are not aware of records that belong to the other 
  websites using the warehouse then how can you be sure these data are exluded 
  from the report? If the **log_browser** module is enabled on the warehouse, 
  then you can go to **Admin** > **Browse Server Logs** on the warehouse menu to 
  view logged messages. The level of detail available will depend on the 
  ``log_threshold`` setting in the application/config/config.php file on the 
  warehouse - typically we want this set to 4 for development purposes to give 
  maximum log detail. If you load the logs for the current day by clicking the 
  **Go** button then you should find the query that has just run near the 
  bottom, if not then check the log_threshold setting on the warehouse and load 
  the report again, then check the logs again. You should find that the 
  #agreements_join# in your query has been replaced by a join to the 
  index_websites_website_agreements table, filtered for the website ID your 
  client website is configured for. 