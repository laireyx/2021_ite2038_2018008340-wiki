

# Lock

**Module:** **[LockManager](/Modules/LockManager)**






`#include <lock_table.h>`

## Public Attributes

|                | Name           |
| -------------- | -------------- |
| <a href="/Modules/LockManager#typedef-locklocation">LockLocation</a> | **[lock_location](/Classes/Lock#variable-lock_location)** <br><a href="/Classes/Lock">Lock</a> table and key information.  |
| pthread_cond_t * | **[cond](/Classes/Lock#variable-cond)** <br>Conditional variable for this lock.  |
| <a href="/Classes/Lock">Lock</a> * | **[prev](/Classes/Lock#variable-prev)** <br>Previous waiting lock.  |
| <a href="/Classes/Lock">Lock</a> * | **[next](/Classes/Lock#variable-next)** <br>Next waiting lock.  |

## Public Attributes Documentation

### variable lock_location

```cpp
LockLocation lock_location;
```

<a href="/Classes/Lock">Lock</a> table and key information. 

### variable cond

```cpp
pthread_cond_t * cond;
```

Conditional variable for this lock. 

### variable prev

```cpp
Lock * prev;
```

Previous waiting lock. 

### variable next

```cpp
Lock * next;
```

Next waiting lock. 

-------------------------------

Updated on 2021-11-09 at 23:03:19 +0900