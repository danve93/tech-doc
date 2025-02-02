.. _add-appserver:

=======================
 Add an AppServer Node
=======================

This section explains how to add an AppServer Node to an existing
|product| installation. This is useful when the number of mailboxes
grows and you need to scale up your infrastructure.

Preliminaries
-------------

Before attempting to install a new AppServer, please read carefully
the whole procedure in this page and make sure the following
requirements are satisfied.

* A |product| infrastructure is already operating correctly

* A new node is available, which satisfies the :ref:`Multi Server
  Requirements <multi-server-req>` and on which the
  :ref:`multi-server-preliminary` have already been executed. We will
  call this node *SRV11.example.com*.

* All commands **must be executed** as the ``root`` user

The procedure to follow is straightforward and identical to the
:ref:`srv3-install` Node.

.. include:: /_includes/_multiserver-installation/srv3-ce.rst

Complete Installation
---------------------

The last task to carry out is the restart of all services

.. code:: console

   # su - zextras -c "zmcontrol restart"

To make sure that the Node was added correctly, use this command:

.. code:: console

   # su - zextras -c "carbonio prov gas mailbox"

The output should include the new node, for example::

   srv1.example.com
   srv2.example.com
   ...
   srv11.example.com

To check whether all the services are running, use command

.. code:: console

   # su - zextras -c "zmcontrol status"
