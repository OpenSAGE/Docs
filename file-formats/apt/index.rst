APT
===

Description
-----------

Apt files are flash based user interface files (based on the `SWF <https://www.mobilefish.com/download/flash/swf_file_format_spec_v9.pdf>`_ 
file format created by Macromedia). Other than the SWF format the contents are splitted into multiple files, 
which are all in a single `.big` archive. Much of the information presented here was gathered
from the apt2xml and xml2apt tools that are available for download `here <http://www.cncmods.net/downloads.php>`_.

Constant Data
-------------

The constant variables that are used for the GUI are all stored in a ``.const`` file. :doc:`The specification of that 
format is described here <const>`.

Apt Data
--------

All parts of the GUI logic and display system are stored inside the ``.apt`` file. Before you are able to load the ``.apt`` file
you are required to load the ``.const`` file to get the entry offset. :doc:`The specification of that format can be found here <apt>`.