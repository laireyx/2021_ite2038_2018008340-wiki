---
title: db/include/types.h

---

# db/include/types.h



## Types

|                | Name           |
| -------------- | -------------- |
| typedef uint64_t | **[pagenum_t](/Files/types_8h#typedef-pagenum-t)**  |
| typedef int64_t | **[tableid_t](/Files/types_8h#typedef-tableid-t)**  |

## Types Documentation

### typedef pagenum_t

```cpp
typedef uint64_t pagenum_t;
```


### typedef tableid_t

```cpp
typedef int64_t tableid_t;
```





## Source code

```cpp
#pragma once

#include <cstdint>
#include <memory>

typedef uint64_t pagenum_t;
typedef int64_t tableid_t;
```


-------------------------------

Updated on 2021-10-25 at 17:06:26 +0900
