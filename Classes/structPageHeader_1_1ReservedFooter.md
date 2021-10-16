---
title: PageHeader::ReservedFooter

---

# PageHeader::ReservedFooter






`#include <page.h>`

## Public Attributes

|                | Name           |
| -------------- | -------------- |
| uint64_t | **[footer_1](/Classes/structPageHeader_1_1ReservedFooter#variable-footer-1)** <br>Can be free space(in leaf page).  |
| uint64_t | **[footer_2](/Classes/structPageHeader_1_1ReservedFooter#variable-footer-2)** <br>Can be leftmost children idx(in internal page) or next sibling idx(in leaf page).  |

## Public Attributes Documentation

### variable footer_1

```cpp
uint64_t footer_1;
```

Can be free space(in leaf page). 

### variable footer_2

```cpp
uint64_t footer_2;
```

Can be leftmost children idx(in internal page) or next sibling idx(in leaf page). 

-------------------------------

Updated on 2021-10-16 at 22:36:54 +0900