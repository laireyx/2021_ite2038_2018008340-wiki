

# trx_helper

**Module:** **[TransactionManager](/Modules/TransactionManager)**



## Functions

|                | Name           |
| -------------- | -------------- |
| bool | **[connect_lock_tail](/Namespaces/trx_helper#function-connect_lock_tail)**(trxid_t trx_id, <a href="/Classes/lock_t">lock_t</a> * lock)<br>Connect a lock to the tail of next-transaction-lock list.  |
| bool | **[is_trx_running](/Namespaces/trx_helper#function-is_trx_running)**(trxid_t trx_id)<br>Check if target transaction is running.  |
| bool | **[verify_trx](/Namespaces/trx_helper#function-verify_trx)**(const <a href="/Classes/TransactionInstance">TransactionInstance</a> & instance)<br>Verify if transaction is on good state.  |
| trxid_t | **[new_trx_instance](/Namespaces/trx_helper#function-new_trx_instance)**()<br>Instantiate a transaction and return its id.  |
| void | **[release_trx_locks](/Namespaces/trx_helper#function-release_trx_locks)**(trxid_t trx_id)<br>Release all the locks in the instance.  |
| void | **[trx_rollback](/Namespaces/trx_helper#function-trx_rollback)**(trxid_t trx_id)<br>Rollback an unfinished transaction and finish it.  |
| void | **[flush_trx_log](/Namespaces/trx_helper#function-flush_trx_log)**()<br>Flush all the transaction log(for recovery.)  |
| void | **[trx_abort](/Namespaces/trx_helper#function-trx_abort)**(trxid_t trx_id)<br>Immediately abort a transaction and release all of its locks.  |
| trxlogid_t | **[log_update](/Namespaces/trx_helper#function-log_update)**(tableid_t table_id, recordkey_t key, const char * old_value, valsize_t old_val_size, trxid_t trx_id)<br>Log an update query into transaction log.  |


## Functions Documentation

### function connect_lock_tail

```cpp
bool connect_lock_tail(
    trxid_t trx_id,
    lock_t * lock
)
```

Connect a lock to the tail of next-transaction-lock list. 

**Parameters**: 

  * **trx_id** transaction id. 
  * **lock** lock object. 


**Return**: <code>true</code> if success, <code>false</code> if otherwise. 

### function is_trx_running

```cpp
bool is_trx_running(
    trxid_t trx_id
)
```

Check if target transaction is running. 

**Parameters**: 

  * **trx_id** transaction id. 


**Return**: <code>true</code> if transaction is running. 

RUNNING, COMMITTING, ABORTING transaction is running transaction.


### function verify_trx

```cpp
bool verify_trx(
    const TransactionInstance & instance
)
```

Verify if transaction is on good state. 

**Parameters**: 

  * **instance** transaction instance. 


**Return**: <code>true</code> if instance is good to go. <code>false</code> otherwise. 

check if the transaction is already finished(by commit or abort).


### function new_trx_instance

```cpp
trxid_t new_trx_instance()
```

Instantiate a transaction and return its id. 

**Return**: transaction id. 

### function release_trx_locks

```cpp
void release_trx_locks(
    trxid_t trx_id
)
```

Release all the locks in the instance. 

**Parameters**: 

  * **trx_id** transaction id. 


### function trx_rollback

```cpp
void trx_rollback(
    trxid_t trx_id
)
```

Rollback an unfinished transaction and finish it. 

**Parameters**: 

  * **trx_id** transaction id. 


### function flush_trx_log

```cpp
void flush_trx_log()
```

Flush all the transaction log(for recovery.) 

Not used for this time(maybe for project6). 


Todotodo on recovery. 

Todotodo on recovery. 


### function trx_abort

```cpp
void trx_abort(
    trxid_t trx_id
)
```

Immediately abort a transaction and release all of its locks. 

**Parameters**: 

  * **trx_id** transaciton id obtained with <code><a href="/Modules/TransactionManager#function-trx-begin">trx&#95;begin()</a></code>. 


### function log_update

```cpp
trxlogid_t log_update(
    tableid_t table_id,
    recordkey_t key,
    const char * old_value,
    valsize_t old_val_size,
    trxid_t trx_id
)
```

Log an update query into transaction log. 

**Parameters**: 

  * **table_id** table id. 
  * **key** record key. 
  * **old_value** old record value. 
  * **old_val_size** old value size. 
  * **trx_id** transaction id. 


**Return**: created log id. 





-------------------------------

Updated on 2021-12-05 at 22:45:19 +0900