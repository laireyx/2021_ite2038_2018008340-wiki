---
title: TableInstance
summary: Table file instance. 

---

# TableInstance

**Module:** **[DiskSpaceManager](/Modules/group__DiskSpaceManager)**



Table file instance. 


`#include <file.h>`

## Public Attributes

|                | Name           |
| -------------- | -------------- |
| char * | **[file_path](/Classes/structTableInstance#variable-file-path)** <br>Real table file path(obtained by realpath(3)).  |
| int | **[file_descriptor](/Classes/structTableInstance#variable-file-descriptor)** <br>Table file descriptor.  |
| <a href="/Modules/group__DiskSpaceManager#typedef-headerpage-t">headerpage_t</a> | **[header_page](/Classes/structTableInstance#variable-header-page)** <br>Table header page.  |

## Public Attributes Documentation

### variable file_path

```cpp
char * file_path;
```

Real table file path(obtained by realpath(3)). 

### variable file_descriptor

```cpp
int file_descriptor;
```

Table file descriptor. 

### variable header_page

```cpp
headerpage_t header_page;
```

Table header page. 

-------------------------------

Updated on 2021-10-25 at 17:06:26 +0900