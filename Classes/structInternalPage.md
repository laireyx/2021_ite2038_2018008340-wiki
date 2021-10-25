---
title: InternalPage
summary: struct for allocated internal page. 

---

# InternalPage

**Module:** **[DiskSpaceManager](/Modules/group__DiskSpaceManager)**



struct for allocated internal page. 


`#include <page.h>`

Inherits from [AllocatedPage](/Classes/structAllocatedPage), [Page](/Classes/structPage)

## Public Attributes

|                | Name           |
| -------------- | -------------- |
| <a href="/Classes/structPageBranch">PageBranch</a> | **[page_branches](/Classes/structInternalPage#variable-page-branches)** <br><a href="/Classes/structPage">Page</a> branches.  |

## Additional inherited members

**Public Attributes inherited from [AllocatedPage](/Classes/structAllocatedPage)**

|                | Name           |
| -------------- | -------------- |
| <a href="/Classes/structPageHeader">PageHeader</a> | **[page_header](/Classes/structAllocatedPage#variable-page-header)** <br><a href="/Classes/structPage">Page</a> header.  |


## Public Attributes Documentation

### variable page_branches

```cpp
PageBranch page_branches;
```

<a href="/Classes/structPage">Page</a> branches. 

-------------------------------

Updated on 2021-10-25 at 16:59:00 +0900