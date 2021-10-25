---
title: HeaderPage
summary: struct for the header page. 

---

# HeaderPage

**Module:** **[DiskSpaceManager](/Modules/group__DiskSpaceManager)**



struct for the header page. 


`#include <page.h>`

Inherits from [Page](/Classes/structPage)

## Public Attributes

|                | Name           |
| -------------- | -------------- |
| pagenum_t | **[free_page_idx](/Classes/structHeaderPage#variable-free-page-idx)** <br>The first free page index.  |
| uint64_t | **[page_num](/Classes/structHeaderPage#variable-page-num)** <br>Total count of the page reserved.  |
| pagenum_t | **[root_page_idx](/Classes/structHeaderPage#variable-root-page-idx)** <br>The root page index.  |
| uint8_t | **[reserved](/Classes/structHeaderPage#variable-reserved)** <br>Reserved area for next project.  |

## Public Attributes Documentation

### variable free_page_idx

```cpp
pagenum_t free_page_idx;
```

The first free page index. 

### variable page_num

```cpp
uint64_t page_num;
```

Total count of the page reserved. 

### variable root_page_idx

```cpp
pagenum_t root_page_idx;
```

The root page index. 

### variable reserved

```cpp
uint8_t reserved;
```

Reserved area for next project. 

-------------------------------

Updated on 2021-10-25 at 16:59:00 +0900