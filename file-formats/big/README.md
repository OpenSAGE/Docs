# BIG

## Description

Big files are an archive format, that was used in many game titles created by EA Studios

## Specification

### Header

The header has a fixed size of 16 bytes, following the table below:

| Offset | Bytes | Type | Name        |
|--------|-------|------|-------------|
| 0      | 4     |CHAR[]| FOURCC      |
| 4      | 4     | UINT | SIZE        |
| 8      | 4     | UINT | NUM_ENTRIES |
| 12     | 4     | UINT | OFFSET_FIRST|

* FOURCC: Identifies the string as valid big archive. The string may either be "BIG4" or "BIGF", depending on the version.
* SIZE: The entire size of an big archive. Size of a single archive can not be greater than 2^32 bytes
* NUM_ENTRIES: Number of files that were packed into this archive
* OFFSET_FIRST: The offset inside the file to the first entry

### List of entries

After the header the follows a list with NUM_ENTRIES elements, each entry looks the following:

| Offset | Bytes | Type  | Name        |
|--------|-------|-------|-------------|
| 0      | 4     | UINT  | ENTRY_OFFSET|
| 4      | 4     | UINT  | ENTRY_SIZE  |
| 8      | N     |CSTRING| ENTRY_NAME  |

* ENTRY_OFFSET: specified the start of this entry inside the file (in bytes)
* ENTRY_SIZE: the size of the specified entry
* ENTRY_NAME: the name of this entry, read as a nullterminated string


