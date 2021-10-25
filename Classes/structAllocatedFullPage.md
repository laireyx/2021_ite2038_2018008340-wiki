---
title: AllocatedFullPage
summary: struct for any allocated page. 

---

# AllocatedFullPage

**Module:** **[DiskSpaceManager](/Modules/group__DiskSpaceManager)**



struct for any allocated page.  [More...](#detailed-description)


`#include <page.h>`

Inherits from [AllocatedPage](/Classes/structAllocatedPage), [Page](/Classes/structPage)

## Public Attributes

|                | Name           |
| -------------- | -------------- |
| uint8_t | **[reserved](/Classes/structAllocatedFullPage#variable-reserved)** <br>Reserved area for normal allocated page.  |

## Additional inherited members

**Public Attributes inherited from [AllocatedPage](/Classes/structAllocatedPage)**

|                | Name           |
| -------------- | -------------- |
| <a href="/Classes/structPageHeader">PageHeader</a> | **[page_header](/Classes/structAllocatedPage#variable-page-header)** <br><a href="/Classes/structPage">Page</a> header.  |


## Detailed Description

```cpp
class AllocatedFullPage;
```

struct for any allocated page. 

You should use this struct for read & write allocated page. 

## Public Attributes Documentation

### variable reserved

```cpp
uint8_t reserved;
```

Reserved area for normal allocated page. 

-------------------------------

Updated on 2021-10-25 at 17:06:26 +0900