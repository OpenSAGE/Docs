CHARACTERS
==========

Description
-----------

Characters are the elements that make up the logic of the `.apt` file. There are 
several types that each fulfill a specific purpose. The endianness is always LE 
in this format.

Specification
-------------

All of the characters derive from one base class. This part is used to retrieve
the character type and validate that this a correct character. 

Base data
~~~~~~~~~

======  =====  =======  ===========
Offset  Bytes  Type     Name
======  =====  =======  ===========
0       4      UINT32   TYPE
4       4      UINT32   SIGNATURE
======  =====  =======  ===========

The type defines which type of character this is and how the data after the base
data is processed. The signature is always equal to `0x09876543`.

Character types
~~~~~~~~~~~~~~~

======  =========  ===========
TYPE    Character  Description
======  =========  ===========
1       Shape      Contains geometry and display data
2       Text       Displays a string
3       Font       Contains glyph data for embedded fonts
4       Button     Defines a region and contains Actionscript 
                   for the related buttons events
5       Sprite     Another container, just like Movie
6       Sound      Defines a sound. Not used in APT 
7       Image      Defines an image that can be used from Shapes. References
                   the `.dat` file.
8       Morph      A morph animation. Not used in APT 
9       Movie      The root character that contains all other characters.
======  =========  ===========

Display System
~~~~~~~~~~~~~~

For displaying actual data the `.apt` format uses a display list. Some character
types can be placed on that list by either frame items or actionscript.
