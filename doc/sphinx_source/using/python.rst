=======================
Using the Python Module
=======================

In Eggdrop 1.10.0, Eggdrop was shipped with a Python module that, similar to the existing core Tcl capability, allows Eggdrop to run python scripts.

-------------------
System Requirements
-------------------
Similar to Tcl requirements, Eggdrop requires both python and python development libraries to be installed on the host machine. On Debian/Ubuntu machines, this requires the packages python-dev AND python-is-python3 to be installed. The python-is-python3 updates symlinks on the host system that allow Eggdrop to find it.

--------------
Loading Python
--------------

Put this line into your Eggdrop configuration file to load the python module::

  loadmodule python

To load a python script from your config file, place the .py file in the scripts/ folder and add the following line to your config::

  pysource scripts/myscript.py

-----------------------
Eggdrop Python Commands
-----------------------

The Python module is built to use the existing core Tcl commands integrated into Eggdrop via the ``eggdrop.tcl`` module. To call an existing Tcl command from Python, you can either load the entire catalog by running ``import eggdrop.tcl``, or be more specific by ``from eggdrop.tcl import putserv, putlog, chanlist``, etc.

Arguments to the Tcl functions are automatically converted as follows:

* ``None`` is converted to an empty Tcl object (the empty string, ``""``)
* ``List`` and ``Tuple`` is converted to a ``Tcl list``
* ``Dict`` is converted to a ``Tcl dictionary``
* Everything else is converted to a string using the str() method

Return values from Tcl functions must be manually converted:

* ``""`` the empty string is automatically converted to None
* everything else is returned as string
* ``Tcl list`` as string can be converted to a Python ``List`` using ``parse_tcl_list``
* ``Tcl dictionary`` as string can be converted to a Python ``Dict`` using ``parse_tcl_list``

^^^^^^^^^^^^^^^^
bind <arguments>
^^^^^^^^^^^^^^^^

An important difference to note is that Eggdrop Python has its own ``bind`` command implemented. You will generally want to create binds using the Python ``bind`` command and not import bind from eggdrop.tcl because a Python bind will call a Python function, whereas using the Tcl bind will call a Tcl function (not one from the script you are writing).

The python version of the bind command is used to create a bind that triggers a python function. The python bind takes the same arguments as the Tcl binds, but here each argument is passed individually. For example, a bind that would look like ``bind pub * !foo myproc`` in Tcl is written as ``bind("pub", "*", "!foo", myproc)``. For more information on Eggsrop bind argument syntax please see :ref:`bind_types`. The eggdrop.tcl.bind command should not be used as it will attempt to call a Tcl proc.

^^^^^^^^^^^^^^^^^^^^^^^
parse_tcl_list <string>
^^^^^^^^^^^^^^^^^^^^^^^

When a python script calls a Tcl command that returns a list via the eggdrop.tcl module, the return value will be a Tcl-formatted list- also simply known as a string. The ``parse_tcl_list`` command will convert the Tcl-formatted list into a Python list, which can then freely be used within the Python script.

^^^^^^^^^^^^^^^^^^^^^^^
parse_tcl_dict <string>
^^^^^^^^^^^^^^^^^^^^^^^

When a python script calls a Tcl command that returns a dict via the eggdrop.tcl module, the return value will be a Tcl-formatted dict- also simply known as a string. The ``parse_tcl_dict`` command will convert the Tcl-formatted dict into a Python list, which can then freely be used within the Python script.

--------------------------------
Writing an Eggdrop Python script
--------------------------------

Some example scripts, complete with documentation, are included with the Python module that ships with Eggdrop (src/mod/python.mod/scripts). These scripts are included to help demonstrate script formatting and usage. The scripts are: 


.. glossary::

    bestfriend.py
      This example script demonstrates how to use the parse_tcl_list() python command to convert a list returned by a Tcl command into a list that is usable by Python.

    greet.py
      This is a very basic script that demonstrates how a Python script with binds can be run by Eggdrop.

    imdb.py
      This script shows how to use an existing third-party module to extend a Python script, in this case retrieving information from imdb.com.

    listtls.py
      This script demonstrates how to use parse-tcl_list() and parse_tcl_dict() to convert a list of dicts provided by Tcl into something that is usable by Python.

    urltitle.py
      This script shows how to use an existing third-party module to extend a Python script, in this case using an http parser to collect title information from a provided web page.
    

^^^^^^^^^^^^^^
Header section
^^^^^^^^^^^^^^

Python is able to call any Tcl command by importing the ``eggdrop`` module. For example, to use the ``putlog`` command in a python script, you would import it as::

  from eggdrop.tcl import putlog

and then call it using::

  putlog("This is a logged message")


An important difference to note is that Eggdrop Python has its own ``bind`` command implemented. You will generally want to create binds using the Python ``bind`` command and not import bind from eggdrop.tcl because a Python bind will call a Python function, whereas using the Tcl bind will call a Tcl function (not one from the script you are writing).

Where does python print go?

-------------------------------------
Writing a module for use with Eggdrop
-------------------------------------

This is how you import a module for use with an egg python script.

Copyright (C) 2000 - 2024 Eggheads Development Team

