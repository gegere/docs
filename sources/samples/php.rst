:titles: php samples
:description: A brief description about php sample
:keywords: php, Language,phpunit,script,Notification alerts


.. _php:

php-samples
===========

These samples will help you get started with Shippable. Test and Coverage tools used here are
`phpunit <http://phpunit.de/>`_.

`PHP Sample <https://github.com/shippableSamples/sample_php>`_

`PHP Sample with Memcached <https://github.com/shippableSamples/sample_php_memcached>`_

`PHP Sample with MongoDB <https://github.com/shippableSamples/sample_php_mongo>`_

`PHP Sample with MySQL <https://github.com/shippableSamples/sample_php_mysql>`_

`PHP Sample with Nginx <https://github.com/shippableSamples/sample_php_nginx>`_

`PHP Sample with Redis <https://github.com/shippableSamples/sample_php_redis>`_

We need the yml file to analyze the project details. So add the shippable.yml file to the root of your repo by specifying:

**language :** Specify the language used to create the project. php is used in this sample project.

**php :** Specify the runtime against which your build needs to run using **php** tag. The sample project uses “5.4".

**script :** Specify the command to run the test and coverage using script key and export the generated junit test output to shippable/testresults folder and xml coverage output to shippable/codecoverage foldet to get the visualization of reports.  

Note: For using the phpunit that is installed as a composer dependency, use a command like

.. code-block:: bash

	vendor/bin/phpunit --log-junit shippable/testresults/junit.xml --coverage-xml shippable/codecoverage tests/calculator_test.php


**notification alerts:** Email notifications are disabled in this sample project.

This is the complete yml file for sample_php project:

.. code-block:: bash

	language: php

	php: 
  	  - 5.4

        before_script:
          - mkdir -p shippable/codecoverage 
          - mkdir -p shippable/testresults

        script:
         - phpunit --log-junit shippable/testresults/junit.xml --coverage-xml shippable/codecoverage tests/calculator_test.php
          
        notifications:
          email: false


Create a project by enabling the repo sample_php and run it using an Ubuntu minion. Once the build finishes execution, you can check for the console output, test and code coverage output on the respective build’s tab.

