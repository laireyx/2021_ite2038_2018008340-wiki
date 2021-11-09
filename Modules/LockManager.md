

# LockManager



## Namespaces

| Name           |
| -------------- |
| **[std](/Namespaces/std)**  |

## Classes

|                | Name           |
| -------------- | -------------- |
| struct | **[Lock](/Classes/Lock)**  |
| class | **[lock_t](/Classes/lock_t)** <br>Lock instance.  |

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

## Attributes

|                | Name           |
| -------------- | -------------- |
| pthread_mutex_t * | **[lock_manager_mutex](/Modules/LockManager#variable-lock_manager_mutex)**  |
| std::map< <a href="/Modules/LockManager#typedef-locklocation">LockLocation</a>, <a href="/Classes/lock_t">lock_t</a> * > | **[lock_instances](/Modules/LockManager#variable-lock_instances)**  |

## Types Documentation

### typedef LockLocation

```
typedef std::pair<int, int64_t> LockLocation;
```


### typedef lock_t

```
typedef struct Lock lock_t;
```



## Functions Documentation

### function init_lock_table

```
int init_lock_table()
```


**Return**: 0 if success, nonzero otherwise. 

### function lock_acquire

```
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

```
int lock_release(
    lock_t * lock_obj
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


### variable lock_instances

```
std::map< LockLocation, lock_t * > lock_instances;
```


**Todo**: change it to MAX_TABLE_INSTANCE 




-------------------------------

Updated on 2021-11-09 at 23:03:19 +0900