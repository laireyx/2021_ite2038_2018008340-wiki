

# db/include/transaction.h



## Namespaces

| Name           |
| -------------- |
| **[trx_helper](/Namespaces/trx_helper)**  |

## Classes

|                | Name           |
| -------------- | -------------- |
| struct | **[TransactionLog](/Classes/TransactionLog)** <br>Transaction log.  |
| struct | **[TransactionInstance](/Classes/TransactionInstance)** <br>Transaction instance.  |

## Types

|                | Name           |
| -------------- | -------------- |
| enum| **[LogType](/Modules/DiskSpaceManager#enum-logtype)** { UPDATE = 0}<br>Transaction log type.  |
| enum| **[TransactionState](/Modules/DiskSpaceManager#enum-transactionstate)** { RUNNING = 0, WAITING = 1, COMMITTING = 2, COMMITTED = 3, ABORTING = 4, ABORTED = 5}<br>Transaction running state.  |
| typedef struct <a href="/Classes/TransactionLog">TransactionLog</a> | **[trxlog_t](/Modules/DiskSpaceManager#typedef-trxlog_t)**  |

## Functions

|                | Name           |
| -------------- | -------------- |
| int | **[init_trx](/Modules/DiskSpaceManager#function-init_trx)**()<br>Initialize a transaction manager.  |
| int | **[cleanup_trx](/Modules/DiskSpaceManager#function-cleanup_trx)**()<br>Cleanup a transaction manager.  |
| trxid_t | **[trx_begin](/Modules/DiskSpaceManager#function-trx_begin)**()<br>Begin a transaction.  |
| trxid_t | **[trx_commit](/Modules/DiskSpaceManager#function-trx_commit)**(trxid_t trx_id)<br>Commit a transaction.  |

## Types Documentation

### enum LogType

| Enumerator | Value | Description |
| ---------- | ----- | ----------- |
| UPDATE | 0|   |



Transaction log type. 

### enum TransactionState

| Enumerator | Value | Description |
| ---------- | ----- | ----------- |
| RUNNING | 0|   |
| WAITING | 1|   |
| COMMITTING | 2|   |
| COMMITTED | 3|   |
| ABORTING | 4|   |
| ABORTED | 5|   |



Transaction running state. 

### typedef trxlog_t

```cpp
typedef struct TransactionLog trxlog_t;
```



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



## Source code

```cpp

#pragma once

#include <lock.h>
#include <types.h>

#include <vector>

enum LogType { UPDATE = 0 };

struct TransactionLog {
    LogType type;
    tableid_t table_id;
    recordkey_t key;
    char* old_value;
    valsize_t old_val_size;
    trxlogid_t prev_trx_log;
};

enum TransactionState { RUNNING = 0, WAITING = 1, COMMITTING = 2, COMMITTED = 3, ABORTING = 4, ABORTED = 5 };

struct TransactionInstance {
    TransactionState state;
    trxlogid_t log_tail;
    Lock* lock_head;
    Lock* lock_tail;
};

typedef struct TransactionLog trxlog_t;

namespace trx_helper {

TransactionInstance& get_trx_instance(trxid_t trx_id);

lock_t* lock_acquire(int table_id, pagenum_t page_idx, int key_idx,
                   trxid_t trx_id, int lock_mode);
bool verify_trx(const TransactionInstance& instance);
trxid_t new_trx_instance();
void release_trx_locks(TransactionInstance& instance);
void trx_rollback(trxid_t trx_id);
void flush_trx_log();
void trx_abort(trxid_t trx_id);
trxlogid_t log_update(tableid_t table_id, recordkey_t key,
                      const char* old_value, valsize_t old_val_size,
                      trxid_t trx_id);
};  // namespace trx_helper

int init_trx();

int cleanup_trx();

trxid_t trx_begin();
trxid_t trx_commit(trxid_t trx_id);
```


-------------------------------

Updated on 2021-12-05 at 18:36:40 +0900
