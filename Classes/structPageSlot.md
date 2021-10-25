---
title: PageSlot
summary: page slot for allocated(internal and leaf) node. 

---

# PageSlot

**Module:** **[DiskSpaceManager](/Modules/group__DiskSpaceManager)**



page slot for allocated(internal and leaf) node. 


`#include <page.h>`

## Public Attributes

|                | Name           |
| -------------- | -------------- |
| int64_t | **[key](/Classes/structPageSlot#variable-key)** <br>The page key.  |
| uint16_t | **[value_size](/Classes/structPageSlot#variable-value-size)** <br>The value size(in bytes).  |
| uint16_t | **[value_offset](/Classes/structPageSlot#variable-value-offset)** <br>The value offset(in bytes).  |

## Public Attributes Documentation

### variable key

```cpp
int64_t key;
```

The page key. 

### variable value_size

```cpp
uint16_t value_size;
```

The value size(in bytes). 

### variable value_offset

```cpp
uint16_t value_offset;
```

The value offset(in bytes). 

-------------------------------

Updated on 2021-10-25 at 16:59:00 +0900