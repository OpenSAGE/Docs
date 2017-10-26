# W3D Mesh Header Chunk

The W3D_CHUNK_MESH_HEADER3 chunk is structured as follows:

| Offset | Bytes | Type  | Name      |
|--------|-------|-------|-----------|
| 0      | 4     | UINT32  | Version   |
| 4      | 4     | W3D_MESH_FLAGS  | Attributes   |
| 8      | 16     | CHAR[16]  | MeshName   |
| 24      | 16     | CHAR[16]  | ContainerName   |
| 40      | 4     | UINT32  | NumTriangles   |
| 44      | 4     | UINT32  | NumVertices   |
| 48      | 4     | UINT32  | NumMaterials   |
| 52      | 4     | UINT32  | NumDamageStages   |
| 56      | 4     | UINT32  | SortLevel   |
| 60      | 4     | UINT32  | PrelitVersion   |
| 64      | 4     | UINT32  | FutureCounts   |
| 68      | 4     | W3D_VERTEX_CHANNELS  | VertexChannels   |
| 72      | 4     | W3D_FACE_CHANNELS  | FaceChannels   |
| 76      | 4     | W3D_VECTOR3  | BoundingBoxMin   |
| 88      | 4     | W3D_VECTOR3  | BoundingBoxMax   |
| 100      | 4     | W3D_VECTOR3  | SphereCenter   |
| 112      | 4     | FLOAT  | SphereRadius   |