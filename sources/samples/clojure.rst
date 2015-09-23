:title: Clojure Samples
:description: A brief description about sample_clojure
:keywords: Clojure, Leiningen, Language, version number, Notification alerts

.. _clojure:

clojure-samples
==================
 
This sample will help you get started with Shippable. The testing framework used here is
`test2junit <https://clojars.org/test2junit>`_ .

`Clojure Sample <https://github.com/shippableSamples/sample_clojure>`_

`Clojure Sample with MongoDB <https://github.com/shippableSamples/sample_clojure_mongodb>`_

`Clojure Sample with PostgreSQL <https://github.com/shippableSamples/sample_clojure_postgres>`_

Copy the test and code coverage output into the special folders Shippable/testresults and Shippable/codecoverage to get the reports parsed. The test report must be in the JUnit XML format and code coverage report in the Cobertura XML format.

We need the yml file to analyze your project details. So add the shippable.yml file to the root of your repo by specifying:

**language :** Clojure is used in this sample project

**lein :** lein2 is the version of Leinigen used in this sample project.


Here is the complete yml file for sample_clojure:

.. code-block:: bash

	# Language setting
	language: clojure

	lein:
	  - lein2

	# Create directories for test and coverage reports
	before_script:
	  - mkdir -p shippable/testresults
	  - mkdir -p shippable/codecoverage

	# Running the test with Leiningen
	script:
	  - cd sample
	  - lein test2junit

Enable the repo sample_clojure and run it using an Ubuntu minion. Once the build finishes execution, you can check for the console output, test and codecoverage results on the respective build's page.
