:orphan:

:is-up-to-date: True

.. _craftercms-plugin-yaml-file:

-----------------------------
Crafter CMS Plugin Descriptor
-----------------------------

The ``craftercms-plugin.yaml`` file contains information for use in Crafter CMS.  We'll take a look at a file used for
a blueprint and for a site plugin.

^^^^^^^^^^^^^^^^^^^^^^^^^
Blueprint Descriptor File
^^^^^^^^^^^^^^^^^^^^^^^^^

Here's a sample taken from the  ``craftercms-plugin.yaml`` for the Empty blueprint.

.. code-block:: yaml
    :caption: *craftercms-plugin.yaml file for the Empty blueprint*
    :linenos:

    # This file describes a plugin for use in Crafter CMS

    # The version of the format for this file
    descriptorVersion: 2

    # Describe the plugin
    plugin:
      type: blueprint
      id: org.craftercms.blueprint.empty
      name: Empty Blueprint
      tags:
        - blueprint
        - website
      version:
        major: 1
        minor: 0
        patch: 0
      description: |
        Simple empty blueprint
      website:
        name: Empty Blueprint
        url: https://craftercms.org
      media:
        screenshots:
          - title: Home Page
            description: Screenshot of the homepage
            url: /studio/static-assets/images/blueprints/empty/bp_empty.png
      developer:
        company:
          name: Crafter Software
          email: info@craftersoftware.com
          url: https://craftersoftware.com/
      build:
        id: c3d2a5444e6a24b5e0481d6ba87901d0b02716c8
        date: 2021-01-23T00:00:00Z
      license:
        name: MIT
        url: https://opensource.org/licenses/MIT
      crafterCmsVersions:
        - major: 4
          minor: 0
          patch: 0
      crafterCmsEditions:
        - community
        - enterprise
      searchEngine: Elasticsearch

where the following fields are required:

- ``descriptorVersion`` - The version of the format for this file which is currently 2
- ``plugin.type`` - ``blueprint`` or ``site`` depending on the type of plugin you're building
- ``plugin.id`` - a unique Id that is meaningful/recognizable to people who will be using the blueprint/plugin
- ``plugin.name`` - blueprint/plugin name (For our blueprint example, it is the blueprint name shown in the
  **Choose Blueprint** screen of **Create Site**)
- ``plugin.version`` - a version number for the blueprint/site plugin
- ``plugin.crafterCmsVersions`` - Crafter CMS versions that the plugin is compatible with (look in the :ref:`release-notes`
  section for the versions available), and you'll need to keep this up to date
- ``plugin.searchEngine`` - search engine your plugin requires, the only value possible at this time is``Elasticsearch``

|
|

For ``plugin.media.screenshots``, the url ``../.crafter/screenshots/default.png`` is the default path for Crafter CMS to look for a default representative image of a plugin or blueprint.  For more information on adding a default representative image for your blueprint see :ref:`adding-default-image-for-bp` and :ref:`site-plugins-create-your-plugin` for site plugins.

.. note::

  For the images to be used for the ``screenshots`` in the ``craftercms-plugin.yaml`` file, we recommend
  using images with approximately a ``4:3`` aspect ratio (width to height), such as an image sized at 1200x800

.. _site-plugin-descriptor-file:

^^^^^^^^^^^^^^^^^^^^^^^^^^^
Site Plugin Descriptor File
^^^^^^^^^^^^^^^^^^^^^^^^^^^

Crafter CMS supports automatically wiring your site plugin to the corresponding configuration file in Studio during your site plugin installation.

To setup a site plugin to be automatically wired in the corresponding configuration file in Studio (for example, a form control, will be wired to the Site Config Tools Configuration file) during the installation, add the following to your ``craftercms-plugin.yaml`` descriptor file

.. code-block:: yaml
   :linenos:
   :caption: *Setup auto-wiring to Studio in descriptor file*

   installation:
    - type: preview-app
      parent:
        id: craftercms.components.ToolsPanel
      element:
        name: widget
        children:
        - name: plugin
          attributes:
          - name: id
            value: org.craftercms.plugin.sidebar
          - name: type
            value: sidebar
          - name: name
            value: react-sample
          - name: file
            value: main.js

where:

- ``installation.type`` is the type of site plugin for auto-wiring in Studio.
  Available values are **form-control**, **form-datasource** and **preview-app**
- ``installation.parent.id`` contains the  Crafter CMS library to be used by the plugin, required when installation-type is *preview-app*
- ``installation.element.name`` is the element name to be wired in your site configuration file so the plugin will show up in Studio
  Available values are **control**, **datasource** and **widget**
- ``installation.element.children`` contains any number of **name** and **children**  describing your plugin, such  as the icon to be  used  by your plugin if  applicable, or the plugin location, where:
  - ``name`` is the name of what's being described, e.g. **plugin** or **icon**
  - ``children`` contains any number of **name** and **value** and can contain the class (icon), plugin id, plugin type, plugin name and plugin files/folders (plugin location) and its corresponding  values


Below is a sample ``craftercms-plugin.yaml`` for a form control plugin descriptor.

.. code-block:: yaml
   :caption: *Example craftercms-plugin.yaml file for a form-control site plugin*
   :linenos:
   :emphasize-lines: 44-62

   # This file describes a plugin for use in Crafter CMS

   # The version of the format for this file
   descriptorVersion: 2

   # Describe the plugin
   plugin:
     type: site
     id: org.craftercms.plugin.control
     name: My Form Control Site Plugin Example
     tags:
       - test
     version:
       major: 3
       minor: 0
       patch: 0
     description: My simple form control site plugin
     website:
       name: Site Plugin Example
       url: https://github.com/craftercms/site-plugins-example
     media:
       screenshots:
         - title: Crafter CMS
           description: Crafter CMS Example Plugin
           url: "https://raw.githubusercontent.com/craftercms/site-plugin-example/master/.crafter/logo.svg"
     developer:
       company:
         name: Crafter Software
         email: info@craftersoftware.com
         url: https://craftersoftware.com
     build:
       id: f9d09cbf39167609bcca4e31f5d2475d0ef14f8a
       date: 2021-05-21T00:00:00Z
     license:
       name: MIT
       url: https://opensource.org/licenses/MIT
     crafterCmsVersions:
       - major: 4
         minor: 0
         patch: 0
     crafterCmsEditions:
       - community
       - enterprise
     installation:
       - type: form-control
         element:
           name: control
           children:
             - name: plugin
               children:
                 - name: pluginId
                   value: org.craftercms.plugin.control
                 - name: type
                   value: control
                 - name: name
                   value: text-input
                 - name: filename
                   value: main.js
             - name: icon
               children:
                 - name: class
                   value: fa-pencil-square-o

|

See :ref:`studio-plugins` for examples of plugins auto-wired in Studio.

