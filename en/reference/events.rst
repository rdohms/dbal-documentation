Events
======

Both ``Doctrine\DBAL\DriverManager`` and
``Doctrine\DBAL\Connection`` accept an instance of
``Doctrine\Common\EventManager``. The EventManager has a couple of
events inside the DBAL layer that are triggered for the user to
listen to.

PostConnect Event
-----------------

``Doctrine\DBAL\Events::postConnect`` is triggered right after the
connection to the database is established. It allows to specify any
relevant connection specific options and gives access to the
``Doctrine\DBAL\Connection`` instance that is responsible for the
connection management via an instance of
``Doctrine\DBAL\Event\ConnectionEventArgs`` event arguments
instance.

Doctrine is already shipped with two implementations for the
"PostConnect" event:


-  ``Doctrine\DBAL\Event\Listeners\OracleSessionInit`` allows to
   specify any number of Oracle Session related enviroment variables
   that are set right after the connection is established.
-  ``Doctrine\DBAL\Event\Listeners\MysqlSessionInit`` allows to
   specify the Charset and Collation of the Client Connection if these
   options are not configured correctly on the MySQL server side.

You can register events by subscribing them to the ``EventManager``
instance passed to the Connection factory:

.. code-block:: php

    <?php
    $evm = new EventManager();
    $evm->addEventSubscriber(new MysqlSessionInit('UTF8'));
    
    $conn = DriverManager::getConnection($connectionParams, null, $evm);


