

# db/src/db.cc



## Functions

|                | Name           |
| -------------- | -------------- |
| int | **[init_db](/Modules/DatabaseAPI#function-init_db)**(int num_buf =<a href="/Modules/BufferManager#variable-default-buffer-size">DEFAULT_BUFFER_SIZE</a>)<br>Initialize database management system.  |
| tableid_t | **[open_table](/Modules/DatabaseAPI#function-open_table)**(char * pathname)<br>Open existing data file using ‘pathname’ or create one if not existed.  |
| int | **[db_insert](/Modules/DatabaseAPI#function-db_insert)**(tableid_t table_id, recordkey_t key, char * value, valsize_t value_size)<br>Insert input (key, value) record with its size to data file at the right place.  |
| int | **[db_find](/Modules/DatabaseAPI#function-db_find)**(tableid_t table_id, recordkey_t key, char * ret_val, valsize_t * value_size)<br>Find the record corresponding the input key.  |
| int | **[db_find](/Modules/DatabaseAPI#function-db_find)**(tableid_t table_id, recordkey_t key, char * ret_val, valsize_t * value_size, trxid_t trx_id)<br>Find the record corresponding the input key in transaction.  |
| int | **[db_update](/Modules/DatabaseAPI#function-db_update)**(tableid_t table_id, recordkey_t key, char * value, valsize_t new_val_size, valsize_t * old_val_size, trxid_t trx_id)<br>Find the matching record and modify its value if found.  |
| int | **[db_delete](/Modules/DatabaseAPI#function-db_delete)**(tableid_t table_id, recordkey_t key)<br>Find the matching record and delete it if found.  |
| int | **[shutdown_db](/Modules/DatabaseAPI#function-shutdown_db)**()<br>Shutdown database management system.  |


## Functions Documentation

### function init_db

```cpp
int init_db(
    int num_buf =DEFAULT_BUFFER_SIZE
)
```

Initialize database management system. 

**Parameters**: 

  * **num_buf** Number of buffered pages. 


**Return**: If success, return 0. Otherwise return non-zero value. 

### function open_table

```cpp
tableid_t open_table(
    char * pathname
)
```

Open existing data file using ‘pathname’ or create one if not existed. 

**Parameters**: 

  * **pathname** Table file path. 


**Return**: unique table id which represents the own table in this database. return negative value otherwise. 

### function db_insert

```cpp
int db_insert(
    tableid_t table_id,
    recordkey_t key,
    char * value,
    valsize_t value_size
)
```

Insert input (key, value) record with its size to data file at the right place. 

**Parameters**: 

  * **table_id** table id obtained with <code><a href="/Modules/DatabaseAPI#function-open-table">open&#95;table()</a></code>. 
  * **key** record key. 
  * **value** record value. 
  * **value_size** record value size. 


**Return**: 0 if success. negative value otherwise. 

### function db_find

```cpp
int db_find(
    tableid_t table_id,
    recordkey_t key,
    char * ret_val,
    valsize_t * value_size
)
```

Find the record corresponding the input key. 

**Parameters**: 

  * **table_id** table id obtained with <code><a href="/Modules/DatabaseAPI#function-open-table">open&#95;table()</a></code>. 
  * **key** record key. 
  * **value** record value. 
  * **value_size** record value size. 


**Return**: 0 if success. negative value otherwise. 

If found, ret_val and value_size will be set to matching value and its size.


### function db_find

```cpp
int db_find(
    tableid_t table_id,
    recordkey_t key,
    char * ret_val,
    valsize_t * value_size,
    trxid_t trx_id
)
```

Find the record corresponding the input key in transaction. 

**Parameters**: 

  * **table_id** table id obtained with <code><a href="/Modules/DatabaseAPI#function-open-table">open&#95;table()</a></code>. 
  * **key** record key. 
  * **value** record value. 
  * **value_size** record value size. 
  * **trx_id** transaction id. 


**Return**: 0 if success. negative value otherwise. 

If found, ret_val and value_size will be set to matching value and its size.


### function db_update

```cpp
int db_update(
    tableid_t table_id,
    recordkey_t key,
    char * value,
    valsize_t new_val_size,
    valsize_t * old_val_size,
    trxid_t trx_id
)
```

Find the matching record and modify its value if found. 

**Parameters**: 

  * **table_id** table id obtained with <code><a href="/Modules/DatabaseAPI#function-open-table">open&#95;table()</a></code>. 
  * **key** record key. 
  * **value** new record value. 
  * **new_val_size** new record value size. 
  * **old_val_size** old record value size. 
  * **trx_id** transaction id. 


**Return**: 0 if success. negative value otherwise. 

### function db_delete

```cpp
int db_delete(
    tableid_t table_id,
    recordkey_t key
)
```

Find the matching record and delete it if found. 

**Parameters**: 

  * **table_id** table id obtained with <code><a href="/Modules/DatabaseAPI#function-open-table">open&#95;table()</a></code>. 
  * **key** record key. 


**Return**: 0 if success. negative value otherwise. 

### function shutdown_db

```cpp
int shutdown_db()
```

Shutdown database management system. 

**Return**: If success, return 0. Otherwise return non-zero value. 



## Source code

```cpp

#include <buffer.h>
#include <db.h>
#include <tree.h>
#include <lock.h>
#include <transaction.h>

#include <cstring>

int init_db(int num_buf) {
    if(init_lock_table() != 0) return -1;
    if(init_trx() != 0) return -1;
    if(init_buffer(num_buf) != 0) return -1;
    return 0;
}

tableid_t open_table(char* pathname) {
    return buffered_open_table_file(pathname);
}

int db_insert(tableid_t table_id, recordkey_t key, char* value,
              valsize_t value_size) {
    return insert_node(table_id, key, value, value_size) != 0 ? 0 : -1;
}

int db_find(tableid_t table_id, recordkey_t key, char* ret_val,
            valsize_t* value_size) {
    if (!find_by_key(table_id, key, ret_val, value_size, 0)) {
        return -1;
    }
    return 0;
}

int db_find(tableid_t table_id, recordkey_t key, char* ret_val,
            valsize_t* value_size, trxid_t trx_id) {
    if (!find_by_key(table_id, key, ret_val, value_size, trx_id)) {
        return -1;
    }
    return 0;
}

int db_update(tableid_t table_id, recordkey_t key, char* value,
              valsize_t new_val_size, valsize_t* old_val_size, trxid_t trx_id) {
    if (!update_node(table_id, key, value, new_val_size, old_val_size,
                     trx_id)) {
        return -1;
    }
    return 0;
}

int db_delete(tableid_t table_id, recordkey_t key) {
    if (!delete_node(table_id, key)) {
        return -1;
    }
    return 0;
}

int shutdown_db() {
    shutdown_buffer();
    cleanup_trx();
    cleanup_lock_table();
    return 0;
}
```


-------------------------------

Updated on 2021-12-05 at 22:45:19 +0900
