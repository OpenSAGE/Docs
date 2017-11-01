WAK
===

Description
-----------

``.wak`` files were used in C&C Generals and Zero Hour to store ocean and pond wave definitions. In later SAGE games, this data became part of the ``.map`` format.

Not all maps have accompanying ``.wak`` files, but those that require wave movement do.

File Structure
-------------

The file has a variable number of entries. The number of entries is stored at the end of the file.

===============  ===============  ==========================  ===============
Offset           Bytes            Type                        Name
===============  ===============  ==========================  ===============
0                NumEntries * 20  `WAVE_ENTRY`_\[NumEntries]  Entries
NumEntries * 20  4                UINT32                      NumEntries
===============  ===============  ==========================  ===============

* **Entries**: See `WAVE_ENTRY`_ structure.
* **NumEntries**: Number of entries. Don't ask me why this is at the end of the file, not the beginning...
  Thankfully WAVE_ENTRY is a fixed-length structure.

WAVE_ENTRY
~~~~~~~~~~~~~~

======  =====   ============  =============
Offset  Bytes   Type          Name
======  =====   ============  =============
0       4       UINT32        StartX
4       4       UINT32        StartY
8       4       UINT32        EndX
12      4       UINT32        EndY
16      4       `WAVE_TYPE`_  WaveType
======  =====   ============  =============

* **StartX** and **StartY**: Specify the start position of the wave.
* **EndX** and **EndY**: Specify the end position of the wave.
* **WaveType**: Type of the wave. See `WAVE_TYPE`_ enumeration.

Enumerations
------------

WAVE_TYPE
~~~~~~~~~~~~~

=====  ==================
Value  Name
=====  ==================
0      Pond
1      Ocean
2      CloseOcean
3      CloseOceanDouble
4      Radial
=====  ==================