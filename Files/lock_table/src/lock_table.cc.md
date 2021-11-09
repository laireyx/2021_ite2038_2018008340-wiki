

# lock_table/src/lock_table.cc



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



## Attributes Documentation

### variable lock_manager_mutex

```cpp
pthread_mutex_t * lock_manager_mutex;
```


### variable lock_instances

```cpp
std::map< LockLocation, lock_t * > lock_instances;
```


**Todo**: change it to MAX_TABLE_INSTANCE 


## Source code

```cpp

#include "lock_table.h"

#include <new>
#include <map>

pthread_mutex_t* lock_manager_mutex;

std::map<LockLocation, lock_t*> lock_instances;

int init_lock_table() {
  try {
    lock_manager_mutex = new pthread_mutex_t;
    return pthread_mutex_init(lock_manager_mutex, nullptr);
  } catch(std::bad_alloc exception) {
    return -1;
  }
}

lock_t* lock_acquire(int table_id, int64_t key) {
  pthread_mutex_lock(lock_manager_mutex);
  lock_t* lock_instance = new lock_t;
  auto lock_location = std::make_pair(table_id, key);

  lock_instance->lock_location = lock_location;

  lock_instance->cond = nullptr;

  lock_instance->prev = nullptr;
  lock_instance->next = nullptr;

  if(lock_instances.find(lock_location) == lock_instances.end()) {
    lock_instance->cond = new pthread_cond_t;
    pthread_cond_init(lock_instance->cond, nullptr);

    lock_instances[lock_location] = lock_instance;
    pthread_mutex_unlock(lock_manager_mutex);
    return lock_instance;
  }
  
  lock_t* lock_tail = lock_instances[lock_location];
  while(lock_tail->next != nullptr) {
    lock_tail = lock_tail->next;
  }

  lock_instance->cond = lock_tail->cond;
  
  lock_instance->prev = lock_tail;
  lock_tail->next = lock_instance;
  
  while(true) {
    pthread_cond_wait(lock_instance->cond, lock_manager_mutex);

    if(lock_instance->prev == nullptr) {
      break;
    }
  }

  pthread_mutex_unlock(lock_manager_mutex);
  return lock_instance;
};

int lock_release(lock_t* lock_obj) {
  pthread_mutex_lock(lock_manager_mutex);

  if(lock_obj->next == nullptr) {
    lock_instances.erase(lock_obj->lock_location);

    pthread_cond_destroy(lock_obj->cond);
    delete lock_obj->cond;
    delete lock_obj;

    pthread_mutex_unlock(lock_manager_mutex);
    return 0;
  }

  lock_t* next_lock = lock_obj->next;
  next_lock->prev = nullptr;

  lock_instances[lock_obj->lock_location] = next_lock;
  delete lock_obj;

  pthread_cond_broadcast(next_lock->cond);
  pthread_mutex_unlock(lock_manager_mutex);
  return 0;
}
```


-------------------------------

Updated on 2021-11-09 at 23:03:19 +0900
