Kohana Conventions
==================

The Indicia warehouse code fulfills several objectives, including providing 
a user interface for managing the back-end data required for online recording
as well as the web-services required to support the client websites. To do this,
the code needs to respond to calls to certain web addresses, load information
from the database, apply business logic and return appropriate output. Any 
development framework worth its salt aims to make development of this sort of
application as simple as simple as possible, by

* making the code easier to write.
* limiting the amount of code you have to write so development is quick.
* simplifying coding which tends to make things more reliable and easier to maintain.

The obvious way in which a framework can achieve these goals is to provide a set
of classes or other library code which can perform the everyday tasks without
you needing to write substantial amounts of code. The Kohana framework goes 
beyond being a simple library of useful code functions in several ways, not only
because being an MVC framework, it expects you to split your code into models, 
views and controllers. Kohana also makes good use of on the principle of 
**convention over configuration**, that is, it relies on the notion that the 
developer should follow a set of conventions when writing code in order to allow
Kohana to make certain assumptions. For example, in a traditional framework one
might have to write a configuration file with content such as the following 
fictitious example in order to use a database access class::

  table_name=survey
  model_file_name=models/survey_data_access_model.php
  model_class_name=Survey_Model

This would enable the framework to work on the name of the PHP file to include
and the class name to instantiate when it needs to load the class responsible
for accessing survey data. There are several problems with this approach:

* There is quite a lot of configuration to write to support the whole system.
* There is a risk of inconsistency in the naming conventions, file locations 
  and so forth. There is nothing stopping a model class being stored in a 
  different folder or having a typo in the file name for example.

The principle of convetion over configuration means that the Kohana framework
avoids the necessity for extensive configuration files by publishing conventions
regarding things like class names, file names and locations. The framework code
can then be written with the assumption that the conventions are followed and
because the code will simply not work if conventions are not followed, it forces
consistency. It also saves time once the developer has learnt these conventions
since there is no need to write the configuration for each thing you do. Rather 
than go into detail about all the conventions here you can read about them in 
the Kohana documentation. 

.. todo::

  Turn last 2 words into hyperlink to kohana conventions page.

Folder Structure Conventions
----------------------------

There are a few conventions regarding the folder structure it would be handy to
mention at this stage:

* The Kohana framework code is kept in the **system** folder. This code should 
  not be touched.
* The code for the application you write is kept in the **application** folder.
* Code for models (classes which provide access to an entity in the database)
  is kept in **application/models**.
* Code for views (code which provide the user interface) is kept in the 
  **application/views** folder.
* Code for controllers (classes which provide business logice and the glue 
  between models and views) is kept in the **application/controllers** folder.
* The application can be extended by **modules**. Each module is represented by
  a single folder in the **modules** folder. Modules can contain their own 
  models, views and controllers in subfolders of the same name.
* When you write code that requires the Kohana framework to load a particular
  piece of code, Kohana will autoload the required PHP file. It will first look 
  in the enabled modules, then the application folder, then the system folder.
  This means that modules can override existing application functionality by
  providing different versions of the code files.

How do URLs relate to controllers?
----------------------------------

When you access a website, you do so by hitting certain web addresses which then
return appropriate responses, normally HTML. Kohana has a simple convention 
allowing it to map web addresses you write. All web addresses actually go to a
single PHP file called **index.php** in the root folder and the path given after
this address dictates which bit of code is used to provide the response. For
example there might be a web address available on your website::

  http://www.example.com/index.php/survey/create

When this web address is accessed, Kohana's framework code will look through the
available controller code files for one called survey.php. It will first look in
the controllers folder for each of the enabled modules, then it will look in the
application/controllers folder, finally it will look in the system/controllers
folder. Once it finds the appropriate PHP file, it then loads the file and
attempts to create an instance of a class called ``Survey_Controller``, which 
is the name we must give to our controller class for providing the 'glue' 
required for the database's surveys table according to the Kohana conventions.

Once the class has been created, the Kohana framework code then calls the
`create` method in the class, because if you look back at the URL we are 
accessing you will see that was the name given in the second part of the URL 
path. This method provided by the controller is often called the *action*. 
Therefore we could summarise the web addresses which our Kohana based site
using the following pattern::

  http://{site-domain}/index.php/{controller}/{action}

So, each controller declares a class with public methods that map to a single 
URL path in the application. The controller code is responsible for coordinating 
the database access logic in the model with the view template code to produce the 
required output. 

.. topic:: A look at the code

  If you have access to a development warehouse's files, then see if you can 
  find the survey.php file in the application/controllers folder. You will 
  actually not see a create function in the class as you might expect, because
  the `Survey_Controller` class *inherits* from a class called 
  `Gridview_Base_Controller`, which in turn inherits from a class called
  `Indicia_Controller`. This means that the survey controller inherits the 
  available functions in these parent classes, which includes a create function
  in the `Indicia_Controller` class. So, this is the function that will be 
  called when the index.php/survey/create web address is accessed.

  See if you can find the create function in the code. Once you've found it
  try inserting something like `echo "I am here!";` above the first line of code
  in the function and saving the file. Now try creating a survey using your 
  warehouse and see what happens. Don't forget to remove the line of code when 
  you are done!