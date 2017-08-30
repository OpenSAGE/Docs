# WAK

## Description

`.wak` files were used in C&C Generals and Zero Hour to store ocean and pond wave definitions. In later SAGE games, this data became part of the `.map` format.

Not all maps have accompanying `.wak` files, but those that require wave movement do.

## Specification

### File Structure

The file has a variable number of entries. The number of entries is stored at the end of the file.

| Offset | Bytes | Type         | Name        |
|--------|-------|--------------|-------------|
| 0      | N     | WAVE_ENTRY[] | ENTRIES     |
| N      | 4     | UINT         | NUM_ENTRIES |

* WAVE_ENTRY: See below for wave entry structure
* NUM_ENTRIES: Number of entries

### Wave Entry

Each WAVE_ENTRY looks like the following:

| Offset | Bytes | Type  | Name      |
|--------|-------|-------|-----------|
| 0      | 4     | UINT  | START_X   |
| 4      | 4     | UINT  | START_Y   |
| 8      | 4     | UINT  | END_X     |
| 12     | 4     | UINT  | END_Y     |
| 16     | 4     | UINT  | WAVE_TYPE |

* START_X and START_Y: Specify the start position of the wave
* END_X and END_Y: Specify the end position of the wave
* WAVE_TYPE: Type of the wave. See WAVE_TYPE enumeration below.

### Enumerations

#### Wave Type

| Value | Name               |
|-------|--------------------|
| 0     | POND               |
| 1     | OCEAN              |
| 2     | CLOSE_OCEAN        |
| 3     | CLOSE_OCEAN_DOUBLE |
| 4     | RADIAL             |