---
title: FreePage
summary: struct for the free page. 

---

# FreePage

**Module:** **[DiskSpaceManager](/Modules/group__DiskSpaceManager)**



struct for the free page. 


`#include <page.h>`

Inherits from [Page](/Classes/structPage)

## Public Attributes

|                | Name           |
| -------------- | -------------- |
| pagenum_t | **[next_free_idx](/Classes/structFreePage#variable-next-free-idx)** <br>Index of the very next free page.  |
| uint8_t | **[reserved](/Classes/structFreePage#variable-reserved)** <br>Reserved area for next project.  |

## Public Attributes Documentation

### variable next_free_idx

```cpp
pagenum_t next_free_idx;
```

Index of the very next free page. 

### variable reserved

```cpp
uint8_t reserved;
```

Reserved area for next project. 

-------------------------------

Updated on 2021-10-25 at 17:06:26 +0900