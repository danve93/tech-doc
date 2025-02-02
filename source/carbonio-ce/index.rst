.. SPDX-FileCopyrightText: 2022 Zextras <https://www.zextras.com/>
..
.. SPDX-License-Identifier: CC-BY-NC-SA-4.0

.. Zextras Carbonio documentation master file, created by
   sphinx-quickstart on Thu Aug 26 11:06:34 2021.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.

********************************
 Welcome to |ce| Documentation!
********************************

This is the documentation of |ce| edition, the free and open source
e-mail and collaboration software.

The content is organised in multiple parts:

* Release. Contains links to all changelogs of |product|'s components
  on GitHub repositories and directions to carry out the upgrade
* Install. Information on |product| and its architecture, installation
  and upgrade instructions, security tips
* Admin by GUI. |product| administration using the |adminui|
* Admin by CLI. Administration of |product| from the command line:
  management tasks and some advanced usage for |mesh|
* Use. everyday's usage of |product|, including its use on the
  dedicated mobile Apps
* Develop. Links to API reference

If you are looking for |carbonio|'s documentation, please refer to its
`dedicated page <../../carbonio/html/index.html>`_.

.. grid:: 1 2 2 3
   :gutter: 2

   .. grid-item-card::
      :columns: 12 12 6 6
      :class-header: sd-font-weight-bold sd-fs-5

      .. toctree::
         :maxdepth: 2

         basics/general

   .. grid-item-card::
      :columns: 12 12 6 6
      :class-header: sd-font-weight-bold sd-fs-5

      .. toctree::
         :maxdepth: 1

         release/toc

   .. grid-item-card::
      :columns: 12 12 6 6
      :class-header: sd-font-weight-bold sd-fs-5

      .. toctree::
         :maxdepth: 2

         install/toc

   .. grid-item-card::
      :columns:  12 12 6 6
      :class-header: sd-font-weight-bold sd-fs-5

      .. toctree::
         :maxdepth: 2
         :includehidden:

         /adminpanel/toc

   .. grid-item-card::
      :columns:  12 12 6 6
      :class-header: sd-font-weight-bold sd-fs-5

      .. toctree::
         :maxdepth: 2
         :includehidden:

         admincli/toc

   .. grid-item-card::
      :columns: 12 12 6 6
      :class-header: sd-font-weight-bold sd-fs-5

      .. toctree::
         :maxdepth: 2

         usage/toc


   .. grid-item-card::
      :columns: 12 12 6 6
      :class-header: sd-font-weight-bold sd-fs-5

      .. toctree::
         :maxdepth: 1

         develop/toc
