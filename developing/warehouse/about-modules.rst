About Warehouse Modules
=======================

The Kohana framework provides useful support for extensibility via the concept
of modules, which allow the provision of additional models, views and 
controllers as well as the overriding of existing code from the main 
application. Indicia's warehouse extends this idea by allowing modules to hook
into various bits of core functionality. For example, Indicia allows a module
to provide additional items to add to the main menu or new tabs for an existing
page. To achieve this the Indicia warehouse code has a set of conventions which
must be followed, rather like those which are pubished for the Kohana framework:


* AS module must contain a folder called *plugins*.
* In this folder there must be a PHP file with the same name as the module ,
  itself, so for example the cache_builder module has a file called 
  cache_builder.php in the modules/cache_builder/plugins/ folder.

.. todo::

  Copy across other documentation on plugins from the Wiki.