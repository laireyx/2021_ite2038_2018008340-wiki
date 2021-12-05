

# LockManager



## Namespaces

| Name           |
| -------------- |
| **[lock_helper](/Namespaces/lock_helper)**  |

## Classes

|                | Name           |
| -------------- | -------------- |
| class | **[LockList](/Classes/LockList)** <br>Lock list.  |
| struct | **[Lock](/Classes/Lock)**  |
| class | **[lock_t](/Classes/lock_t)** <br>Lock instance.  |

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
| <a href="/Classes/Lock">Lock</a> * | **[lock_acquire](/Modules/LockManager#function-lock_acquire)**(int table_id, pagenum_t page_id, int key_idx, trxid_t trx_id, int lock_mode)<br>Acquire a lock corresponding to given table id and key.  |
| int | **[lock_release](/Modules/LockManager#function-lock_release)**(<a href="/Classes/Lock">Lock</a> * lock_obj)<br>Release a lock.  |

## Attributes

|                | Name           |
| -------------- | -------------- |
| pthread_mutex_t * | **[lock_manager_mutex](/Modules/LockManager#variable-lock_manager_mutex)** <br><a href="/Classes/Lock">Lock</a> manager mutex.  |
| std::unordered_map< LockLocation, <a href="/Classes/LockList">LockList</a> * > | **[lock_instances](/Modules/LockManager#variable-lock_instances)**  |
| std::unordered_map< trxid_t, std::unordered_set< trxid_t > > | **[trx_wait](/Modules/LockManager#variable-trx_wait)** <br>Transaction waiting list.  |

## Types Documentation

### enum LockMode

| Enumerator | Value | Description |
| ---------- | ----- | ----------- |
| SHARED | 0|   |
| EXCLUSIVE | 1|   |




### typedef lock_t

```
typedef struct Lock lock_t;
```



## Functions Documentation

### function init_lock_table

```
int init_lock_table()
```

Initialize lock table. 

**Return**: <code>0</code> if success, negative value otherwise. 

### function cleanup_lock_table

```
int cleanup_lock_table()
```

Cleanup lock table. 

**Return**: <code>0</code> if success, negative value otherwise. 

### function lock_acquire

```
Lock * lock_acquire(
    int table_id,
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
  * **key** record key index. 
  * **trx_id** transaction id. 
  * **lock_mode** lock mode. 


**Return**: non-null lock instance if success, <code>nullptr</code> otherwise. 

If there are no existing lock, it instantly returns a new lock instance. Otherwise, blocks until all previous lock is released and returns.


### function lock_release

```
int lock_release(
    Lock * lock_obj
)
```

Release a lock. 

**Parameters**: 

  * **lock_obj** lock object acquired by <code><a href="/Modules/LockManager#function-lock-acquire">lock&#95;acquire()</a></code>. 


**Return**: 0 if success, nonzero otherwise. 

Releases given lock and wakes up next blocked <code><a href="/Modules/LockManager#function-lock-acquire">lock&#95;acquire()</a></code>.



## Attributes Documentation

### variable lock_manager_mutex

```
pthread_mutex_t * lock_manager_mutex;
```

<a href="/Classes/Lock">Lock</a> manager mutex. 

### variable lock_instances

```
std::unordered_map< LockLocation, LockList * > lock_instances;
```


**Todo**: change it to MAX_TABLE_INSTANCE 

### variable trx_wait

```
std::unordered_map< trxid_t, std::unordered_set< trxid_t > > trx_wait;
```

Transaction waiting list. 




-------------------------------

Updated on 2021-12-05 at 18:36:40 +0900