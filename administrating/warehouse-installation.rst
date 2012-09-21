Installing the Indicia warehouse
================================

This guide is a quick summary of the steps required to install the Indicia 
Warehouse. You don't have to install the Warehouse to use Indicia if you are 
happy to use a Warehouse provided by another organisation. Note that installing 
the Warehouse involves setting up a web server since Indicia is a web, rather 
than a desktop, application. Therefore installation is a little more involved 
than the typical desktop application.

Note that if you are installing Indicia onto a hosted shared server, the chances
are you will not have the full privileges required to setup the PostGIS database
and PHP settings (if required). It may be helpful to ask your web host to 
perform the steps :ref:`install-postgres` and to check that the **pgsql** and
**cURL** extensions are enabled for PHP as described in the section 
:ref:`install-php`.

There is a screencast of this installation process on a Mac available at 
http://www.youtube.com/watch?v=wSfRJK9q2gs. The steps are similar for Windows.

.. _install-postgres:

Install PostgreSQL and PostGIS
------------------------------

First, install the PostgreSQL database server and the PostGIS extension for your 
operating system. For downloads and more information see 
http://www.postgresql.org/ and http://postgis.refractions.net/. You should 
install version 8.4 or later of PostgreSQL, preferably 9.x, though if installing
on Windows it is worth reviewing the `Windows installation notes 
<http://postgis.refractions.net/download/windows/>`_ before selecting a version. 
For example, at the current point in time version 9.1 is the latest version 
which supports installation of PostGIS using the Application Stack Builder, so 
opting for this version rather than 9.2 will make things simpler later.

If you are installing PostgreSQL on a Linux server then you may find the 
following links extremely useful:

* `An almost idiot's guide to installing PostgreSQL 9.0 with Yum <http://www.postgresonline.com/journal/archives/203-postgresql90-yum.html>`_
* `An almost idiot's guide to installing PostGIS 1.5 on PostgreSQL 9.0 via Yum <http://www.postgresonline.com/journal/archives/204-postgis15-install-yum.html>`_

Next, create a PostGIS database on your server. You don't need to put any 
content in the database yet. There are several possible ways of doing this, but 
the easiest is to run pgAdmin III which is supplied in the PostgreSQL 
installation. In the tree on the left, there is a node called Servers, and 
underneath that is your server. Double click this and enter your password if it 
asks you. Now, expand the Databases node, select "postgres", then click the icon 
in the toolbar that looks like a document titled SQL with a pencil. Now, in the 
editor that appears, enter the following SQL script and click the green run 
button in the toolbar, which creates a PostGIS database called indicia.::

  CREATE DATABASE indicia TEMPLATE=template_postgis;

A word of warning. After choosing the 'Server' from the left hand pane within 
pgAdmin, a list of existing databases will be displayed for that server, 
including the 'template_postgis'. One of these databases will be opened by 
default - unfortunately for a blank installation this will be the 
'template_postgis' database as there are no others. In order for the above 
statement to work, this must not be active (i.e. it must have a red cross over 
its icon) - pgAdmin will complain that another user (in this case itself) has 
the template database open. In this case, the problem may be circumvented by 
creating a blank database with no template (e.g. called 'X'), shutting down 
pgAdmin, and then restarting it. On reopening the relevant server, the dummy 
database should be active, and the 'template_postgis' one not. The above 
statement should now be able to be run without issue. The dummy database may 
then be deleted.

You can use the postgres super-user account to run Indicia if you like, which is
the easiest but least secure method. Please do **NOT** use the super-user account 
for anything other than development purposes. Assuming you want to create your 
own user to run Indicia, run the following script to create the user and add it 
to the database, changing the username, password and database name as required: ::

  CREATE USER indicia_user WITH PASSWORD 'indicia';
  GRANT ALL PRIVILEGES ON DATABASE indicia TO indicia_user;

Now, connect your query tool to the new indicia database by using the drop down 
in the toolbar then selecting <new connection>, and run the following script 
which allows the Indicia user just enough rights to access the PostGIS 
functionality: ::

  GRANT ALL PRIVILEGES ON TABLE geometry_columns TO indicia_user;
  GRANT ALL PRIVILEGES ON TABLE spatial_ref_sys TO indicia_user;
  GRANT EXECUTE ON FUNCTION st_astext(geometry) TO indicia_user;
  GRANT EXECUTE ON FUNCTION st_geomfromtext(text, integer) TO indicia_user;
  GRANT EXECUTE ON FUNCTION st_transform(geometry, integer) TO indicia_user;

.. _install-php:

Install PHP and a web server
----------------------------

If you are installing on Windows, then you will typically want to use IIS 
(Internet Information Services) or Apache as your web server software. For other
operating systems we recommend the Apache web server. This tutorial is written 
on the assumption that you are using Apache, though the steps will mostly be the
same (except with different folder locations) for IIS.

There are many tutorials on the web on how to install PHP and a webserver such 
as Apache. PHP version 5.2 or higher is required. For example, the following 
guide explains installation of PHP, Apache and MySQL on Windows: 
http://www.php-mysql-tutorial.com/install-apache-php-mysql.php. MySQL is not 
required by the Indicia Warehouse. Another alternative is to install a ready 
made 'stack' with a set of predefined components all packaged into one install 
kit. An example is XAMPP though note that version 1.7.4 removed several modules 
from previous versions making Indicia installation more difficult. You can 
download version 1.7.3 for Windows from 
http://sourceforge.net/projects/xampp/files/XAMPP%20Windows/1.7.3/.

After installing PHP, edit your php.ini file and uncomment the following two 
lines by removing the semi-colon at the start. This enables the pgsql module 
required for PHP to access the PostgreSQL database, and the cURL module which 
the demonstration site pages use to access the web services. After you've 
changed and saved the file, restart your Apache web server. ::

  extension=php_curl.dll
  extension=php_pgsql.dll

Once you have done this, it's a good idea to check that the cURL and pgsql 
libraries have been installed successfully for PHP. You can do this by creating 
a file called phpinfo.php in the root html directory of your webserver, and 
editing it with a text editor. If you installed XAMPP, then you will find this 
folder under XAMPP/htdocs. Enter the following text into the file and save it:

.. code-block:: php

  <?php
  echo phpinfo(); 
  ?>

Now go to a web browser, and enter the root of your webserver followed by 
phpinfo.php (e.g http://localhost/phpinfo.php). The page you see should detail 
your PHP configuration, and if you look down the page you should see that the 
cURL and pgsql libraries are loaded.

*Tip: If you have installed PHP 5.2.6 and the pgsql library won't load, this may 
be because of a bug in this release of PHP making it incompatible with the 
version of PostgreSQL you have installed. To fix this, you will need to replace 
the file php_pgsql.dll in your PHP installation with the version from the 
PHP 5.2.5 download, and also replace the file libpq.dll from your PostgreSQL 
install folder with the one from this download. Another problem can occur when 
loading the pgsql libraries for PHP on a Windows Apache server, because of the 
paths not being correct. For more information on this issue see 
http://stackoverflow.com/questions/551734/php-not-loading-php-pgsql-dll-on-windows.*