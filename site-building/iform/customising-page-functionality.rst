Customising pages built using prebuilt forms in Drupal
------------------------------------------------------

There are many things you can do to customise the behaviour and display of a 
page built using the Indicia forms module's prebuilt form library, without 
having to write your own form code or to hack the existing forms. You can modify
the form's appearance, language used for the labels, functionality including
validation rules and even change the template used for the entire page. All 
these tasks are performed by creating files which are placed in the correct
location and which have a file name adhering to the correct pattern so that the
Indicia forms module can find and use them.

Add your own CSS stylesheets
^^^^^^^^^^^^^^^^^^^^^^^^^^^^

To include your own CSS files, you need to create a .css file and place it in 
the folder sites\all\modules\iform\client_helpers\prebuilt_forms\css. The file 
should be called node.nid.css where nid is the node id of the Drupal page your 
form is on.

In addition, developers of prebuilt forms can provide CSS that is added to the 
page for all instances of a particular form by creating a file called 
sites\all\modules\iform\client_helpers\prebuilt_forms\css\form-name.css, 
replacing form-name with the name of the form. For example you will find a file 
called verification_1.css in this location which is used for all instances of 
the verification_1.php form.

Adding your own JavaScript
^^^^^^^^^^^^^^^^^^^^^^^^^^

To include your own JavaScript files, you need to create a .js file and place 
it in the folder sites\all\modules\iform\client_helpers\prebuilt_forms\js. The 
file should be called node.nid.js where nid is the node id of the Drupal page 
your form is on.

Your JavaScript code can use the `jQuery <http://jquery.com>`_ library version 
1.3.2 which is linked in to all Indicia powered forms.

In addition, developers of prebuilt forms can provide JavaScript? that is added 
to the page for all instances of a particular form by creating a file called 
sites\all\modules\iform\client_helpers\prebuilt_forms\js\form-name.js, replacing
form-name with the name of the form. For example you will find a file called 
verification_1.js in this location which runs for all instances of the 
verification_1.php form.

If you need to interact with the map on your web page, there are 2 hooks you can
use. The first, mapSettingsHooks lets you alter the settings object before it is
used to setup the map. For example:

.. code-block:: php

  <?php
  mapSettingsHooks.push(function(opts) {
    // disable zooming on scroll wheel usage
    opts.scroll_wheel_zoom = false;
  }

Don't forget that there are normally other ways to provide these configurations,
for example on the dynamic form you could edit the form structure and provide 
some property overrides after the [map] element. The second hook is 
mapInitialisationHooks which is called once the map is setup, letting you make 
any post-setup changes or attach event handlers and so forth. For example:

.. code-block:: php

  <?php  
  mapInitialisationHooks.push(function(div) {
    div.map.events.register('zoomend', this, function (event) { 
      var currentZoom = map.getZoom(); 
      if (currentZoom > 5) { 
        alert('You are zoomed in');
      } 
    }); 
  }

Overridding the HTML templates used to output the input controls
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

The data_entry_helper declares a global array of templates called 
$indicia_templates. To change any of the template values for an instance of a 
form, you can create a folder called templates in 
sites\all\modules\iform\client_helpers\prebuilt_forms\. In this folder, create a
file called node.nid.php where nid is the node id of the form page. The file 
simply changes entries in the $indicia_templates array to suit. The following 
example shows template changes to remove the header above uploaded images as 
well as add some instructions to the upload button:

.. code-block:: php

  <?php
  global $indicia_templates;
  $indicia_templates['file_box_initial_file_info'] = 
      '<div id="{id}" class="ui-widget-content ui-corner-all photo">'.
      '<div class="progress"><div class="progress-bar" style="width: {imagewidth}px"></div>'.
      '<div class="progress-percent"></div></div><span class="photo-wrapper"></span></div>';
  $indicia_templates['file_box'] = 
    '<fieldset class="ui-corner-all">\n<legend>{caption}</legend>\n{uploadSelectBtn}&nbsp;'.
      '<span class="tip">'.
      'You may upload up to four images of each species (max size per image of 4mb)</span>'.
      '<div class="filelist"></div>{uploadStartBtn}</fieldset>';

Developers of prebuilt forms can also create a file in the templates folder 
called form-name.php where form-name is the name of the form without the .php 
extension. This provides a template override file which runs for all instances 
of a particular form.

Providing your own language files
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Language files for each prebuilt form are placed in the folder 
sites\all\modules\iform\client_helpers\prebuilt_forms\lang and are called 
form-name.lang.php where lang is the 2 character ISO language code matching the
declared code in Drupal.

In addition, a single form instance can either replace or change the language 
file for a form by declaring a file in the lang folder called node.nid.en.php 
where nid is the form page's node id. Provide a complete set of custom terms by 
using the global $custom_terms array:

.. code-block:: php

  <?php 
  global $custom_terms;

  $custom_terms = array(
          'Species' => 'Art',
          'Latin Name' => 'Latäineschen Numm',
          'Date' => 'Datum',
          'Spatial Ref' => 'Koordinaten'
  );

or override one or more terms leaving the rest intact by using the 
$custom_term_overrides array:

.. code-block:: php

  <?php
  global $custom_term_overrides;
  $custom_term_overrides[] = array(
    'LANG_Tab_place' => 'When and Where?',
  );

If you need to override language strings in a Drupal multisite setup then you 
can place this in the site specific version of the same folder. The site 
specific versions of language files take precedence over the all sites versions
and the node specific versions take precedence over those defined for a prebuilt
form.

Why don't we use Drupal i18n? A good question - Drupal has mechanisms for 
internationalisation which are mature and robust. We don't use them in Indicia
though, for 2 good reasons. Firstly, Indicia is not a Drupal specific project so
needs its own mechanisms for localisation. Secondly and more importantly, Drupal
allows you to localise into different languages but does not provide a mechanism
for overriding a string in the default language (other than hacking around with 
theme functions or template files etc). So, in the example above we change the
English place tab title, even though the form developer had already provided a
suitable string. Drupal does not do this.

Providing custom validation code
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

When the form submission has been built, ready to send to the warehouse, it is 
possible to run custom PHP to validation the form POST data and return an array
of errors. To do this, create a folder within your iform module 
iform\client_helpers\prebuilt_forms\validation. Inside this folder, create a 
file called validate.nid.php where the nid is replaced by your page's Drupal 
node ID. This file will be automatically loaded by the iform module at the 
appropriate point. Inside the PHP file, create a single function called 
iform_custom_validation which recieves a $post parameter containing form post 
array and returns an an associative array of control names with error messages. 
It can of course return an empty array if there are no problems found. Here's an
example:

.. code-block:: php

  <?php
  function iform_custom_validation($post) {
    $errors = array();
    if (substr($post['sample:entered_sref'], 0, 2)!=='SU')
      $errors['sample:entered_sref']=lang::get('This survey only accepts data in the SU grid square.');
    return $errors;
  }
