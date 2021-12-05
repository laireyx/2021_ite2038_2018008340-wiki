

# trx_helper

**Module:** **[TransactionManager](/Modules/TransactionManager)**



## Functions

|                | Name           |
| -------------- | -------------- |
| <a href="/Classes/TransactionInstance">TransactionInstance</a> & | **[get_trx_instance](/Namespaces/trx_helper#function-get_trx_instance)**(trxid_t trx_id)<br>Get the trx instance object.  |
| <a href="/Classes/lock_t">lock_t</a> * | **[lock_acquire](/Namespaces/trx_helper#function-lock_acquire)**(int table_id, pagenum_t page_idx, int key_idx, trxid_t trx_id, int lock_mode)<br>Wrapper for <code>lock&#95;acquire()</code> |
| bool | **[verify_trx](/Namespaces/trx_helper#function-verify_trx)**(const <a href="/Classes/TransactionInstance">TransactionInstance</a> & instance)<br>Verify if transaction is on good state.  |
| trxid_t | **[new_trx_instance](/Namespaces/trx_helper#function-new_trx_instance)**()<br>Instantiate a transaction and return its id.  |
| void | **[release_trx_locks](/Namespaces/trx_helper#function-release_trx_locks)**(<a href="/Classes/TransactionInstance">TransactionInstance</a> & instance)<br>Release all the locks in the instance.  |
| void | **[trx_rollback](/Namespaces/trx_helper#function-trx_rollback)**(trxid_t trx_id)<br>Rollback an unfinished transaction and finish it.  |
| void | **[flush_trx_log](/Namespaces/trx_helper#function-flush_trx_log)**()<br>Flush all the transaction log(for recovery.)  |
| void | **[trx_abort](/Namespaces/trx_helper#function-trx_abort)**(trxid_t trx_id)<br>Immediately abort a transaction and release all of its locks.  |
| trxlogid_t | **[log_update](/Namespaces/trx_helper#function-log_update)**(tableid_t table_id, recordkey_t key, const char * old_value, valsize_t old_val_size, trxid_t trx_id)<br>Log an update query into transaction log.  |


## Functions Documentation

### function get_trx_instance

```cpp
TransactionInstance & get_trx_instance(
    trxid_t trx_id
)
```

Get the trx instance object. 

**Parameters**: 

  * **trx_id** transaction id. 


**Return**: transaction instance. 

Quickfix. Expects dragon ahead.


### function lock_acquire

```cpp
lock_t * lock_acquire(
    int table_id,
    pagenum_t page_idx,
    int key_idx,
    trxid_t trx_id,
    int lock_mode
)
```

Wrapper for <code>lock&#95;acquire()</code>

**Parameters**: 

  * **table_id** table id. 
  * **page_idx** page index. 
  * **key** record key index. 
  * **trx_id** transaction id. 
  * **lock_mode** lock mode. 


**Return**: acquired lock. 

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
    TransactionInstance & instance
)
```

Release all the locks in the instance. 

**Parameters**: 

  * **instance** transaction instance. 


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

Updated on 2021-12-05 at 18:37:58 +0900