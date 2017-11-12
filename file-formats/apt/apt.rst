APT
===

Description
-----------

This file contains all logic of the GUI. For that purpose it contains 
so called characters which all have unique properties. As part of those characters
this file also includes Actionscript bytecode that is executed in a stack based
virtual machine.

Specification
-------------

Header 
~~~~~~

The apt file has no real header, but always starts with a magic string.

======  =====  =======  ===========
Offset  Bytes  Type     Name
======  =====  =======  ===========
0       8      CHAR[]   MAGIC
======  =====  =======  ===========

The MAGIC field is always equal to "Apt Data\\0" for a valid `.apt` file.

After the validity of the file has been confirmed the first character is loaded.
The offset for the first character inside the `.apt` is specified in the `.const`
file (see :doc:`here <const>` ). The main character of every `.apt` file is the 
Movie. :doc:`The process of loading the different characters is specified here
<characters>`.