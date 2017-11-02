W3D
===

Description
-----------

``.w3d`` files were used in the following games to store 3D models:

* C&C Generals
* Zero Hour
* BFME I
* BFME II

Later SAGE games (C&C3 and RA3) used an XML-based evolution of the w3d format called w3x.

References
----------

* `w3d2ply/w3d_file.h <https://github.com/mikolalysenko/w3d2ply/blob/ecd8302b6cfd0578ab249cb95c8b70636c4609bc/w3d_file.h>`_
* `libw3d/types.hpp <https://github.com/feliwir/libw3d/blob/fb547b28c91f17070d65ba24edf7a5294a0554d9/include/libw3d/types.hpp>`_

Specification
-------------

Chunks
~~~~~~

``.w3d`` files can contain mesh data, animation data, bone transforms for placing meshes into a hierarchy and doing skinned animations, mesh bounding box information, and even particle emitter definitions. Skinned models are usually split over several files, with each animation chunk placed in a separate file. There is no technical need for this; it was presumably done for maintainability and / or runtime memory efficiency.

``.w3d`` is a chunk-based binary format. A ``.w3d`` file can (and usually does) contain multiple chunks. Every chunk starts with a chunk header:

======  =====  =======  ===========
Offset  Bytes  Type     Name
======  =====  =======  ===========
0       4      UINT32   ChunkType
4       4      UINT32   ChunkSize
======  =====  =======  ===========

* **ChunkType**: A value from the `W3D_CHUNK_TYPE`_ enumeration.
* **ChunkSize**: MSB is 1 if the chunk contains sub-chunks, otherwise it's 0. Lower 31 bits are the chunk size, not including this chunk header.

Some chunks, depending on the value of **ChunkType** in the chunk header, have sub-chunks. If there are sub-chunks, then the Most Significant Bit (MSB) of the **ChunkSize** field will be ``1``.

The following chunk type enumeration is not as complete as the references linked to above. This enumeration intentionally includes only the chunks that are actually used in SAGE games. Other chunks were used in previous W3D-based games, such as C&C Renegade. Note that the indentation denotes the chunk hierarchy, with nested chunk types appearing as sub-chunks of the parent chunk.

W3D_CHUNK_TYPE
~~~~~~~~~~~~~~

==========  ==========================
Value       Name
==========  ==========================
0x0         `W3D_CHUNK_MESH`_
0x2         `W3D_CHUNK_VERTICES`_
0x3         `W3D_CHUNK_VERTEX_NORMALS`_
0xC         `W3D_CHUNK_MESH_USER_TEXT`_
0xE         `W3D_CHUNK_VERTEX_INFLUENCES`_
0x1F        `W3D_CHUNK_MESH_HEADER3`_
0x20        `W3D_CHUNK_TRIANGLES`_
0x22        `W3D_CHUNK_VERTEX_SHADE_INDICES`_
0x28        `W3D_CHUNK_MATERIAL_INFO`_
0x29        `W3D_CHUNK_SHADERS`_
0x2A        `W3D_CHUNK_VERTEX_MATERIALS`_
0x2B        `W3D_CHUNK_VERTEX_MATERIAL`_
0x2C        `W3D_CHUNK_VERTEX_MATERIAL_NAME`_
0x2D        `W3D_CHUNK_VERTEX_MATERIAL_INFO`_
0x2E        `W3D_CHUNK_VERTEX_MAPPER_ARGS0`_
0x2F        `W3D_CHUNK_VERTEX_MAPPER_ARGS1`_
0x30        `W3D_CHUNK_TEXTURES`_
0x31        `W3D_CHUNK_TEXTURE`_
0x32        `W3D_CHUNK_TEXTURE_NAME`_
0x33        `W3D_CHUNK_TEXTURE_INFO`_
0x38        `W3D_CHUNK_MATERIAL_PASS`_
0x39        `W3D_CHUNK_VERTEX_MATERIAL_IDS`_
0x3A        `W3D_CHUNK_SHADER_IDS`_
0x3B        `W3D_CHUNK_DCG`_
0x3C        `W3D_CHUNK_DIG`_
0x3E        `W3D_CHUNK_SCG`_
0x3F        `W3D_CHUNK_SHADER_MATERIAL_ID`_
0x48        `W3D_CHUNK_TEXTURE_STAGE`_
0x49        `W3D_CHUNK_TEXTURE_IDS`_
0x4A        `W3D_CHUNK_STAGE_TEXCOORDS`_
0x4B        `W3D_CHUNK_PER_FACE_TEXCOORD_IDS`_
0x50        `W3D_CHUNK_SHADER_MATERIALS`_
0x51        `W3D_CHUNK_SHADER_MATERIAL`_
0x52        `W3D_CHUNK_SHADER_MATERIAL_HEADER`_
0x53        `W3D_CHUNK_SHADER_MATERIAL_PROPERTY`_
0x60        `W3D_CHUNK_TANGENTS`_
0x61        `W3D_CHUNK_BITANGENTS`_
0x80        `W3D_CHUNK_PS2_SHADERS`_
0x90        `W3D_CHUNK_AABTREE`_
0x91        `W3D_CHUNK_AABTREE_HEADER`_
0x92        `W3D_CHUNK_AABTREE_POLYINDICES`_
0x93        `W3D_CHUNK_AABTREE_NODES`_
0x100       `W3D_CHUNK_HIERARCHY`_
0x101       `W3D_CHUNK_HIERARCHY_HEADER`_
0x102       `W3D_CHUNK_PIVOTS`_
0x103       `W3D_CHUNK_PIVOT_FIXUPS`_
0x200       `W3D_CHUNK_ANIMATION`_
0x201       `W3D_CHUNK_ANIMATION_HEADER`_
0x202       `W3D_CHUNK_ANIMATION_CHANNEL`_
0x203       `W3D_CHUNK_BIT_CHANNEL`_
0x280       `W3D_CHUNK_COMPRESSED_ANIMATION`_
0x281       `W3D_CHUNK_COMPRESSED_ANIMATION_HEADER`_
0x282       `W3D_CHUNK_COMPRESSED_ANIMATION_CHANNEL`_
0x283       `W3D_CHUNK_COMPRESSED_BIT_CHANNEL`_
0x284       `W3D_CHUNK_COMPRESSED_ANIMATION_MOTION_CHANNEL`_
0x500       `W3D_CHUNK_EMITTER`_
0x501       `W3D_CHUNK_EMITTER_HEADER`_
0x502       `W3D_CHUNK_EMITTER_USER_DATA`_
0x503       `W3D_CHUNK_EMITTER_INFO`_
0x504       `W3D_CHUNK_EMITTER_INFOV2`_
0x505       `W3D_CHUNK_EMITTER_PROPS`_
0x509       `W3D_CHUNK_EMITTER_LINE_PROPERTIES`_
0x510       `W3D_CHUNK_EMITTER_ROTATION_KEYFRAMES`_
0x511       `W3D_CHUNK_EMITTER_FRAME_KEYFRAMES`_
0x512       `W3D_CHUNK_EMITTER_BLUR_TIME_KEYFRAMES`_
0x700       `W3D_CHUNK_HLOD`_
0x701       `W3D_CHUNK_HLOD_HEADER`_
0x702       `W3D_CHUNK_HLOD_LOD_ARRAY`_
0x703       `W3D_CHUNK_HLOD_SUB_OBJECT_ARRAY_HEADER`_
0x704       `W3D_CHUNK_HLOD_SUB_OBJECT`_
0x705       `W3D_CHUNK_HLOD_AGGREGATE_ARRAY`_
0x706       `W3D_CHUNK_HLOD_PROXY_ARRAY`_
0x740       `W3D_CHUNK_BOX`_
0xC00       `W3D_CHUNK_VERTICES_2`_
0xC01       `W3D_CHUNK_VERTEX_NORMALS_2`_
==========  ==========================

Chunks and sub-chunks appear in ``.w3d`` files in the following hierarchy:

* `W3D_CHUNK_MESH`_
  
  * `W3D_CHUNK_VERTICES`_
  * `W3D_CHUNK_VERTICES_2`_
  * `W3D_CHUNK_VERTEX_NORMALS`_
  * `W3D_CHUNK_VERTEX_NORMALS_2`_
  * `W3D_CHUNK_MESH_USER_TEXT`_
  * `W3D_CHUNK_VERTEX_INFLUENCES`_
  * `W3D_CHUNK_MESH_HEADER3`_
  * `W3D_CHUNK_TRIANGLES`_
  * `W3D_CHUNK_VERTEX_SHADE_INDICES`_
  * `W3D_CHUNK_MATERIAL_INFO`_
  * `W3D_CHUNK_SHADERS`_
  * `W3D_CHUNK_VERTEX_MATERIALS`_

    * `W3D_CHUNK_VERTEX_MATERIAL`_

      * `W3D_CHUNK_VERTEX_MATERIAL_NAME`_
      * `W3D_CHUNK_VERTEX_MATERIAL_INFO`_
      * `W3D_CHUNK_VERTEX_MAPPER_ARGS0`_
      * `W3D_CHUNK_VERTEX_MAPPER_ARGS1`_

  * `W3D_CHUNK_TEXTURES`_

    * `W3D_CHUNK_TEXTURE`_

      * `W3D_CHUNK_TEXTURE_NAME`_
      * `W3D_CHUNK_TEXTURE_INFO`_

  * `W3D_CHUNK_MATERIAL_PASS`_

    * `W3D_CHUNK_VERTEX_MATERIAL_IDS`_
    * `W3D_CHUNK_SHADER_IDS`_
    * `W3D_CHUNK_DCG`_
    * `W3D_CHUNK_DIG`_
    * `W3D_CHUNK_SCG`_
    * `W3D_CHUNK_SHADER_MATERIAL_ID`_
    * `W3D_CHUNK_TEXTURE_STAGE`_

      * `W3D_CHUNK_TEXTURE_IDS`_
      * `W3D_CHUNK_STAGE_TEXCOORDS`_
      * `W3D_CHUNK_PER_FACE_TEXCOORD_IDS`_

  * `W3D_CHUNK_SHADER_MATERIALS`_

    * `W3D_CHUNK_SHADER_MATERIAL`_

      * `W3D_CHUNK_SHADER_MATERIAL_HEADER`_
      * `W3D_CHUNK_SHADER_MATERIAL_PROPERTY`_

  * `W3D_CHUNK_TANGENTS`_
  * `W3D_CHUNK_BITANGENTS`_
  * `W3D_CHUNK_PS2_SHADERS`_
  * `W3D_CHUNK_AABTREE`_

    * `W3D_CHUNK_AABTREE_HEADER`_
    * `W3D_CHUNK_AABTREE_POLYINDICES`_
    * `W3D_CHUNK_AABTREE_NODES`_

  * `W3D_CHUNK_HIERARCHY`_
    
    * `W3D_CHUNK_HIERARCHY_HEADER`_
    * `W3D_CHUNK_PIVOTS`_
    * `W3D_CHUNK_PIVOT_FIXUPS`_

  * `W3D_CHUNK_ANIMATION`_

    * `W3D_CHUNK_ANIMATION_HEADER`_
    * `W3D_CHUNK_ANIMATION_CHANNEL`_
    * `W3D_CHUNK_BIT_CHANNEL`_

  * `W3D_CHUNK_COMPRESSED_ANIMATION`_

    * `W3D_CHUNK_COMPRESSED_ANIMATION_HEADER`_
    * `W3D_CHUNK_COMPRESSED_ANIMATION_CHANNEL`_
    * `W3D_CHUNK_COMPRESSED_BIT_CHANNEL`_
    * `W3D_CHUNK_COMPRESSED_ANIMATION_MOTION_CHANNEL`_

  * `W3D_CHUNK_EMITTER`_

    * `W3D_CHUNK_EMITTER_HEADER`_
    * `W3D_CHUNK_EMITTER_USER_DATA`_
    * `W3D_CHUNK_EMITTER_INFO`_
    * `W3D_CHUNK_EMITTER_INFOV2`_
    * `W3D_CHUNK_EMITTER_PROPS`_
    * `W3D_CHUNK_EMITTER_LINE_PROPERTIES`_
    * `W3D_CHUNK_EMITTER_ROTATION_KEYFRAMES`_
    * `W3D_CHUNK_EMITTER_FRAME_KEYFRAMES`_
    * `W3D_CHUNK_EMITTER_BLUR_TIME_KEYFRAMES`_

  * `W3D_CHUNK_HLOD`_

    * `W3D_CHUNK_HLOD_HEADER`_
    * `W3D_CHUNK_HLOD_LOD_ARRAY`_

      * `W3D_CHUNK_HLOD_SUB_OBJECT_ARRAY_HEADER`_
      * `W3D_CHUNK_HLOD_SUB_OBJECT`_

    * `W3D_CHUNK_HLOD_AGGREGATE_ARRAY`_
    * `W3D_CHUNK_HLOD_PROXY_ARRAY`_

  * `W3D_CHUNK_BOX`_

W3D_CHUNK_MESH
~~~~~~~~~~~~~~

This is the root mesh definition chunk. It is a container chunk that can contain these sub-chunks:

* W3D_CHUNK_MESH_HEADER3
* W3D_CHUNK_VERTICES
* W3D_CHUNK_VERTEX_NORMALS
* W3D_CHUNK_MESH_USER_TEXT
* W3D_CHUNK_VERTEX_INFLUENCES
* W3D_CHUNK_TRIANGLES
* W3D_CHUNK_VERTEX_SHADE_INDICES
* W3D_CHUNK_MATERIAL_INFO
* W3D_CHUNK_SHADERS
* W3D_CHUNK_VERTEX_MATERIALS
* W3D_CHUNK_TEXTURES
* W3D_CHUNK_MATERIAL_PASS
* W3D_CHUNK_TANGENTS
* W3D_CHUNK_BITANGENTS
* W3D_CHUNK_SHADER_MATERIALS
* W3D_CHUNK_PS2_SHADERS
* W3D_CHUNK_AABTREE
* W3D_CHUNK_VERTICES_2
* W3D_CHUNK_VERTEX_NORMALS_2

W3D_CHUNK_MESH_HEADER3
~~~~~~~~~~~~~~~~~~~~~~

The mesh header contains general info about the mesh.

======  =====  ===========  ====================
Offset  Bytes  Type         Name
======  =====  ===========  ====================
0       4      UINT32       Version
4       4      UINT32       MeshFlags
8       16     CHAR[16]     MeshName
24      16     CHAR[16]     ContainerName
40      4      UINT32       NumTriangles
44      4      UINT32       NumVertices
48      4      UINT32       NumMaterials
52      4      UINT32       NumDamageStages
56      4      UINT32       SortLevel
60      4      UINT32       PrelitVersion
64      4      UINT32       FutureCounts
68      4      UINT32       VertexChannels
72      4      UINT32       FaceChannels
76      4      W3D_VECTOR3  BoundingBoxMin
88      4      W3D_VECTOR3  BoundingBoxMax
100     4      W3D_VECTOR3  BoundingSphereCenter
112     4      FLOAT32      BoundingSphereRadius
======  =====  ===========  ====================

* **MeshFlags**: bitwise-or'd collection of `W3D_MESH_FLAGS`_ values.
* **VertexChannels**: bitwise-or'd collection of `W3D_VERTEX_CHANNELS`_ values.
* **FaceChannels**: bitwise-or'd collection of `W3D_FACE_CHANNELS`_ values.

W3D_VERTEX_CHANNELS
~~~~~~~~~~~~~~~~~~~

==========  ==========================  ==============
Value       Name                        Description
==========  ==========================  ==============
0x1         Location                    Object-space location of the vertex
0x2         Normal                      Object-space normal for the vertex
0x4         TexCoord                    Texture coordinate
0x8         Color                       Vertex color
0x10        BoneId                      Per-vertex bone id for skins
==========  ==========================  ==============

W3D_FACE_CHANNELS
~~~~~~~~~~~~~~~~~

==========  ==========================  ==============
Value       Name                        Description
==========  ==========================  ==============
0x1         Face                        Basic face info, W3dTriStruct...
==========  ==========================  ==============

W3D_CHUNK_VERTICES
~~~~~~~~~~~~~~~~~~

Array of vertices.

======  ======  =============  ====================
Offset  Bytes   Type           Name
======  ======  =============  ====================
0       12 * N  W3D_VECTOR3[]  Vertices
======  ======  =============  ====================

``N`` is the number of vertices specified in the `W3D_CHUNK_MESH_HEADER3`_ chunk.

W3D_CHUNK_VERTEX_NORMALS
~~~~~~~~~~~~~~~~~~~~~~~~

Array of normals.

======  ======  =============  ====================
Offset  Bytes   Type           Name
======  ======  =============  ====================
0       12 * N  W3D_VECTOR3[]  Normals
======  ======  =============  ====================

``N`` is the number of vertices specified in the `W3D_CHUNK_MESH_HEADER3`_ chunk.

W3D_CHUNK_MESH_USER_TEXT
~~~~~~~~~~~~~~~~~~~~~~~~

Text from the MAX comment field (null-terminated string).

======  ==========  ======================  ====================
Offset  Bytes       Type                    Name
======  ==========  ======================  ====================
0       ChunkSize   CHAR[]                  UserText
======  ==========  ======================  ====================

W3D_CHUNK_VERTEX_INFLUENCES
~~~~~~~~~~~~~~~~~~~~~~~~~~~

Mesh deformation vertex connections.

======  ======  ======================  ====================
Offset  Bytes   Type                    Name
======  ======  ======================  ====================
0       8 * N   W3D_VERTEX_INFLUENCE[]  VertexInfluences
======  ======  ======================  ====================

``N`` is the number of vertices specified in the `W3D_CHUNK_MESH_HEADER3`_ chunk.

W3D_VERTEX_INFLUENCE
~~~~~~~~~~~~~~~~~~~~

======  ======  ============  ====================
Offset  Bytes   Type          Name
======  ======  ============  ====================
0       2       UINT16        BoneIndex
2       6       UINT8[]       [Padding]
======  ======  ============  ====================

**TODO**: Does BFME have a second bone index, and bone weights?

W3D_CHUNK_TRIANGLES
~~~~~~~~~~~~~~~~~~~

======  ======  ============  ====================
Offset  Bytes   Type          Name
======  ======  ============  ====================
0       32 * N  W3D_TRIANGLE  Triangles
======  ======  ============  ====================

``N`` is the number of triangles specified in the `W3D_CHUNK_MESH_HEADER3`_ chunk.

W3D_TRIANGLE
~~~~~~~~~~~~

======  =====  ===========  ====================
Offset  Bytes  Type         Name
======  =====  ===========  ====================
0       4      UINT32       VertexIndex0
4       4      UINT32       VertexIndex1
8       4      UINT32       VertexIndex2
12      4      UINT32       SurfaceType
16      12     W3D_VECTOR3  Normal
28      4      FLOAT32      Distance
======  =====  ===========  ====================

* **SurfaceType**: A value from the `W3D_SURFACE_TYPE`_ enumeration.

W3D_SURFACE_TYPE
~~~~~~~~~~~~~~~~

==========  ==========================
Value       Name
==========  ==========================
0           LightMetal
1           HeavyMetal
2           Water
3           Sand
4           Dirt
5           Mud
6           Grass
7           Wood
8           Concrete
9           Flesh
10          Rock
11          Snow
12          Ice
13          Default
14          Glass
15          Cloth
16          TiberiumField
17          FoliagePermeable
18          GlassPermeable
19          IcePermeable
20          ClothPermeable
21          Electrical
22          Flammable
23          Steam
24          EletricalPermeable
25          FlammablePermeable
26          SteamPermeable
27          WaterPermeable
28          TiberiumWater
29          TiberiumWaterPermeable
30          UnderwaterDirt
31          UnderwaterTiberiumDirt
==========  ==========================

W3D_CHUNK_VERTEX_SHADE_INDICES
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Shade indexes for each vertex.

The meaning of "shade indexes" is unknown.

======  =====  ===========  ====================
Offset  Bytes  Type         Name
======  =====  ===========  ====================
0       4 * N  UINT32       ShadeIndices
======  =====  ===========  ====================

``N`` is the number of vertices specified in the `W3D_CHUNK_MESH_HEADER3`_ chunk.

W3D_CHUNK_MATERIAL_INFO
~~~~~~~~~~~~~~~~~~~~~~~

Declares the number of material passes, vertex materials, shaders, and textures that will be found in subsequent chunks.

======  =====  ===========  ====================
Offset  Bytes  Type         Name
======  =====  ===========  ====================
0       4      UINT32       PassCount
4       4      UINT32       VertexMaterialCount
8       4      UINT32       ShaderCount
12      4      UINT32       TextureCount
======  =====  ===========  ====================

* **PassCount**: How many material passes this render object uses
* **VertexMaterialCount**: How many vertex materials are used
* **ShaderCount**: How many shaders are used
* **TextureCount**: How many textures are used

W3D_CHUNK_SHADERS
~~~~~~~~~~~

Container chunk for an array of `W3D_SHADER`_ structures.
The number of shaders is contained in the **ShaderCount** field in the `W3D_CHUNK_MATERIAL_INFO`_ chunk.

W3D_SHADER
~~~~~~~~~~

======  =====  ===========  ====================
Offset  Bytes  Type         Name
======  =====  ===========  ====================
0       1      UINT8        DepthCompare
1       1      UINT8        DepthMask
2       1      UINT8        ColorMask
3       1      UINT8        DestBlend
4       1      UINT8        FogFunc
5       1      UINT8        PrimaryGradient
6       1      UINT8        SecondaryGradient
7       1      UINT8        SrcBlend
8       1      UINT8        Texturing
9       1      UINT8        DetailColorFunc
10      1      UINT8        DetailAlphaFunc
11      1      UINT8        ShaderPreset
12      1      UINT8        AlphaTest
13      1      UINT8        PostDetailColorFunc
14      1      UINT8        PostDetailAlphaFunc
15      1      UINT8        [Padding]
======  =====  ===========  ====================

* **DepthCompare**: A value from the `W3D_SHADER_DEPTH_COMPARE`_ enumeration.
* **DepthMask**: A value from the `W3D_SHADER_DEPTH_MASK`_ enumeration.
* **ColorMask**: Now obsolete and ignored.
* **DestBlend**: A value from the `W3D_SHADER_DEST_BLEND_FUNC`_ enumeration.
* **FogFunc**: Now obsolete and ignored.
* **PrimaryGradient**: A value from the `W3D_SHADER_PRIMARY_GRADIENT`_ enumeration.
* **SecondaryGradient**: A value from the `W3D_SHADER_SECONDARY_GRADIENT`_ enumeration.
* **SrcBlend**: A value from the `W3D_SHADER_SRC_BLEND_FUNC`_ enumeration.
* **Texturing**: A value from the `W3D_SHADER_TEXTURING`_ enumeration.
* **DetailColorFunc**: A value from the `W3D_SHADER_DETAIL_COLOR_FUNC`_ enumeration.
* **DetailAlphaFunc**: A value from the `W3D_SHADER_DETAIL_ALPHA_FUNC`_ enumeration.
* **ShaderPreset**: Now obsolete and ignored.
* **AlphaTest**: A value from the `W3D_SHADER_ALPHA_TEST`_ enumeration.
* **PostDetailColorFunc**: A value from the `W3D_SHADER_DETAIL_COLOR_FUNC`_ enumeration.
* **PostDetailAlphaFunc**: A value from the `W3D_SHADER_DETAIL_ALPHA_FUNC`_ enumeration.

W3D_SHADER_DEPTH_COMPARE
~~~~~~~~~~~~~~~~~~~~~~~~

==========  ==========================  ==============
Value       Name                        Description
==========  ==========================  ==============
0           PassNever                   Pass never (i.e. always fail depth comparison test)
1           PassLess                    Pass if incoming less than stored
2           PassEqual                   Pass if incoming equal to stored
3           PassLEqual                  Pass if incoming less than or equal to stored (default)
4           PassGreater                 Pass if incoming greater than stored
5           PassNotEqual                Pass if incoming not equal to stored
6           PassGEqual                  Pass if incoming greater than or equal to stored
7           PassAlways                  Pass always
==========  ==========================  ==============

W3D_SHADER_DEPTH_MASK
~~~~~~~~~~~~~~~~~~~~~

==========  ==========================  ==============
Value       Name                        Description
==========  ==========================  ==============
0           WriteDisable                Disable depth buffer writes 
1           WriteEnable                 Enable depth buffer writes (default)
==========  ==========================  ==============

W3D_SHADER_DEST_BLEND_FUNC
~~~~~~~~~~~~~~~~~~~~~~~~~~

==========  ==========================  ==============
Value       Name                        Description
==========  ==========================  ==============
0           Zero                        Destination pixel doesn't affect blending (default)
1           One                         Destination pixel added unmodified
2           SrcColor                    Destination pixel multiplied by fragment RGB components
3           OneMinusSrcColor            Destination pixel multiplied by one minus (i.e. inverse) fragment RGB components
4           SrcAlpha                    Destination pixel multiplied by fragment alpha component
5           OneMinusSrcAlpha            Destination pixel multiplied by fragment inverse alpha
6           SrcColorPreFog              Destination pixel multiplied by fragment RGB components prior to fogging
==========  ==========================  ==============

W3D_SHADER_PRIMARY_GRADIENT
~~~~~~~~~~~~~~~~~~~~~~~~~~~

==========  ==========================  ==============
Value       Name                        Description
==========  ==========================  ==============
0           Disable                     Disable primary gradient (same as OpenGL 'decal' texture blend)
1           Modulate                    Modulate fragment ARGB by gradient ARGB (default)
2           Add                         Add gradient RGB to fragment RGB, copy gradient A to fragment A
3           BumpEnvMap                  Environment-mapped bump mapping
==========  ==========================  ==============

W3D_SHADER_SECONDARY_GRADIENT
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

==========  ==========================  ==============
Value       Name                        Description
==========  ==========================  ==============
0           Disable                     Don't draw secondary gradient (default)
1           Enable                      Add secondary gradient RGB to fragment RGB 
==========  ==========================  ==============

W3D_SHADER_SRC_BLEND_FUNC
~~~~~~~~~~~~~~~~~~~~~~~~~

==========  ==========================  ==============
Value       Name                        Description
==========  ==========================  ==============
0           Zero                        Fragment not added to color buffer
1           One                         Fragment added unmodified to color buffer (default)
2           SrcAlpha                    Fragment RGB components multiplied by fragment A
3           OneMinusSrcAlpha            Fragment RGB components multiplied by fragment inverse (one minus) A
==========  ==========================  ==============

W3D_SHADER_TEXTURING
~~~~~~~~~~~~~~~~~~~~

==========  ==========================  ==============
Value       Name                        Description
==========  ==========================  ==============
0           Disable                     No texturing (treat fragment initial color as 1,1,1,1) (default)
1           Enable                      Enable texturing
==========  ==========================  ==============

W3D_SHADER_DETAIL_COLOR_FUNC
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

==========  ==========================  ==============
Value       Name                        Description
==========  ==========================  ==============
0           Disable                     Local (default)
1           Detail                      Other
2           Scale                       Local * Other
3           InvScale                    ~(~Local * ~Other) = Local + (1-Local)*Other
4           Add                         Local + Other
5           Sub                         Local - Other
6           SubR                        Other - Local
7           Blend                       (LocalAlpha)*Local + (~LocalAlpha)*Other
8           DetailBlend                 (OtherAlpha)*Local + (~OtherAlpha)*Other
==========  ==========================  ==============

W3D_SHADER_DETAIL_ALPHA_FUNC
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

==========  ==========================  ==============
Value       Name                        Description
==========  ==========================  ==============
0           Disable                     Local (default)
1           Detail                      Other
2           Scale                       Local * Other
3           InvScale                    ~(~Local * ~Other) = Local + (1-Local)*Other
==========  ==========================  ==============

W3D_SHADER_ALPHA_TEST
~~~~~~~~~~~~~~~~~~~~~

==========  ==========================  ==============
Value       Name                        Description
==========  ==========================  ==============
0           Disable                     Disable alpha testing (default)
1           Enable                      Enable alpha testing
==========  ==========================  ==============

W3D_CHUNK_VERTEX_MATERIALS
~~~~~~~~~~~~~~~~~~~~~~~~~~

Wraps the vertex materials.

W3D_CHUNK_VERTEX_MATERIAL
~~~~~~~~~~~~~~~~~~~~~~~~~

Vertex material wrapper.

W3D_CHUNK_VERTEX_MATERIAL_NAME
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Vertex material name (null-terminated string)

W3D_CHUNK_VERTEX_MATERIAL_INFO
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Vertex material info.

W3D_CHUNK_VERTEX_MAPPER_ARGS0
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Arguments for the first stage mapping (null-terminated, line-break-separated string).

W3D_CHUNK_VERTEX_MAPPER_ARGS1
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Arguments for the second stage mapping (null-terminated, line-break-separated string).

W3D_CHUNK_TEXTURES
~~~~~~~~~~~~~~~~~~

Wraps all of the texture info.

W3D_CHUNK_TEXTURE
~~~~~~~~~~~~~~~~~

Wraps a texture definition.

W3D_CHUNK_TEXTURE_NAME
~~~~~~~~~~~~~~~~~~~~~~

Texture filename (null-terminated string).

W3D_CHUNK_TEXTURE_INFO
~~~~~~~~~~~~~~~~~~~~~~

Optional texture info.

W3D_CHUNK_MATERIAL_PASS
~~~~~~~~~~~~~~~~~~~~~~~

Wraps the information for a single material pass.

W3D_CHUNK_VERTEX_MATERIAL_IDS
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Single or per-vertex array of UINT32 vertex material indices.

W3D_CHUNK_SHADER_IDS
~~~~~~~~~~~~~~~~~~~~

Single or per-triangle array of UINT32 shader indices.

W3D_CHUNK_DCG
~~~~~~~~~~~~~

Per-vertex diffuse color values.

W3D_CHUNK_DIG
~~~~~~~~~~~~~

Per-vertex diffuse illumination values.

W3D_CHUNK_SCG
~~~~~~~~~~~~~

Per-vertex specular color values.

W3D_CHUNK_SHADER_MATERIAL_ID
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Index into the array of shader materials in the `W3D_SHADER_MATERIALS`_ chunk.

W3D_CHUNK_TEXTURE_STAGE
~~~~~~~~~~~~~~~~~~~~~~~

Wrapper around a texture stage.

W3D_CHUNK_TEXTURE_IDS
~~~~~~~~~~~~~~~~~~~~~

Single or per-triangle array of UINT32 texture indices.

W3D_CHUNK_STAGE_TEXCOORDS
~~~~~~~~~~~~~~~~~~~~~~~~~

Per-vertex texture coordinates.

W3D_CHUNK_PER_FACE_TEXCOORD_IDS
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Indices to `W3D_CHUNK_STAGE_TEXCOORDS`_.

W3D_CHUNK_SHADER_MATERIALS
~~~~~~~~~~~~~~~~~~~~~~~~~~

Wrapper around an array of shader materials.

W3D_CHUNK_SHADER_MATERIAL
~~~~~~~~~~~~~~~~~~~~~~~~~

Stores material properties for use in conjunction with a specific pixel shader.

W3D_CHUNK_SHADER_MATERIAL_HEADER
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Stores the shader filename.

W3D_CHUNK_SHADER_MATERIAL_PROPERTY
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

A single shader material property with a type and value.

W3D_CHUNK_TANGENTS
~~~~~~~~~~~~~~~~~~

Array of tangent vectors.

W3D_CHUNK_BITANGENTS
~~~~~~~~~~~~~~~~~~~~

Array of bitangent vectors.

W3D_CHUNK_PS2_SHADERS
~~~~~~~~~~~~~~~~~~~~~

Shader info specific to the PlayStation 2.

W3D_CHUNK_AABTREE
~~~~~~~~~~~~~~~~~

Axis-Aligned Box Tree for hierarchical polygon culling

W3D_CHUNK_AABTREE_HEADER
~~~~~~~~~~~~~~~~~~~~~~~~

Catalog of the contents of the AABTree.

W3D_CHUNK_AABTREE_POLYINDICES
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Array of UINT32 polygon indices with count=mesh.PolyCount.

W3D_CHUNK_AABTREE_NODES
~~~~~~~~~~~~~~~~~~~~~~~

Array of W3dMeshAABTreeNode's with count=aabheader.NodeCount.

W3D_CHUNK_HIERARCHY
~~~~~~~~~~~~~~~~~~~

Hierarchy tree definition.

W3D_CHUNK_HIERARCHY_HEADER
~~~~~~~~~~~~~~~~~~~~~~~~~~

W3D_CHUNK_PIVOTS
~~~~~~~~~~~~~~~~

W3D_CHUNK_PIVOT_FIXUPS
~~~~~~~~~~~~~~~~~~~~~~

Only needed by the exporter. Doesn't seem to be used at runtime.

W3D_CHUNK_ANIMATION
~~~~~~~~~~~~~~~~~~~

Hierarchy animation data.

W3D_CHUNK_ANIMATION_HEADER
~~~~~~~~~~~~~~~~~~~~~~~~~~

W3D_CHUNK_ANIMATION_CHANNEL
~~~~~~~~~~~~~~~~~~~~~~~~~~~

Channel of vectors.

W3D_CHUNK_BIT_CHANNEL
~~~~~~~~~~~~~~~~~~~~~

Channel of boolean values (e.g. visibility).

W3D_CHUNK_COMPRESSED_ANIMATION
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Compressed hierarchy animation data.

W3D_CHUNK_COMPRESSED_ANIMATION_HEADER
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Describes playback rate, number of frames, and type of compression.

W3D_CHUNK_COMPRESSED_ANIMATION_CHANNEL
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Compressed channel, format dependent on type of compression.

W3D_CHUNK_COMPRESSED_BIT_CHANNEL
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Compressed bit stream channel, format dependent on type of compression.

W3D_CHUNK_COMPRESSED_ANIMATION_MOTION_CHANNEL
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Added in BFME II.

W3D_CHUNK_EMITTER
~~~~~~~~~~~~~~~~~

Description of a particle emitter.

W3D_CHUNK_EMITTER_HEADER
~~~~~~~~~~~~~~~~~~~~~~~~

General information such as name and version.

W3D_CHUNK_EMITTER_USER_DATA
~~~~~~~~~~~~~~~~~~~~~~~~~~~

User-defined data that specific loaders can switch on.

W3D_CHUNK_EMITTER_INFO
~~~~~~~~~~~~~~~~~~~~~~

Generic particle emitter definition.

W3D_CHUNK_EMITTER_INFOV2
~~~~~~~~~~~~~~~~~~~~~~~~

General particle emitter definition (version 2.0).

W3D_CHUNK_EMITTER_PROPS
~~~~~~~~~~~~~~~~~~~~~~~

Key-frameable properties.

W3D_CHUNK_EMITTER_LINE_PROPERTIES
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Line properties, used by line rendering mode.

W3D_CHUNK_EMITTER_ROTATION_KEYFRAMES
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Rotation keys for the particles.

W3D_CHUNK_EMITTER_FRAME_KEYFRAMES
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Frame keys (u-v based frame animation).

W3D_CHUNK_EMITTER_BLUR_TIME_KEYFRAMES
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Length of tail for line groups.

W3D_CHUNK_HLOD
~~~~~~~~~~~~~~

Description of an HLOD object.

W3D_CHUNK_HLOD_HEADER
~~~~~~~~~~~~~~~~~~~~~

General information such as name and version.

W3D_CHUNK_HLOD_LOD_ARRAY
~~~~~~~~~~~~~~~~~~~~~~~~

Wrapper around the array of objects for each level of detail.

W3D_CHUNK_HLOD_SUB_OBJECT_ARRAY_HEADER
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Info on the objects in this level of detail array.

W3D_CHUNK_HLOD_SUB_OBJECT
~~~~~~~~~~~~~~~~~~~~~~~~~

An object in this level of detail array.

W3D_CHUNK_HLOD_AGGREGATE_ARRAY
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Array of aggregates, contains W3D_CHUNK_SUB_OBJECT_ARRAY_HEADER and W3D_CHUNK_SUB_OBJECT_ARRAY.

W3D_CHUNK_HLOD_PROXY_ARRAY
~~~~~~~~~~~~~~~~~~~~~~~~~~

Array of proxies, used for application-defined purposes, provides a name and a bone.

W3D_CHUNK_BOX
~~~~~~~~~~~~~

Defines an collision box render object (W3dBoxStruct).

W3D_CHUNK_VERTICES_2
~~~~~~~~~~~~~~~~~~~~

Unknown purpose - added in BFME. Contained vertices are very similar to,
but not the same as, the vertices in W3D_CHUNK_VERTICES.

W3D_CHUNK_VERTEX_NORMALS_2
~~~~~~~~~~~~~~~~~~~

Unknown purpose - added in BFME. Contained normals are very similar to,
but not the same as, the normals in W3D_CHUNK_NORMALS.

Structures
----------

W3D_VECTOR3
~~~~~~~~~~~

======  =====  ===========  ====================
Offset  Bytes  Type         Name
======  =====  ===========  ====================
0       4      FLOAT32      X
4       4      FLOAT32      Y
8       4      FLOAT32      Z
======  =====  ===========  ====================

Enumerations
------------

W3D_MESH_FLAGS
~~~~~~~~~~~~~~

==========  ==========================  ==============
Value       Name                        Description
==========  ==========================  ==============
0x0         None                        Plain old normal mesh
0x1         CollisionBox                (obsolete as of 4.1) Mesh is a collision box (should be 8 verts, should be hidden, etc)
0x2         Skin                        (obsolete as of 4.1) Skin mesh 
0x4         Shadow                      (obsolete as of 4.1) Intended to be projected as a shadow
0x8         Aligned                     (obsolete as of 4.1) Always aligns with camera
0xFF0       CollisionTypeMask           Mask for the collision type bits
0x10        CollisionTypePhysical       Physical collisions
0x20        CollisionTypeProjectile     Projectiles (rays) collide with this
0x40        CollisionTypeVis            Vis rays collide with this mesh
0x80        CollisionTypeCamera         Camera rays/boxes collide with this mesh
0x100       CollisionTypeVehicle        Vehicles collide with this mesh (and with physical collision meshes)
0x1000      Hidden                      This mesh is hidden by default
0x2000      TwoSided                    Render both sides of this mesh
0x4000      ObsoleteLightMapped         Obsolete lightmapped mesh
0x8000      CastShadow                  This mesh casts shadows. Retained for backwards compatibility - use W3D_MESH_FLAG_PRELIT_* instead
0xFF0000    GeometryTypeMask            (introduced with 4.1)
0x0         GeometryTypeNormal          (4.1+) Normal mesh geometry
0x10000     GeometryTypeCameraAligned   (4.1+) camera aligned mesh
0x20000     GeometryTypeSkin            (4.1+) skin mesh
0x30000     ObsoleteGeometryTypeShadow  (4.1+) shadow mesh OBSOLETE!
0x40000     GeometryTypeAAbox           (4.1+) aabox OBSOLETE!
0x50000     GeometryTypeOBBox           (4.1+) obbox OBSOLETE!
0x60000     GeometryTypeCameraOriented  (4.1+) Camera oriented mesh (points *towards* camera)
0xF000000   PrelitMask                  (4.2+) 
0x1000000   PrelitUnlit                 Mesh contains an unlit material chunk wrapper
0x2000000   PrelitVertex                Mesh contains a precalculated vertex-lit material chunk wrapper
0x4000000   PrelitLightMapMultiPass     Mesh contains a precalculated multi-pass lightmapped material chunk wrapper
0x8000000   PrelitLightMapMultiTexture  Mesh contains a precalculated multi-texture lightmapped material chunk wrapper
0x10000000  Shatterable                 This mesh is shatterable
0x20000000  NPatchable                  It is okay to NPatch this mesh
==========  ==========================  ==============