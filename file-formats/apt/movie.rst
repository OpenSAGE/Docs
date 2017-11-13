MOVIE
=====

Description
-----------

The movie character inside the APT is the root element that contains all
other characters. Every `.apt` file contains exactly one movie character as
it's first character.
The movie character is the only character that can contain other characters,
in a characterlist. Apart from that it also contains a list of frames, that
define the behaviour of the GUI at runtime. The only other character that can 
also contain frames is the :doc:`sprite <sprite>`. The import and export section 
is used to export and import characters from other movies (for that matter from
different `.apt` files). This allows to reuse common GUI elements for different
APTs.

Specification
-------------

The movie character is derived from the base character class, which is documented
:doc:`here <characters>`. Apart from the base data it contains the following fields:

======  =====  =======  ===========
Offset  Bytes  Type     Name
======  =====  =======  ===========
0       4      UINT32   FRAMECOUNT
4       4      UINT32   FRAMEOFFSET
8       4      UINT32   UNKNOWN
12      4      UINT32   CHARACTERCOUNT
16      4      UINT32   CHARACTEROFFSET
20      4      UINT32   SCREENWIDTH
24      4      UINT32   SCREENHEIGHT
28      4      UINT32   UNKNOWN
32      4      UINT32   IMPORTCOUNT
36      4      UINT32   IMPORTOFFSET
40      4      UINT32   EXPORTCOUNT
44      4      UINT32   EXPORTOFFSET
======  =====  =======  ===========

The count fields always specify how many elements are at the respective offset
(0 being the beginning of the `.apt` file).
