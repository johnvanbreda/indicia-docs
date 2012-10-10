Converting the query to a report
--------------------------------

Converting this query into a report which is available from within Indicia is 
fairly simple and can be achieved by pasting the query into the following 
template, replacing My title, My Description and My query as appropriate: 

.. code-block:: xml

  <?xml version="1.0" encoding="UTF-8"?>
  <report title="My title" description="My description">
    <query>
    My query
    </query>
  </report>

Copying in the query and giving it a title and description should give us 
something like the following. Note that we have remove the semi-colon from the 
end of the query as well as converted the query into XML compliant text by 
replacing the ``>`` with the XML safe equivalent ``&gt;``. 
`More information on the characters you might need to replace in this way 
<http://en.wikipedia.org/wiki/List_of_XML_and_HTML_character_entity_references#Predefined_entities_in_XML>`_.

.. code-block:: xml

  <?xml version="1.0" encoding="UTF-8"?>
  <report title="Tutorial" description="Display some records for the report writing tutorial">
    <query>
    select o.public_entered_sref, o.preferred_taxon, o.default_common_name, 
        o.date_start, o.date_end, o.date_type 
    from cache_occurrences o
    where o.cache_created_on&gt;now()-'1 month'::interval;
    </query>
  </report>

This would be a good time to fire up a text editor and copy this code into the 
editor. Now, create a folder using your name or initials in the warehouse's 
**reports** folder then save the report as **tutorial.xml** inside the folder 
you just created. Log into the warehouse if you are not already logged in then 
select **Entered Data > Reports** from the menu. Find the folder you created and 
expand it, then tick the Tutorial report's radio button before clicking the 
**Load report** button. You will see a basic table of records, showing the 
columns you included in the report query but with the 3 vague date columns 
automatically replaced by a single Date column. The grid has links at the bottom 
for next and previous pages, although they are pretty basic at this point, and 
supports clicking column titles to sort the records. There is also a fledgling 
filter box for the date column but this will only start working when we finish 
setting the report up properly, as at the moment all we have really done is the 
minimum required to link the report in by pasting the query into the XML 
template. 

.. tip::

  In the Indicia database, all the database tables and fields are documented 
  within the database itself - just look at the table or column comments using 
  pgAdmin to find out what they are for. 
