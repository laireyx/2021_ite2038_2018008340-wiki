

# TransactionManager



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
| enum| **[LogType](/Modules/TransactionManager#enum-logtype)** { UPDATE = 0}<br>Transaction log type.  |
| enum| **[TransactionState](/Modules/TransactionManager#enum-transactionstate)** { RUNNING = 0, COMMITTING = 1, ABORTING = 2}<br>Transaction running state.  |
| typedef struct <a href="/Classes/TransactionLog">TransactionLog</a> | **[trxlog_t](/Modules/TransactionManager#typedef-trxlog_t)**  |

## Functions

|                | Name           |
| -------------- | -------------- |
| int | **[init_trx](/Modules/TransactionManager#function-init_trx)**()<br>Initialize a transaction manager.  |
| int | **[cleanup_trx](/Modules/TransactionManager#function-cleanup_trx)**()<br>Cleanup a transaction manager.  |
| trxid_t | **[trx_begin](/Modules/TransactionManager#function-trx_begin)**()<br>Begin a transaction.  |
| trxid_t | **[trx_commit](/Modules/TransactionManager#function-trx_commit)**(trxid_t trx_id)<br>Commit a transaction.  |

## Attributes

|                | Name           |
| -------------- | -------------- |
| pthread_mutex_t * | **[trx_manager_mutex](/Modules/TransactionManager#variable-trx_manager_mutex)** <br>Transaction manager mutex.  |
| trxid_t | **[accumulated_trx_id](/Modules/TransactionManager#variable-accumulated_trx_id)** <br>Accumulated trx id.  |
| trxid_t | **[accumulated_trxlog_id](/Modules/TransactionManager#variable-accumulated_trxlog_id)** <br>Accumulated trx log id.  |
| std::unordered_map< trxid_t, <a href="/Classes/TransactionInstance">TransactionInstance</a> > | **[transaction_instances](/Modules/TransactionManager#variable-transaction_instances)** <br>Transaction instances.  |
| std::unordered_map< trxlogid_t, <a href="/Classes/TransactionLog">TransactionLog</a> > | **[trx_logs](/Modules/TransactionManager#variable-trx_logs)** <br>Transaction log.  |

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
| COMMITTING | 1|   |
| ABORTING | 2|   |



Transaction running state. 

### typedef trxlog_t

```
typedef struct TransactionLog trxlog_t;
```



## Functions Documentation

### function init_trx

```
int init_trx()
```

Initialize a transaction manager. 

**Return**: <code>0</code> if success, negative value otherwise. 

### function cleanup_trx

```
int cleanup_trx()
```

Cleanup a transaction manager. 

**Return**: <code>0</code> if success, negative value otherwise. 

### function trx_begin

```
trxid_t trx_begin()
```

Begin a transaction. 

**Return**: positive transaction id if success. <code>0</code> or negative otherwise. 

### function trx_commit

```
trxid_t trx_commit(
    trxid_t trx_id
)
```

Commit a transaction. 

**Parameters**: 

  * **trx_id** transaction id obtained with <code><a href="/Modules/TransactionManager#function-trx-begin">trx&#95;begin()</a></code>. 


**Return**: <code>trx&#95;id</code>(committed transaction id) if success. <code>0</code> otherwise. 


## Attributes Documentation

### variable trx_manager_mutex

```
pthread_mutex_t * trx_manager_mutex = nullptr;
```

Transaction manager mutex. 

### variable accumulated_trx_id

```
trxid_t accumulated_trx_id = 0;
```

Accumulated trx id. 

### variable accumulated_trxlog_id

```
trxid_t accumulated_trxlog_id = 0;
```

Accumulated trx log id. 

### variable transaction_instances

```
std::unordered_map< trxid_t, TransactionInstance > transaction_instances;
```

Transaction instances. 

### variable trx_logs

```
std::unordered_map< trxlogid_t, TransactionLog > trx_logs;
```

Transaction log. 




-------------------------------

Updated on 2021-12-05 at 22:45:19 +0900