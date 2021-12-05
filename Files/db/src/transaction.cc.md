

# db/src/transaction.cc



## Namespaces

| Name           |
| -------------- |
| **[trx_helper](/Namespaces/trx_helper)**  |

## Functions

|                | Name           |
| -------------- | -------------- |
| int | **[init_trx](/Modules/DiskSpaceManager#function-init_trx)**()<br>Initialize a transaction manager.  |
| int | **[cleanup_trx](/Modules/DiskSpaceManager#function-cleanup_trx)**()<br>Cleanup a transaction manager.  |
| trxid_t | **[trx_begin](/Modules/DiskSpaceManager#function-trx_begin)**()<br>Begin a transaction.  |
| trxid_t | **[trx_commit](/Modules/DiskSpaceManager#function-trx_commit)**(trxid_t trx_id)<br>Commit a transaction.  |

## Attributes

|                | Name           |
| -------------- | -------------- |
| pthread_mutex_t * | **[trx_manager_mutex](/Modules/DiskSpaceManager#variable-trx_manager_mutex)** <br>Transaction manager mutex.  |
| trxid_t | **[accumulated_trx_id](/Modules/DiskSpaceManager#variable-accumulated_trx_id)** <br>Accumulated trx id.  |
| trxid_t | **[accumulated_trxlog_id](/Modules/DiskSpaceManager#variable-accumulated_trxlog_id)** <br>Accumulated trx log id.  |
| std::unordered_map< trxid_t, <a href="/Classes/TransactionInstance">TransactionInstance</a> > | **[transaction_instances](/Modules/DiskSpaceManager#variable-transaction_instances)** <br>Transaction instances.  |
| std::unordered_map< trxlogid_t, <a href="/Classes/TransactionLog">TransactionLog</a> > | **[trx_logs](/Modules/DiskSpaceManager#variable-trx_logs)** <br>Transaction log.  |


## Functions Documentation

### function init_trx

```cpp
int init_trx()
```

Initialize a transaction manager. 

**Return**: <code>0</code> if success, negative value otherwise. 

### function cleanup_trx

```cpp
int cleanup_trx()
```

Cleanup a transaction manager. 

**Return**: <code>0</code> if success, negative value otherwise. 

### function trx_begin

```cpp
trxid_t trx_begin()
```

Begin a transaction. 

**Return**: positive transaction id if success. <code>0</code> or negative otherwise. 

### function trx_commit

```cpp
trxid_t trx_commit(
    trxid_t trx_id
)
```

Commit a transaction. 

**Parameters**: 

  * **trx_id** transaction id obtained with <code><a href="/Modules/DiskSpaceManager#function-trx-begin">trx&#95;begin()</a></code>. 


**Return**: <code>trx&#95;id</code>(committed transaction id) if success. <code>0</code> otherwise. 


## Attributes Documentation

### variable trx_manager_mutex

```cpp
pthread_mutex_t * trx_manager_mutex = nullptr;
```

Transaction manager mutex. 

### variable accumulated_trx_id

```cpp
trxid_t accumulated_trx_id = 0;
```

Accumulated trx id. 

### variable accumulated_trxlog_id

```cpp
trxid_t accumulated_trxlog_id = 0;
```

Accumulated trx log id. 

### variable transaction_instances

```cpp
std::unordered_map< trxid_t, TransactionInstance > transaction_instances;
```

Transaction instances. 

### variable trx_logs

```cpp
std::unordered_map< trxlogid_t, TransactionLog > trx_logs;
```

Transaction log. 


## Source code

```cpp

#include <pthread.h>
#include <transaction.h>
#include <tree.h>

#include <cstring>
#include <map>
#include <new>

pthread_mutex_t* trx_manager_mutex = nullptr;

trxid_t accumulated_trx_id = 0;
trxid_t accumulated_trxlog_id = 0;
std::unordered_map<trxid_t, TransactionInstance> transaction_instances;
std::unordered_map<trxlogid_t, TransactionLog> trx_logs;

namespace trx_helper {

TransactionInstance& get_trx_instance(trxid_t trx_id) {
    return transaction_instances[trx_id];
}
lock_t* lock_acquire(int table_id, pagenum_t page_id, int key_idx,
                   trxid_t trx_id, int lock_mode) {
    pthread_mutex_lock(trx_manager_mutex);
    TransactionInstance& instance = transaction_instances[trx_id];

    instance.state = WAITING;
    pthread_mutex_unlock(trx_manager_mutex);
    
    lock_t* lock;
    if(!(lock = ::lock_acquire(table_id, page_id, key_idx, trx_id, lock_mode))) {
        // Lock failed. abort this transaction.
        trx_abort(trx_id);
        return nullptr;
    }

    pthread_mutex_lock(trx_manager_mutex);
    if (instance.lock_head == nullptr) {
        instance.lock_head = lock;
        instance.lock_tail = lock;
    } else {
        if(!lock->next_trx && instance.lock_tail != lock) {
            instance.lock_tail->next_trx = lock;
            instance.lock_tail = lock;
        }
    }

    instance.state = RUNNING;
    pthread_mutex_unlock(trx_manager_mutex);
    return lock;
}

bool verify_trx(const TransactionInstance& instance) {
    return instance.state == RUNNING;
}

trxid_t new_trx_instance() {
    trxid_t instance_id = ++accumulated_trx_id;
    TransactionInstance instance;
    
    instance.state = RUNNING;
    instance.lock_head = instance.lock_tail = nullptr;
    instance.log_tail = 0;
    transaction_instances[instance_id] = instance;
    return instance_id;
}

void release_trx_locks(TransactionInstance& instance) {
    lock_t* lock_ptr = instance.lock_head;
    while (lock_ptr != nullptr) {
        lock_t* next_lock = lock_ptr->next_trx;
        lock_release(lock_ptr);
        lock_ptr = next_lock;
    }
}

void trx_rollback(trxid_t trx_id) {
    TransactionInstance& instance = transaction_instances[trx_id];

    trxid_t current_log_id = instance.log_tail;
    while (current_log_id != 0) {
        TransactionLog& log = trx_logs[current_log_id];
        trxid_t next_log_id = log.prev_trx_log;

        update_node(log.table_id, log.key, log.old_value, log.old_val_size,
                    nullptr, trx_id);

        trx_logs.erase(current_log_id);
        current_log_id = next_log_id;
    }
}

void flush_trx_log() {
}

void trx_abort(trxid_t trx_id) {
    TransactionInstance& instance = transaction_instances[trx_id];

    instance.state = ABORTING;
    trx_rollback(trx_id);
    flush_trx_log();
    release_trx_locks(instance);

    transaction_instances.erase(trx_id);
}

trxlogid_t log_update(tableid_t table_id, recordkey_t key,
                      const char* old_value, valsize_t old_val_size,
                      trxid_t trx_id) {
    pthread_mutex_lock(trx_manager_mutex);
    TransactionLog update_log;
    TransactionInstance& instance = transaction_instances[trx_id];

    if(instance.state != RUNNING) {
        pthread_mutex_unlock(trx_manager_mutex);
        return 0;
    }

    trxlogid_t log_id = ++accumulated_trxlog_id;

    update_log.type = UPDATE;
    update_log.table_id = table_id;
    update_log.key = key;
    memcpy(update_log.old_value, old_value, old_val_size);
    update_log.old_val_size = old_val_size;

    trx_logs[log_id] = update_log;

    // connect a trx log list.
    trx_logs[instance.log_tail].prev_trx_log = instance.log_tail;
    if (instance.log_tail == 0) {
        instance.log_tail = accumulated_trxlog_id;
    }

    pthread_mutex_unlock(trx_manager_mutex);
    return log_id;
}

}  // namespace trx_helper

int init_trx() {
    try {
        if (trx_manager_mutex != nullptr) {
            return -1;
        }
        trx_manager_mutex = new pthread_mutex_t;
        if (pthread_mutex_init(trx_manager_mutex, nullptr)) {
            return -1;
        }
        transaction_instances.clear();
    } catch (std::bad_alloc exception) {
        return -1;
    }
    return 0;
}

int cleanup_trx() {
    if (trx_manager_mutex) {
        pthread_mutex_destroy(trx_manager_mutex);
        delete trx_manager_mutex;
        trx_manager_mutex = nullptr;
    }

    transaction_instances.clear();
    trx_logs.clear();
    return 0;
}

trxid_t trx_begin() {
    pthread_mutex_lock(trx_manager_mutex);
    trxid_t created_trx = trx_helper::new_trx_instance();
    pthread_mutex_unlock(trx_manager_mutex);
    return created_trx;
}

trxid_t trx_commit(trxid_t trx_id) {
    pthread_mutex_lock(trx_manager_mutex);
    TransactionInstance& instance = transaction_instances[trx_id];

    if (!trx_helper::verify_trx(instance)) {
        pthread_mutex_unlock(trx_manager_mutex);
        return 0;
    }

    instance.state = COMMITTING;

    trxid_t current_log_id = instance.log_tail;
    while (current_log_id != 0) {
        trxid_t next_log_id = trx_logs[current_log_id].prev_trx_log;
        trx_logs.erase(current_log_id);
        current_log_id = next_log_id;
    }

    trx_helper::release_trx_locks(instance);
    trx_helper::flush_trx_log();
    instance.state = COMMITTED;

    transaction_instances.erase(trx_id);
    pthread_mutex_unlock(trx_manager_mutex);
    return trx_id;
}
```


-------------------------------

Updated on 2021-12-05 at 18:36:40 +0900
