BIG
===

Description
-----------

``.big`` files are an archive format that was used in many game titles created by EA Studios.

Specification
-------------

Header
~~~~~~

The header has a fixed size of 16 bytes, following the table below:

======  =====  ======  ===========  ==========
Offset  Bytes  Type    Name         Endianness
======  =====  ======  ===========  ==========
0       4      CHAR[]  FourCC       \-
4       4      UINT32  Size         LE
8       4      UINT32  NumEntries   BE
12      4      UINT32  OffsetFirst  BE
======  =====  ======  ===========  ==========

* **FourCC**: Identifies the string as valid big archive. The string may either be "BIG4" or "BIGF", depending on the version.
* **Size**: The entire size of a big archive. Size of a single archive can not be greater than 2^32 bytes
* **NumEntries**: Number of files that were packed into this archive
* **OffsetFirst**: The offset inside the file to the first entry

List of entries
~~~~~~~~~~~~~~~

After the header the follows a list with **NumEntries** elements, each entry looks the following:

======  =====  =======  ===========  ==========
Offset  Bytes  Type     Name         Endianness
======  =====  =======  ===========  ==========
0       4      UINT32   EntryOffset  BE
4       4      UINT32   EntrySize    BE
8       4      CSTRING  EntryName    \-
======  =====  =======  ===========  ==========

* **EntryOffset**: specified the start of this entry inside the file (in bytes)
* **EntrySize**: the size of the specified entry
* **EntryName**: the name of this entry, read as a nullterminated string. The maximum length is limited ny the Windows MAX_PATH (which is 260).


