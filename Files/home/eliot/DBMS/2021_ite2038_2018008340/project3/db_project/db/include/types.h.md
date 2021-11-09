

# /home/eliot/DBMS/2021_ite2038_2018008340/project3/db_project/db/include/types.h



## Types

|                | Name           |
| -------------- | -------------- |
| typedef uint64_t | **[pagenum_t](/Files/home/eliot/DBMS/2021_ite2038_2018008340/project3/db_project/db/include/types.h#typedef-pagenum_t)**  |
| typedef int64_t | **[tableid_t](/Files/home/eliot/DBMS/2021_ite2038_2018008340/project3/db_project/db/include/types.h#typedef-tableid_t)**  |

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

Updated on 2021-11-09 at 23:03:19 +0900
