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

* **ChunkType**: See `W3D_CHUNK_TYPE`_ enumeration.
* **ChunkSize**: MSB is 1 if the chunk contains sub-chunks, otherwise it's 0. Lower 31 bits are the chunk size, not including this chunk header.

Some chunks, depending on the value of **ChunkType** in the chunk header, have sub-chunks. If there are sub-chunks, then the Most Significant Bit (MSB) of the **ChunkSize** field will be ``1``.

The following chunk type enumeration is not as complete as the references linked to above. This enumeration intentionally includes only the chunks that are actually used in SAGE games. Other chunks were used in previous W3D-based games, such as C&C Renegade. Note that the indentation denotes the chunk hierarchy, with nested chunk types appearing as sub-chunks of the parent chunk.

W3D_CHUNK_TYPE
~~~~~~~~~~~~~~

TODO

.. code-block:: c

   enum W3dChunkType
   {
       // Mesh definition (W3dMesh)
       W3D_CHUNK_MESH = 0x0,
   
           // Array of vertices (W3dVector3[])
           W3D_CHUNK_VERTICES = 0x2,
   
           // Array of normals (W3dVector3[])
           W3D_CHUNK_VERTEX_NORMALS = 0x3,
   
           // Text from the MAX comment field (null-terminated string)
           W3D_CHUNK_MESH_USER_TEXT = 0xC,
   
           // Mesh Deformation vertex connections (W3dVertexInfluence[])
           W3D_CHUNK_VERTEX_INFLUENCES = 0xE,
   
           // Mesh header contains general info about the mesh (W3dMeshHeader3)
           W3D_CHUNK_MESH_HEADER3 = 0x1F,
   
           // New improved triangles chunk (W3dTriangle[])
           W3D_CHUNK_TRIANGLES = 0x20,
   
           // Shade indexes for each vertex (uint32[])
           W3D_CHUNK_VERTEX_SHADE_INDICES = 0x22,
   
           // Materials information, pass count, etc (W3dMaterialInfo)
           W3D_CHUNK_MATERIAL_INFO = 0x28,
   
           // Shaders (W3dShader[])
           W3D_CHUNK_SHADERS = 0x29,
   
           // Wraps the vertex materials
           W3D_CHUNK_VERTEX_MATERIALS = 0x2A,
   
               // Vertex material wrapper
               W3D_CHUNK_VERTEX_MATERIAL = 0x2B,
   
                   // Vertex material name (null-terminated string)
                   W3D_CHUNK_VERTEX_MATERIAL_NAME = 0x2C,
   
                   // Vertex material info (W3dVertexMaterialInfo)
                   W3D_CHUNK_VERTEX_MATERIAL_INFO = 0x2D,
   
                   // Arguments for the first stage mapping (null-terminated,    line-break-separated string)
                   W3D_CHUNK_VERTEX_MAPPER_ARGS0 = 0x2E,
   
                   // Arguments for the second stage mapping (null-terminated,    line-break-separated string)
                   W3D_CHUNK_VERTEX_MAPPER_ARGS1 = 0x2F,
   
           // Wraps all of the texture info
           W3D_CHUNK_TEXTURES = 0x30,
   
               // Wraps a texture definition
               W3D_CHUNK_TEXTURE = 0x31,
   
                   // Texture filename (null-terminated string)
                   W3D_CHUNK_TEXTURE_NAME = 0x32,
   
                   // Optional texture info (W3dTextureInfo)
                   W3D_CHUNK_TEXTURE_INFO = 0x33,
   
           // Wraps the information for a single material pass
           W3D_CHUNK_MATERIAL_PASS = 0x38,
   
               // Single or per-vertex array of uint32 vertex material indices    (check chunk size)
               W3D_CHUNK_VERTEX_MATERIAL_IDS = 0x39,
   
               // Single or per-triangle array of uint32 shader indices (check    chunk size)
               W3D_CHUNK_SHADER_IDS = 0x3A,
   
               // Per-vertex diffuse color values (W3dRgba[])
               W3D_CHUNK_DCG = 0x3B,
   
               // Per-vertex diffuse illumination values (W3dRgb[])
               W3D_CHUNK_DIG = 0x3C,
   
               // Per-vertex specular color values (W3dRgb[])
               W3D_CHUNK_SCG = 0x3E,
   
               // Index into array of shader materials in the    W3D_CHUNK_SHADER_MATERIALS chunk
               W3D_CHUNK_SHADER_MATERIAL_ID = 0x3F,
   
               // Wrapper around a texture stage.
               W3D_CHUNK_TEXTURE_STAGE = 0x48,
   
                   // Single or per-tri array of uint32 texture indices (check    chunk size)
                   W3D_CHUNK_TEXTURE_IDS = 0x49,
   
                   // Per-vertex texture coordinates (W3dTexCoord[])
                   W3D_CHUNK_STAGE_TEXCOORDS = 0x0000004A,
   
                   // Indices to W3D_CHUNK_STAGE_TEXCOORDS, (W3dVector3[])
                   W3D_CHUNK_PER_FACE_TEXCOORD_IDS = 0x0000004B,
   
           // Wrapper around an array of shader materials
           W3D_CHUNK_SHADER_MATERIALS = 0x50,
   
               // Stores material properties for use in conjunction with    vertex / pixels shaders
               W3D_CHUNK_SHADER_MATERIAL = 0x51,
   
                   // Stores the shader filename
                   W3D_CHUNK_SHADER_MATERIAL_HEADER = 0x52,
   
                   // A single shader material property with type / value
                   W3D_CHUNK_SHADER_MATERIAL_PROPERTY = 0x53,
   
           // Array of tangents (W3dVector3[])
           W3D_CHUNK_TANGENTS = 0x60,
   
           // Array of bitangents (W3dVector3[])
           W3D_CHUNK_BITANGENTS = 0x61,
   
           // Shader info specific to the Playstation 2.
           W3D_CHUNK_PS2_SHADERS = 0x80,
   
           // Axis-Aligned Box Tree for hierarchical polygon culling
           W3D_CHUNK_AABTREE = 0x90,
   
               // Catalog of the contents of the AABTree
               W3D_CHUNK_AABTREE_HEADER = 0x91,
   
               // Array of uint32 polygon indices with count=mesh.PolyCount
               W3D_CHUNK_AABTREE_POLYINDICES = 0x92,
   
               // Array of W3dMeshAABTreeNode's with count=aabheader.NodeCount
               W3D_CHUNK_AABTREE_NODES = 0x93,
   
       // Hierarchy tree definition
       W3D_CHUNK_HIERARCHY = 0x100,
           W3D_CHUNK_HIERARCHY_HEADER = 0x101,
           W3D_CHUNK_PIVOTS = 0x102,
   
           // Only needed by the exporter. Doesn't seem to be used at runtime.
           W3D_CHUNK_PIVOT_FIXUPS = 0x103,
   
       // Hierarchy animation data
       W3D_CHUNK_ANIMATION = 0x200,
           W3D_CHUNK_ANIMATION_HEADER = 0x201,
   
           // Channel of vectors
           W3D_CHUNK_ANIMATION_CHANNEL = 0x202,
   
           // Channel of boolean values (e.g. visibility)
           W3D_CHUNK_BIT_CHANNEL = 0x203,
   
       // Compressed hierarchy animation data
       W3D_CHUNK_COMPRESSED_ANIMATION = 0x280,
   
           // Describes playback rate, number of frames, and type of    compression
           W3D_CHUNK_COMPRESSED_ANIMATION_HEADER = 0x281,
   
           // Compressed channel, format dependent on type of compression
           W3D_CHUNK_COMPRESSED_ANIMATION_CHANNEL = 0x282,
   
           // Compressed bit stream channel, format dependent on type of    compression
           W3D_CHUNK_COMPRESSED_BIT_CHANNEL = 0x283,
   
           // Added in BFME II
           W3D_CHUNK_COMPRESSED_ANIMATION_MOTION_CHANNEL = 0x284,
   
       // Description of a particle emitter
       W3D_CHUNK_EMITTER = 0x00000500,
   
           // General information such as name and version
           W3D_CHUNK_EMITTER_HEADER = 0x501,
   
           // User-defined data that specific loaders can switch on
           W3D_CHUNK_EMITTER_USER_DATA = 0x502,
   
           // Generic particle emitter definition
           W3D_CHUNK_EMITTER_INFO = 0x503,
   
           // Generic particle emitter definition (version 2.0)
           W3D_CHUNK_EMITTER_INFOV2 = 0x504,
   
           // Key-frameable properties
           W3D_CHUNK_EMITTER_PROPS = 0x505,
   
           // Line properties, used by line rendering mode
           W3D_CHUNK_EMITTER_LINE_PROPERTIES = 0x509,
   
           // Rotation keys for the particles
           W3D_CHUNK_EMITTER_ROTATION_KEYFRAMES = 0x510,
   
           // Frame keys (u-v based frame animation)
           W3D_CHUNK_EMITTER_FRAME_KEYFRAMES = 0x511,
   
           // Length of tail for line groups
           W3D_CHUNK_EMITTER_BLUR_TIME_KEYFRAMES = 0x512,
   
       // Description of an HLod object (see HLodClass)
       W3D_CHUNK_HLOD = 0x700,
   
           // General information such as name and version
           W3D_CHUNK_HLOD_HEADER = 0x701,
   
           // Wrapper around the array of objects for each level of detail
           W3D_CHUNK_HLOD_LOD_ARRAY = 0x702,
   
               // Info on the objects in this level of detail array
               W3D_CHUNK_HLOD_SUB_OBJECT_ARRAY_HEADER = 0x703,
   
               // An object in this level of detail array
               W3D_CHUNK_HLOD_SUB_OBJECT = 0x704,
   
           // Array of aggregates, contains W3D_CHUNK_SUB_OBJECT_ARRAY_HEADER    and W3D_CHUNK_SUB_OBJECT_ARRAY
           W3D_CHUNK_HLOD_AGGREGATE_ARRAY = 0x705,
   
           // Array of proxies, used for application-defined purposes,    provides a name and a bone.
           W3D_CHUNK_HLOD_PROXY_ARRAY = 0x706,
   
       // Defines an collision box render object (W3dBoxStruct)
       W3D_CHUNK_BOX = 0x740,
   
       // Unknown purpose - added in BFME
       W3D_CHUNK_VERTICES_2 = 0xC00,
   
       // Unknown purpose - added in BFME
       W3D_CHUNK_NORMALS_2 = 0xC01
   }

W3D_CHUNK_MESH
~~~~~~~~~~~~~~

A container chunk that can contain these sub-chunks:

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
* W3D_CHUNK_NORMALS_2

W3D_CHUNK_MESH_HEADER3
~~~~~~~~~~~~~~~~~~~~~~

Describes the mesh, including mesh name, number of vertices, bounding box and bounding sphere, etc.

.. code-block:: csharp

   struct W3dMeshHeader3
   {
       uint32 Version;
       W3dMeshFlags Attributes;
   
       char[16] MeshName;
       char[16] ContainerName;
   
       uint32 NumTriangles;
       uint32 NumVertices;
       uint32 NumMaterials;
       uint32 NumDamageStages;
       uint32 SortLevel;
       uint32 PrelitVersion;
       uint32 FutureCounts;
   
       W3dVertexChannels VertexChannels;
       W3dFaceChannels FaceChannels;
   
       W3dVector3 BoundingBoxMin;
       W3dVector3 BoundingBoxMax;
   
       W3dVector3 BoundingSphereCenter;
       float32 BoundingSphereRaduius;
   }
   
   [Flags]
   enum W3dMeshFlags : uint32
   {
       None = 0x00000000, // plain ole normal mesh
   
       CollisionBox = 0x00000001, // (obsolete as of 4.1) mesh is a collision    box (should be 8 verts, should be hidden, etc)
       Skin = 0x00000002, // (obsolete as of 4.1) skin mesh 
       Shadow = 0x00000004, // (obsolete as of 4.1) intended to be projected    as a shadow
       Aligned = 0x00000008, // (obsolete as of 4.1) always aligns with camera
   
       CollisionTypeMask = 0x00000FF0, // mask for the collision type bits
       CollisionTypeShift = 4, // shifting to get to the collision type bits
       CollisionTypePhysical = 0x00000010, // physical collisions
       CollisionTypeProjectile = 0x00000020, // projectiles (rays) collide    with this
       CollisionTypeVis = 0x00000040, // vis rays collide with this mesh
       CollisionTypeCamera = 0x00000080, // camera rays/boxes collide with    this mesh
       CollisionTypeVehicle = 0x00000100, // vehicles collide with this mesh    (and with physical collision meshes)
   
       Hidden = 0x00001000, // this mesh is hidden by default
       TwoSided = 0x00002000, // render both sides of this mesh
       ObsoleteLightMapped = 0x00004000, // obsolete lightmapped mesh
   
       // NOTE: retained for backwards compatibility - use    W3D_MESH_FLAG_PRELIT_* instead.
       CastShadow = 0x00008000, // this mesh casts shadows
   
       GeometryTypeMask = 0x00FF0000, // (introduced with 4.1)
       GeometryTypeNormal = 0x00000000, // (4.1+) normal mesh geometry
       GeometryTypeCameraAligned = 0x00010000, // (4.1+) camera aligned mesh
       GeometryTypeSkin = 0x00020000, // (4.1+) skin mesh
       ObsoleteGeometryTypeShadow = 0x00030000, // (4.1+) shadow mesh OBSOLETE!
       GeometryTypeAAbox = 0x00040000, // (4.1+) aabox OBSOLETE!
       GeometryTypeOBBox = 0x00050000, // (4.1+) obbox OBSOLETE!
       GeometryTypeCameraOriented = 0x00060000, // (4.1+) camera oriented mesh    (points _towards_ camera)
   
       PrelitMask = 0x0F000000, // (4.2+) 
       PrelitUnlit = 0x01000000, // mesh contains an unlit material chunk    wrapper
       PrelitVertex = 0x02000000, // mesh contains a precalculated vertex-lit    material chunk wrapper 
   
       PrelitLightMapMultiPass = 0x04000000, // mesh contains a precalculated    multi-pass lightmapped material chunk wrapper
   
       PrelitLightMapMultiTexture = 0x08000000, // mesh contains a    precalculated multi-texture lightmapped material chunk wrapper
   
       Shatterable = 0x10000000, // this mesh is shatterable.
       NPatchable = 0x20000000 // it is ok to NPatch this mesh
   }
   
   [Flags]
   enum W3dVertexChannels : uint32
   {
       // object-space location of the vertex
       Location = 0x1,
   
       // object-space normal for the vertex
       Normal = 0x2,
   
       // texture coordinate
       TexCoord = 0x4,
   
       // vertex color
       Color = 0x8,
   
       // per-vertex bone id for skins
       BoneId = 0x10
   }
   
   [Flags]
   enum W3dFaceChannels : uint32
   {
       // basic face info, W3dTriStruct...
       Face = 0x1
   }

W3D_CHUNK_VERTICES
~~~~~~~~~~~~~~~~~~

.. code-block:: csharp

   struct W3dMeshVertices
   {
       W3dVector3[N] Vertices;
   }

``N`` is the number of vertices specified in the `W3D_CHUNK_MESH_HEADER3`_ chunk.

W3D_CHUNK_VERTEX_NORMALS
~~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: csharp

   struct W3dMeshVertexNormals
   {
       W3dVector3[N] Normals;
   }

``N`` is the number of vertices specified in the `W3D_CHUNK_MESH_HEADER3`_ chunk.

W3D_CHUNK_MESH_USER_TEXT
~~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: csharp

   struct W3dMeshUserText
   {
       char[ChunkSize] UserText;
   }

W3D_CHUNK_VERTEX_INFLUENCES
~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: csharp

   struct W3dMeshVertexInfluences
   {
       W3dVertexInfluence[N] VertexInfluences;
   }
   
   struct W3dVertexInfluence
   {
       uint16 BoneIndex;
   
       // TODO: Does BFME have a second bone index, and bone weights?
   
       uint8[6] _Padding;
   }

``N`` is the number of vertices specified in the `W3D_CHUNK_MESH_HEADER3`_ chunk.

### Triangles chunk

**Chunk type: `W3D_CHUNK_TRIANGLES`**

``` csharp
struct W3dTriangles
{
    // NumTris is the number of triangles specified in the W3dMeshHeader3 chunk.
    W3dTriangle[NumTris] Triangles;
}

struct W3dTriangle
{
    uint32 VertexIndex0;
    uint32 VertexIndex1;
    uint32 VertexIndex2;

    W3dSurfaceType SurfaceType;

    // Plane normal and distance
    W3dVector3 Normal;
    float32 Distance;
}

enum W3dSurfaceType : uint32
{
    LightMetal = 0,
    HeavyMetal,
    Water,
    Sand,
    Dirt,
    Mud,
    Grass,
    Wood,
    Concrete,
    Flesh,
    Rock,
    Snow,
    Ice,
    Default,
    Glass,
    Cloth,
    TiberiumField,
    FoliagePermeable,
    GlassPermeable,
    IcePermeable,
    ClothPermeable,
    Electrical,
    Flammable,
    Steam,
    ElectricalPermeable,
    FlammablePermeable,
    SteamPermeable,
    WaterPermeable,
    TiberiumWater,
    TiberiumWaterPermeable,
    UnderwaterDirt,
    UnderwaterTiberiumDirt
}
```

### Mesh vertex shade indices chunk

**Chunk type: `W3D_CHUNK_VERTEX_SHADE_INDICES`**

The meaning of this chunk is unknown.

``` csharp
struct W3dMeshVertexShadeIndices
{
    uint32[N] ShadeIndices;
}
```

`N` is the number of vertices specified in the `W3dMeshHeader3` chunk.

### Mesh material info

**Chunk type: `W3D_CHUNK_MATERIAL_INFO`**

Declares the number of material passes, vertex materials, shaders, and textures that will be found in subsequent chunks.

``` csharp
struct W3dMaterialInfo
{
    // How many material passes this render object uses
    uint32 PassCount;

    // How many vertex materials are used
    uint32 VertexMaterialCount;

    // How many shaders are used
    uint32 ShaderCount;

    // How many textures are used
    uint32 TextureCount;
}
```

### Mesh shaders

**Chunk type: `W3D_SHADERS`**

Container chunk for an array of `W3D_SHADER` sub-chunks.
The number of shaders is contained in `W3dMaterialInfo.ShaderCount`.

### Mesh shader

**Chunk type: `W3D_SHADER`**

``` csharp
struct W3dShader
{
    W3dShaderDepthCompare DepthCompare;
    W3dShaderDepthMask DepthMask;

    // Now obsolete and ignored
    uint8 ColorMask;

    W3dShaderDestBlendFunc DestBlend;

    // Now obsolete and ignored
    uint8 FogFunc;

    W3dShaderPrimaryGradient PrimaryGradient;
    W3dShaderSecondaryGradient SecondaryGradent;
    W3dShaderSrcBlendFunc SrcBlend;
    W3dShaderTexturing Texturing;
    W3dShaderDetailColorFunc DetailColorFunc;
    W3dShaderDetailAlphaFunc DetailAlphaFunc;

    // Now obsolete and ignored
    uint8 ShaderPreset;

    W3dShaderAlphaTest AlphaTest;
    W3dShaderDetailColorFunc PostDetailColorFunc;
    W3dShaderDetailAlphaFunc PostDetailAlphaFunc;

    // Padding
    uint8 _Padding;
}

enum W3dShaderDepthCompare : uint8
{
    // pass never (i.e. always fail depth comparison test)
    PassNever = 0,

    // pass if incoming less than stored
    PassLess,

    // pass if incoming equal to stored
    PassEqual,

    // pass if incoming less than or equal to stored (default)
    PassLEqual,

    // pass if incoming greater than stored	
    PassGreater,

    // pass if incoming not equal to stored
    PassNotEqual,

    // pass if incoming greater than or equal to stored
    PassGEqual,

    // pass always
    PassAlways
}

enum W3dShaderDepthMask : uint8
{
    // disable depth buffer writes 
    WriteDisable = 0,

    // enable depth buffer writes (default)
    WriteEnable
}

enum W3dShaderDestBlendFunc : uint8
{
    // destination pixel doesn't affect blending (default)
    Zero = 0,

    // destination pixel added unmodified
    One,

    // destination pixel multiplied by fragment RGB components
    SrcColor,

    // destination pixel multiplied by one minus (i.e. inverse) fragment RGB components
    OneMinusSrcColor,

    // destination pixel multiplied by fragment alpha component
    SrcAlpha,

    // destination pixel multiplied by fragment inverse alpha
    OneMinusSrcAlpha,

    // destination pixel multiplied by fragment RGB components prior to fogging
    SrcColorPreFog
}

enum W3dShaderPrimaryGradient : uint8
{
    // disable primary gradient (same as OpenGL 'decal' texture blend)
    Disable = 0,

    // modulate fragment ARGB by gradient ARGB (default)
    Modulate,

    // add gradient RGB to fragment RGB, copy gradient A to fragment A
    Add,

    // environment-mapped bump mapping
    BumpEnvMap
}

enum W3dShaderSecondaryGradient : uint8
{
    // don't draw secondary gradient (default)
    Disable = 0,

    // add secondary gradient RGB to fragment RGB 
    Enable
}

enum W3dShaderSrcBlendFunc : uint8
{
    // fragment not added to color buffer
    Zero = 0,

    // fragment added unmodified to color buffer (default)
    One,

    // fragment RGB components multiplied by fragment A
    SrcAlpha,

    // fragment RGB components multiplied by fragment inverse (one minus) A
    OneMinusSrcAlpha
}

enum W3dShaderTexturing : uint8
{
    // no texturing (treat fragment initial color as 1,1,1,1) (default)
    Disable = 0,

    // enable texturing
    Enable
}

enum W3dShaderDetailColorFunc : uint8
{
    // local (default)
    Disable = 0,

    // other
    Detail,

    // local * other
    Scale,

    // ~(~local * ~other) = local + (1-local)*other
    InvScale,

    // local + other
    Add,

    // local - other
    Sub,

    // other - local
    SubR,

    // (localAlpha)*local + (~localAlpha)*other
    Blend,

    // (otherAlpha)*local + (~otherAlpha)*other
    DetailBlend
}

enum W3dShaderDetailAlphaFunc : uint8
{
    // local (default)
    Disable = 0,

    // other
    Detail,

    // local * other
    Scale,

    // ~(~local * ~other) = local + (1-local)*other
    InvScale
}

enum W3dShaderAlphaTest : uint8
{
    // disable alpha testing (default)
    Disable = 0,

    // enable alpha testing
    Enable
}
```

Structures
----------

``` csharp
struct W3dVector3
{
    float32 X;
    float32 Y;
    float32 Z;
}
```