SAV
===

Description
-----------

``.sav`` files, in the format described here, were used in C&C Generals and Zero Hour, and maybe later games too (to be investigated).

These files appear to store the complete state of the game engine at the time that the game was saved. This provides a valuable glimpse into the inner
workings of the game engine.

Thanks to [Omniblade](https://github.com/omniblade)
for his help in figuring out this format.

Common Types
------------

Strings in `.sav` files are one of two types:

* ASCII string: prefixed by the number of characters as a byte
* Unicode string: prefixed by the number of characters as a byte. Since each character is 2 bytes, the actual number of bytes to read is double this number

File Structure
-------------

A ``.sav`` file is a chunk-based binary format. The overall structure is:

* Variable number of `CHUNK`s
* "SG_EOF" as an ASCII string (prefixed by length as a byte)

A chunk has this structure:

============  ===============
Type          Name
============  ===============
ASCII String  Chunk name
UINT32        Chunk size
BYTE          Version
============  ===============

The following are the chunks I have reverse-engineered so far.

CHUNK_GameState
~~~~~~~~~~~~~~~

=====   ============  =============
Bytes   Type          Name
=====   ============  =============
4       GAME_TYPE     Game type
n       ASCII String  Map path
2       UINT16        Year
2       UINT16        Month
2       UINT16        Day
2       UINT16        Day of week (Sunday = 0)
2       UINT16        Hour
2       UINT16        Minute
2       UINT16        Second
2       UINT16        Millisecond
n       ASCII String  Display name
n       ASCII String  Map file name
4       UINT32        Mission index
=====   ============  =============

CHUNK_Campaign
~~~~~~~~~~~~~~

=====   ============  =============
Bytes   Type          Name
=====   ============  =============
n       ASCII String  Side
n       ASCII String  Mission name
4       UINT32        ?
4       UINT32        ? maybe difficulty
=====   ============  =============

If chunk version >= 5

=====   ============  =============
Bytes   Type          Name
=====   ============  =============
5       ?             ?
=====   ============  =============

CHUNK_GameStateMap
~~~~~~~~~~~~~~~~~~

=====   ============  =============
Bytes   Type          Name
=====   ============  =============
n       ASCII String  Map path 1
n       ASCII String  Map path 2
4       UINT32        ? seems to be either 0 or 2
4       UINT32        length in bytes of map data
n       MAP           Byte-for-byte copy of current .map file
TODO
=====   ============  =============

CHUNK_TerrainLogic
~~~~~~~~~~~~~~~~~~

TODO

CHUNK_TeamFactory
~~~~~~~~~~~~~~~~~

TODO

CHUNK_Players
~~~~~~~~~~~~~

TODO

CHUNK_GameLogic
~~~~~~~~~~~~~~~

TODO

CHUNK_ParticleSystem
~~~~~~~~~~~~~~~~~~~~

TODO

CHUNK_Radar
~~~~~~~~~~~

TODO

CHUNK_ScriptEngine
~~~~~~~~~~~~~~~~~~

TODO

CHUNK_SidesList
~~~~~~~~~~~~~~~

TODO

CHUNK_TacticalView
~~~~~~~~~~~~~~~~~~

TODO

CHUNK_GameClient
~~~~~~~~~~~~~~~~

TODO

CHUNK_InGameUI
~~~~~~~~~~~~~~

TODO

CHUNK_Partition
~~~~~~~~~~~~~~~

TODO

CHUNK_TerrainVisual
~~~~~~~~~~~~~~~~~~~

TODO

CHUNK_GhostObject
~~~~~~~~~~~~~~~~~

TODO

Enumerations
------------

GAME_TYPE
~~~~~~~~~~~~~

=====  ==================
Value  Name
=====  ==================
0      Skirmish
1      SinglePlayer
=====  ==================