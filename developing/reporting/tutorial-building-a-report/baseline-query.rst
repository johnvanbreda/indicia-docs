Our baseline query
------------------

Indicia includes several methods of reporting on the information it holds but 
underpinning most of these is an XML format which defines a report query, the 
input parameters and output columns. Once you have got to grips with this format 
it becomes possible to integrate the reports into tables, charts, maps and data 
downloads, or even to expose the report output to your own code via web 
services. Let's start by choosing a fairly simple query then look at the 
possibilities available when this query is integrated into the reporting system. 
The following query returns all records added to the system in this month: 

.. code-block:: sql 

  select o.public_entered_sref, o.preferred_taxon, o.default_common_name, 
      o.date_start, o.date_end, o.date_type 
  from cache_occurrences o 
  where o.cache_created_on>now()-'1 month'::interval; 

.. tip::

  Rather than using the **occurrences** table and joining to other tables to 
  get the sample date, species names and so on, the handy **cache_occurrences** 
  table is used which gives us a quick and easy way to get the key bits of 
  information about a record. There are also tables **cache_termlists_terms** 
  and **cache_taxa_taxon_lists** available to simplify other queries where 
  appropriate. Not only do these tables make your queries simpler to write, but 
  they also improve the performance of the query considerably. 

.. tip::

  Secondly, the query uses a bit of PostgreSQL specific syntax. It provides a 
  piece of text ``'1 month'`` followed by a double colon, which tells the 
  database that we want to *cast* the text as another type of data, that is, we 
  want to convert it. By converting the text into an *interval* PostgreSQL is 
  able to understand that this text actually means an interval of time and to be 
  able to use it as such. 

Whilst not being a very impressive query in its own right, let's take a look at 
how to turn this into an Indicia report, then to extend the capabilities of the 
report to make it quite a bit more powerful. 