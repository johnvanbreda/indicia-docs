Report File Format
------------------

.. seealso::

  :doc:`tutorial-building-a-report/index`

Reports in Indicia are defined by placing XML files in the **reports** folder on
the warehouse which define the SQL query to run against the database, the
input parameters and the output columns. Report files must be valid XML 
documents with the following element structure.

Element <report>
================

This is the top level element of the XML document.

Attributes
^^^^^^^^^^

* **title** - the display title of the report.
* **description** - the description of the report.

Child elements
^^^^^^^^^^^^^^

* :ref:`query <query-label>`
* :ref:`order-bys <order-bys-label>`
* :ref:`field-sql <field-sql-label>`
* :ref:`parameters <parameters-label>`
* :ref:`columns <columns-label>`

Example
^^^^^^^

.. code-block:: xml

  <report title="My report" description="A list of records">
    ...
  </report>

.. _query-label:

Element <query>
===============

There is a single element called query in the XML document, which should be a 
direct child of the report element. This contains the SQL statement to run, 
which may contain modifications and replacement tokens to allow it to integrate
with the reporting system, as described elsewhere.

Attributes
^^^^^^^^^^

* *website_filter_field* - 

Replacements Tokens
^^^^^^^^^^^^^^^^^^^

* #columns#
* #field_sql# - see :ref:`field-sql-label` for information on using this 
  replacement token. If using #columns# then it is not necessary.
* #agreements_join#
* #sharing_filter#
* #idlist#
* #order_by# - When a report output is required in a particular sort order, e.g.
  after clicking on a column title in a grid to sort it, Indicia will append an 
  SQL ``ORDER BY`` clause to the end of the query. This token is only required 
  in the unusual circumstance that the clause needs to be inserted into the 
  query somewhere other than the very end of the report SQL, e.g. if it needs
  to precede a ``LIMIT`` statement. 

In addition any declared :ref:`parameters <parameters-label>` are available as 
replacement tokens.

Example
^^^^^^^

.. code-block:: xml

  <query website_filter_field="o.website_id">
  SELECT #columns#
  FROM cache_occurrences o
  JOIN websites w on w.id=o.website_id 
  #agreements_join#
  #joins#
  WHERE #sharing_filter# 
  AND o.record_status not in ('I','T') AND (#ownData#=1 OR o.record_status not in ('D','R'))
  AND ('#searchArea#'='' OR st_intersects(o.public_geom, st_geomfromtext('#searchArea#',900913)))
  AND (#ownData#=0 OR CAST(o.created_by_id AS character varying)='#currentUser#')
  #idlist#
  </query>

.. _order-bys-label:

Element <order_bys>
===================

.. _field-sql-label:

Element <field-sql>
===================

When the #field_sql# replacement token is used in the query, provide the SQL for
the list of fields in this element which will be replaced into the token when 
the query is run. The #field_sql# token should go immediately after the 
``SELECT`` keyword and before the ``FROM`` keyword to form a valid SQL statement
when it is replaced. This approach provides a quick way of allowing Indicia to 
perform a count of the records in a report without running the entire report
query, thus enabling grid pagination.

Example
^^^^^^^

.. code-block:: xml

  ...
  <query>SELECT #field_sql# FROM cache_occurrences</query>
  <field_sql>id, preferred_taxon_name, public_entered_sref</field_sql>
  ...

.. _parameters-label:

Element <parameters>
====================

Attributes
^^^^^^^^^^

* **name** - the name of the attribute. Must consist of alphabetic characters,
  numbers and underscores only. The attribute is wrapped in hashes to create the
  replacement token which will be replaced in the query. For example, if the 
  parameter named "startdate" is set to 01/10/2012, then the report might 
  include a clause ``WHERE date>'#startdate#'`` which would be replaced when the
  report is run to form the SQL ``WHERE date>'01/10/2012'``.

.. _columns-label:

Element <columns>
=================