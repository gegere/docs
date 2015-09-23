:title: clojure
:description: How to configure shippable.yml file for clojure 
:keywords: yml,clojure,lein,lein2

.. _langclojure:


**clojure**
==========

This section helps you to create a shippable.yml file for your clojure project.


-  Set the appropriate language and the version number. Clojure minions use ``lein`` by default to set the runtime platform.
	.. code-block:: python
	
     		# language setting
              language: clojure
        	# lein tag
	      lein: lein2

- Use **install** key to install the required dependencies for your project.  
	.. code-block:: python

            install: lein protobuf install

 
- **Test scripts**:  Use **script** key in shippable.yml file to specify what command to run tests with. The default command to run leiningen test suite is **lein test2junit** 
       .. code-block:: python

            script: lein test2junit

**Build Examples**

Want to see build examples? Reference our :ref:`Clojure build samples <clojure>` .
 

