

# PageSlot

**Module:** **[DiskSpaceManager](/Modules/DiskSpaceManager)**



page slot for allocated(internal and leaf) node. 


`#include <page.h>`

## Public Attributes

|                | Name           |
| -------------- | -------------- |
| recordkey_t | **[key](/Classes/PageSlot#variable-key)** <br>The page key.  |
| valsize_t | **[value_size](/Classes/PageSlot#variable-value_size)** <br>The value size(in bytes).  |
| uint16_t | **[value_offset](/Classes/PageSlot#variable-value_offset)** <br>The value offset(in bytes).  |

## Public Attributes Documentation

### variable key

```cpp
recordkey_t key;
```

The page key. 

### variable value_size

```cpp
valsize_t value_size;
```

The value size(in bytes). 

### variable value_offset

```cpp
uint16_t value_offset;
```

The value offset(in bytes). 

-------------------------------

Updated on 2021-12-05 at 18:37:58 +0900