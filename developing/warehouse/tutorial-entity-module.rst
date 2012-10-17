Tutorial: using a custom module to add an entity
================================================

In this tutorial we are going to kill two birds with one stone, by introducing
the code required for supporting a new database entity in the warehouse and 
using a custom module to do this. The Indicia data model is focused on the core
requirements of the majority of online recording surveys and is intentionally
kept fairly lightweight, as this makes it easier to program against as well as 
giving better querying performance in general. It is not uncommon, therefore, to 
come across requirements which necessitate extending the data model with new 
database tables. When doing this we could

* enhance the core code, but that increases the complexity of the data model so 
  is only beneficial when most online surveys will make use of the change.
* change the core code but keep the changes only on our local warehouse, but 
  that means that future updates to warehouse code could overwrite our local 
  changes making the upgrade process almost impossible to manage.
* write an optional module which adds support for the new database entity.

The latter approach is obviously the way to go since the module can be kept 
separate from the core warehouse code and can be enabled only on the warehouses
which need the additional functionality, therefore keeping the core both
lightweight and efficient. Fortunately, the Kohana framework has `built in 
support for extension modules <http://docs.kohanaphp.com/general/modules>`_.
