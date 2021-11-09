

# lock_table/include/lock_table.h



## Namespaces

| Name           |
| -------------- |
| **[std](/Namespaces/std)**  |

## Classes

|                | Name           |
| -------------- | -------------- |
| struct | **[Lock](/Classes/Lock)**  |
| struct | **[std::hash< LockLocation >](/Classes/std::hash< LockLocation >)**  |

## Types

|                | Name           |
| -------------- | -------------- |
| typedef std::pair< int, int64_t > | **[LockLocation](/Modules/LockManager#typedef-locklocation)**  |
| typedef struct <a href="/Classes/Lock">Lock</a> | **[lock_t](/Modules/LockManager#typedef-lock_t)**  |

## Functions

|                | Name           |
| -------------- | -------------- |
| int | **[init_lock_table](/Modules/LockManager#function-init_lock_table)**() |
| <a href="/Classes/lock_t">lock_t</a> * | **[lock_acquire](/Modules/LockManager#function-lock_acquire)**(int table_id, int64_t key)<br>Acquire a lock corresponding to given table id and key.  |
| int | **[lock_release](/Modules/LockManager#function-lock_release)**(<a href="/Classes/lock_t">lock_t</a> * lock_obj)<br>Release a lock.  |

## Types Documentation

### typedef LockLocation

```cpp
typedef std::pair<int, int64_t> LockLocation;
```


### typedef lock_t

```cpp
typedef struct Lock lock_t;
```



## Functions Documentation

### function init_lock_table

```cpp
int init_lock_table()
```


**Return**: 0 if success, nonzero otherwise. 

### function lock_acquire

```cpp
lock_t * lock_acquire(
    int table_id,
    int64_t key
)
```

Acquire a lock corresponding to given table id and key. 

**Parameters**: 

  * **table_id** table id. 
  * **key** row key. 


**Return**: 0 if success, nonzero otherwise. 

If there are no existing lock, it instantly returns a new lock instance. Otherwise, blocks until all previous lock is released and returns.


### function lock_release

```cpp
int lock_release(
    lock_t * lock_obj
)
```

Release a lock. 

**Parameters**: 

  * **lock_obj** lock object acquired by <code><a href="/Modules/LockManager#function-lock-acquire">lock&#95;acquire()</a></code>. 


**Return**: 0 if success, nonzero otherwise. 

Releases given lock and wakes up next blocked <code><a href="/Modules/LockManager#function-lock-acquire">lock&#95;acquire()</a></code>.




## Source code

```cpp

#pragma once

#include <pthread.h>

#include <cstdint>
#include <functional>

typedef std::pair<int, int64_t> LockLocation;

struct Lock {
  LockLocation lock_location;

  pthread_cond_t* cond;

  Lock* prev;
  Lock* next;
};

namespace std {
template <>
struct hash<LockLocation> {
    size_t operator()(const LockLocation& location) const {
        size_t hash_value = 17;
        hash_value = hash_value * 31 + std::hash<int>()(location.first);
        hash_value = hash_value * 31 + std::hash<int64_t>()(location.second);
        return hash_value;
    }
};
}  // namespace std

typedef struct Lock lock_t;

/* APIs for lock table */

int init_lock_table();

lock_t *lock_acquire(int table_id, int64_t key);

int lock_release(lock_t* lock_obj);
```


-------------------------------

Updated on 2021-11-09 at 23:03:19 +0900
