Technology Primer
-----------------

Before starting with Instant Indicia, it’s worth taking a few moments for a 
quick primer of the technologies involved in a setup of Instant Indicia. 

Installing Indicia is not like installing an application to run locally
on your computer because it is designed to run from a web browser over the 
internet. When you view a web page over the internet, your web browser has sent 
a request for information to another computer called a web server. This is a 
specially configured computer with a permanent connection to the internet that 
is set up to respond to your requests by sending back web page content as 
appropriate. Although this is the typical paradigm of the internet, it is 
perfectly possible to use the same technologies to install Instant Indicia on a 
local web server on your computer and access it from the web browser on the
same computer. In fact this is what we typically do as developers of products 
like Indicia. Installing Instant Indicia from scratch is a bit more complicated 
than installing a typical desktop application simply because there are more 
components required to set up the web server.

There are quite a few different web servers which you can install on most 
machines. The two most commonly used contenders are IIS (Internet Information 
Services, Windows only) and Apache (Windows, Mac, Linux). Because the latter is 
the most widely used web server on the internet today Apache is a good choice if
you don’t have any other reason for your selection of web server. Another point
to be aware of is that when you purchase some web space from a host, the typical
low cost options are *shared web servers*, that is, the server is shared between 
your website and a number of other websites. That is how the host can make money
by only charging a few pounds a month.

All web servers have one thing in common – they are designed to receive requests
from web browsers and other web enabled applications, and to respond to those 
requests by sending back web content and other data. When you build a website,
ultimately you are building a set of web pages which can be viewed on a browser.
The content you write for your website is returned to the browser as text with 
special tags inserted into it to denote formatting, links to images and so 
forth. For example to output emphasized (italic) text the text can be marked up 
as follows: ::

  <em>This text is emphasized</em>. This text is normal.

This would appear in the browser as:

  *This text is emphasized.* This text is normal.

The language used to format text for use on the web is called HTML (HyperText 
Markup Language). It is beyond the scope of this documentation to teach HTML and
the associated technologies but there are many books and websites on the topic which are 
readily available if you are interested. However, it is fairly obvious that 
writing a large website by hand using HTML is quite a laborious task. There are 
tools such as `Adobe Dreamweaver <http://www.adobe.com/products/dreamweaver.html>`_ \
which facilitate this task. But what happens if you want to change the layout of 
the pages across the site, e.g. to insert a new menu at the top? And what 
happens if you want functionality on your site rather than just content, for 
example a weather widget on the home page which shows the latest weather? We can 
get around the problem of supporting functionality by using programming 
languages on the server to perform tasks which result in the output of HTML. For 
example, the weather widget could be explained by the following workflow:

.. todo::
  Weather widget

The actual technologies available on the servers will vary from server to 
server. For example, a web server will support one or more programming languages
to allow you to implement this sort of functionality. A web server will often 
also have access to one or more *database management systems* allowing it to 
store and retrieve the information typically required to build a web page. For 
example, on a news based website, when you visit a web page, code on the web 
server will retrieve news items from a database and use them to construct the 
page you finally see.

Instant Indicia uses a language on the server called PHP, chosen because it is 
very widely available on web servers, probably more so than any other language. 
Likewise it requires a database management system called MySQL which is also 
found on more web servers than any other database technology. This means that 
Instant Indicia can be installed on many low cost shared web accounts. Finally, 
during the tutorials you will come across something called PhpMyAdmin. This is a
web application that lets you perform management tasks on MySQL databases on a 
web server such as creating databases and tables or backing up the data. It is 
also provided on many web servers that run MySQL.

However, all these technologies put together help, but it is still an enormous 
task to write the code required to build a large site. We still haven’t solved 
the problem of making site-wide changes such as the addition of a new menu 
either, though we could probably work out how to do that using some code. Worst
of all, the task of editing a web page requires someone with programming skills 
which is clearly not practical. The solution to all these problems is to use 
something called a *content management system*. A content management system is a
special type of program that provides the functionality required for a website, 
but without any content pages. It does, however, provide simple ways of adding 
new pages and content. Rather than writing code for each web page, you select 
from a menu an option along the lines of *Add new page* then simply type your
content in, rather like when typing a document in a Word processor. A content 
management system typically also provides the following useful functionality:

* Ability to install plugin modules to extend the functionality. For example 
  rather than writing a weather widget or code for a site search, just install 
  the module and it is done.
* Ability to let users register, login and to control the functionality that is 
  available to them. For example, only site editors are allowed to add or edit 
  pages.

Clearly then, writing a website using a content management system is vastly 
preferable to without once you have more than a couple of pages with limited 
functionality.

There are a huge number of content management systems available which are based 
on a variety of different technologies and costing anything from free to 
hundreds of thousands of pounds. Fortunately there are excellent free ones which
are capable of running very extensive and powerful websites. The benefits of 
integrating Indicia into a content management system are clear – the CMS can
provide content editing and other miscellaneous functionality, whereas Indicia 
can provide online recording, reporting and other specialist facilities. For 
Indicia, we have elected to focus our efforts on a single CMS called Drupal for
the following reasons:

.. todo::
 
  Drupal choice image

It is possible to use Indicia alongside many of the other available content 
management systems though of course it will be a bit more work, since the 
Indicia project has included development of extensive integration with Drupal in
order to make setting up online recording sites using Drupal as simple as 
possible.