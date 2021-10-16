---
title: LeafPage
summary: struct for allocated leaf page. 

---

# LeafPage

**Module:** **[DiskSpaceManager](/Modules/group__DiskSpaceManager)**



struct for allocated leaf page. 


`#include <page.h>`

Inherits from [AllocatedPage](/Classes/structAllocatedPage), [Page](/Classes/structPage)

## Public Attributes

|                | Name           |
| -------------- | -------------- |
| uint8_t | **[reserved](/Classes/structLeafPage#variable-reserved)** <br>Reserved area for normal allocated page.  |

## Additional inherited members

**Public Attributes inherited from [AllocatedPage](/Classes/structAllocatedPage)**

|                | Name           |
| -------------- | -------------- |
| <a href="/Classes/structPageHeader">PageHeader</a> | **[page_header](/Classes/structAllocatedPage#variable-page-header)** <br><a href="/Classes/structPage">Page</a> header.  |


## Public Attributes Documentation

### variable reserved

```cpp
uint8_t reserved;
```

Reserved area for normal allocated page. 

-------------------------------

Updated on 2021-10-16 at 22:36:54 +0900