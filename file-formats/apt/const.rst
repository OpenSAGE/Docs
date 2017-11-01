CONST
=====

Description
-----------

This file contains constant integer and string values that are used for the GUI (e.g Font Colors etc.)

Specification
-------------

The endianness is always LE in this format.

Header
~~~~~~

The header has a fixed size of 32 bytes, following the table below:

======  =====  =======  ===========
Offset  Bytes  Type     Name
======  =====  =======  ===========
0       17     CHAR[]   FourCC
17      3      UINT32   Unused
20      4      UINT32   AptOffset
24      4      UINT32   ItemCount
28      4      UINT32   Unused
======  =====  =======  ===========

* **FourCC**: This string is equal to "Apt constant file" for a valid `.apt` file.
* **AptOffset**: The offset to the first entry in the `.apt` file.
* **ItemCount**: The number of constant entries in this `.const` file.

List of items
~~~~~~~~~~~~~

After the header there follow the items, with their size specified by **ItemCount**. An item
looks like the specification in the table below:

======  =====  =======  ===========
Offset  Bytes  Type     Name
======  =====  =======  ===========
0       4      UINT32   Type
4       4      UINT32   Value
======  =====  =======  ===========

* **Type**: there are two types of entries:
    - STRING = 1
    - NUMBER = 4
* **Value**: depending on the type of this entry the value is interpreted differently. In case of a number the value is just used directly. For strings the value specifies an offset within the to a null terminated String.
