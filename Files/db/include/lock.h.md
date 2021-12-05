

# db/include/lock.h



## Namespaces

| Name           |
| -------------- |
| **[lock_helper](/Namespaces/lock_helper)**  |

## Classes

|                | Name           |
| -------------- | -------------- |
| class | **[LockList](/Classes/LockList)** <br>Lock list.  |
| struct | **[Lock](/Classes/Lock)**  |

## Types

|                | Name           |
| -------------- | -------------- |
| enum| **[LockMode](/Modules/LockManager#enum-lockmode)** { SHARED = 0, EXCLUSIVE = 1} |
| typedef struct <a href="/Classes/Lock">Lock</a> | **[lock_t](/Modules/LockManager#typedef-lock_t)**  |

## Functions

|                | Name           |
| -------------- | -------------- |
| int | **[init_lock_table](/Modules/LockManager#function-init_lock_table)**()<br>Initialize lock table.  |
| int | **[cleanup_lock_table](/Modules/LockManager#function-cleanup_lock_table)**()<br>Cleanup lock table.  |
| <a href="/Classes/Lock">Lock</a> * | **[explicit_lock](/Modules/LockManager#function-explicit_lock)**(tableid_t table_id, pagenum_t page_id, int key_idx, trxid_t trx_id)<br>Create an acquired explicit lock.  |
| <a href="/Classes/Lock">Lock</a> * | **[lock_acquire](/Modules/LockManager#function-lock_acquire)**(tableid_t table_id, pagenum_t page_id, int key_idx, trxid_t trx_id, int lock_mode)<br>Acquire a lock corresponding to given table id and key.  |
| int | **[lock_release](/Modules/LockManager#function-lock_release)**(<a href="/Classes/Lock">Lock</a> * lock_obj)<br>Release a lock.  |
| bool | **[empty_trx](/Modules/LockManager#function-empty_trx)**(trxid_t trx_id) |

## Types Documentation

### enum LockMode

| Enumerator | Value | Description |
| ---------- | ----- | ----------- |
| SHARED | 0|   |
| EXCLUSIVE | 1|   |




### typedef lock_t

```cpp
typedef struct Lock lock_t;
```



## Functions Documentation

### function init_lock_table

```cpp
int init_lock_table()
```

Initialize lock table. 

**Return**: <code>0</code> if success, negative value otherwise. 

### function cleanup_lock_table

```cpp
int cleanup_lock_table()
```

Cleanup lock table. 

**Return**: <code>0</code> if success, negative value otherwise. 

### function explicit_lock

```cpp
Lock * explicit_lock(
    tableid_t table_id,
    pagenum_t page_id,
    int key_idx,
    trxid_t trx_id
)
```

Create an acquired explicit lock. 

**Parameters**: 

  * **table_id** table id. 
  * **page_id** page id. 
  * **key_idx** record key index. 
  * **trx_id** transaction id. 


**Return**: created lock instance. 

Implicit locking is only for X-lock, so we can safely assume that created lock is always has <code>EXCLUSIVE</code> lock mode.


### function lock_acquire

```cpp
Lock * lock_acquire(
    tableid_t table_id,
    pagenum_t page_id,
    int key_idx,
    trxid_t trx_id,
    int lock_mode
)
```

Acquire a lock corresponding to given table id and key. 

**Parameters**: 

  * **table_id** table id. 
  * **page_id** page id. 
  * **key_idx** record key index. 
  * **trx_id** transaction id. 
  * **lock_mode** lock mode. 


**Return**: non-null lock instance if success, <code>nullptr</code> otherwise. 

If there are no existing lock, it instantly returns a new lock instance. Otherwise, blocks until all previous lock is released and returns.


### function lock_release

```cpp
int lock_release(
    Lock * lock_obj
)
```

Release a lock. 

**Parameters**: 

  * **lock_obj** lock object acquired by <code><a href="/Modules/LockManager#function-lock-acquire">lock&#95;acquire()</a></code>. 


**Return**: 0 if success, nonzero otherwise. 

Releases given lock and wakes up next blocked <code><a href="/Modules/LockManager#function-lock-acquire">lock&#95;acquire()</a></code>.


### function empty_trx

```cpp
bool empty_trx(
    trxid_t trx_id
)
```




## Source code

```cpp

#pragma once

#include <pthread.h>
#include <types.h>

#include <cstdint>
#include <functional>

enum LockMode { SHARED = 0, EXCLUSIVE = 1 };
struct Lock;
struct LockList {
    LockLocation lock_location;

    Lock* head;
    Lock* tail;
};

struct Lock {
    LockMode lock_mode;
    bool acquired;

    pthread_cond_t* cond;

    LockList* list;

    trxid_t trx_id;
    lockmask_t mask;

    Lock* prev;
    Lock* next;
    Lock* next_trx;
};

namespace lock_helper {

constexpr int get_bit(Lock* lock, int pos) {
    return (lock->mask & (uint64_t(1) << pos)) >> pos;
}
constexpr int set_bit(Lock* lock, int pos) {
    return lock->mask |= (uint64_t(1) << pos);
}
constexpr int clear_bit(uint64_t& mask, int pos) {
    return mask |= ~(uint64_t(1) << pos);
}

}  // namespace lock_helper

/* APIs for lock table */

int init_lock_table();

int cleanup_lock_table();

Lock* explicit_lock(tableid_t table_id, pagenum_t page_id, int key_idx, trxid_t trx_id);

Lock* lock_acquire(tableid_t table_id, pagenum_t page_id, int key_idx,
                   trxid_t trx_id, int lock_mode);

int lock_release(Lock* lock_obj);

bool empty_trx(trxid_t trx_id);

typedef struct Lock lock_t;
```


-------------------------------

Updated on 2021-12-05 at 22:45:19 +0900
