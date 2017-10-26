# W3D

## Description

`.w3d` files were used in the following games to store 3D models:

* C&C Generals
* Zero Hour
* BFME I
* BFME II

Later SAGE games (C&C3 and RA3) used an XML-based evolution of the w3d format called w3x.

## References

* [w3d2ply/w3d_file.h](https://github.com/mikolalysenko/w3d2ply/blob/ecd8302b6cfd0578ab249cb95c8b70636c4609bc/w3d_file.h)
* [libw3d/types.hpp](https://github.com/feliwir/libw3d/blob/fb547b28c91f17070d65ba24edf7a5294a0554d9/include/libw3d/types.hpp)

## Specification

### Chunks

w3d is a chunk-based binary format. Chunks can optionally contain sub-chunks. Every chunk starts with a chunk header:

| Offset | Bytes | Type       | Name       |
|--------|-------|------------|------------|
| 0      | 4     | CHUNK_TYPE | ChunkType |
| 4      | 4     | UINT       | ChunkSize |

* CHUNK_TYPE: See below for chunk type enumeration
* CHUNK_SIZE: MSB is 1 if the chunk contains sub-chunks, otherwise it's 0. Lower 31 bits are the chunk size.

#### CHUNK_TYPE Enumeration

| Value | Name               |
|------:|--------------------|
| 0x0   | [W3D_CHUNK_MESH](chunks/w3d-chunk-mesh)                 |
| 0x2   | [W3D_CHUNK_VERTICES](#w3d-chunk-vertices)             |
| 0x3   | [W3D_CHUNK_VERTEX_NORMALS](#w3d-chunk-vertex-normals)       |
| 0xC   | W3D_CHUNK_MESH_USER_TEXT       |
| 0xE   | W3D_CHUNK_VERTEX_INFLUENCES    |
| 0x1F  | W3D_CHUNK_MESH_HEADER3         |
| 0x20  | W3D_CHUNK_TRIANGLES            |
| 0x22  | W3D_CHUNK_VERTEX_SHADE_INDICES |
| 0x23  | W3D_CHUNK_PRELIT_UNLIT |
| 0x24  | W3D_CHUNK_PRELIT_VERTEX |
| 0x25  | W3D_CHUNK_PRELIT_LIGHTMAP_MULTI_PASS |
| 0x26  | W3D_CHUNK_PRELIT_LIGHTMAP_MULTI_TEXTURE |
| 0x28  | W3D_CHUNK_MATERIAL_INFO |
| 0x29  | W3D_CHUNK_SHADERS |
| 0x2A  | W3D_CHUNK_VERTEX_MATERIALS |
| 0x2B  | W3D_CHUNK_VERTEX_MATERIAL |
| 0x2C  | W3D_CHUNK_VERTEX_MATERIAL_NAME |
| 0x2D  | W3D_CHUNK_VERTEX_MATERIAL_INFO |
| 0x2E  | W3D_CHUNK_VERTEX_MAPPER_ARGS0 |
| 0x2F  | W3D_CHUNK_VERTEX_MAPPER_ARGS1 |
| 0x30  | W3D_CHUNK_TEXTURES |

#### <a name="w3d-chunk-vertices"></a>W3D_CHUNK_VERTICES

| Offset | Bytes | Type  | Name      |
|--------|-------|-------|-----------|
| 0      | 12 * N     | [W3D_VECTOR3](#w3d-vector3)[N]  | Vertices   |

N is the number of vertices as specified in the W3D_MESH_HEADER3 chunk.

#### <a name="w3d-chunk-vertex-normals"></a>W3D_CHUNK_VERTEX_NORMALS

| Offset | Bytes | Type  | Name      |
|--------|-------|-------|-----------|
| 0      | 12 * N     | [W3D_VECTOR3](#w3d-vector3)[N]  | Normals   |

N is the number of vertices as specified in the W3D_MESH_HEADER3 chunk.

### Structures

#### <a name="w3d-vector3"></a>W3D_VECTOR3

| Offset | Bytes | Type  | Name      |
|--------|-------|-------|-----------|
| 0      | 4     | FLOAT  | X   |
| 4      | 4     | FLOAT  | Y   |
| 8      | 4     | FLOAT  | Z   |