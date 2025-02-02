.. _adminpanel:

|adminui| Overview
==================

|adminui| is the component that allows access to the administration
functionalities of |carbonio| and is installed by default from
|product| 22.11.0 onwards. It is not available for previous versions,
but can be installed after upgrading to that version, see
:ref:`carbonio-upgrade`.

Like for every other component, it can be reached using a
:ref:`supported browser <browser_compatibility>` and point it to
https://acme.example:6071/login, replacing `acme.example` with
your domain.

To access the |adminui|, you need an account that is marked as
**Global Administrator**. The default Global Admin is
``zextras@acme.example``; at the first login it will be prompted to
change the password. This can be done from the CLI using the command
shown in Section :ref:`manage-admins`. When a password expires and the
Admin tries to login, a dialog will be show, which informs of the
expired password and allows to change it.

Once logged in, more Global Admins can be added from the |adminui|;
please refer to section :ref:`ap-new-admin` for directions.

|adminui| allows to manage the |product| domains, mailstores,
accounts, |cos|, and privacy settings. The overall organisation of the
panel is similar to the others components: a the *Top Bar* allows
quick creation of a new domain or COS by clicking the |create| button,
while navigation items are on the left-hand column.

The landing page is shown in :numref:`fig_ap-top` and
:numref:`fig_ap-bottom`.

.. grid:: 1 1 2 2
   :gutter: 3

   .. grid-item::
      :columns: 6
      
      .. _fig_ap-top:

      .. figure:: /img/adminpanel/AP-landing-top.png
	 :width: 100%

         The upper part of Admin Panel's landing page

   .. grid-item::
      :columns: 6

      In the upper part, clicking on either of the boxes will open the
      |adminui| page for the Accounts and mailing list, respectively.


.. grid:: 1 1 2 2
   :gutter: 3
                 
   .. grid-item::
      :columns: 6

      .. _fig_ap-bottom:

      .. figure:: /img/adminpanel/AP-landing-bottom.png
	 :width: 100%

         The lower part of Admin Panel's landing page

   .. grid-item::
      :columns: 6

      In the lower part are shown the versions of |carbonio| and of
      |carbonio| Core for all the servers defined within the
      |carbonio| infrastructure. The button `GO TO MAILSTORES SERVERS
      LIST` allows to open the :menuselection:`Storage --> Global
      Servers --> Server List` page.
