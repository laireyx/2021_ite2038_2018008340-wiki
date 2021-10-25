

# DatabaseAPI



## Functions

|                | Name           |
| -------------- | -------------- |
| int | **[init_db](/Modules/DatabaseAPI#function-init_db)**(int num_buf =1024)<br>Initialize database management system.  |
| tableid_t | **[open_table](/Modules/DatabaseAPI#function-open_table)**(char * pathname)<br>Open existing data file using ‘pathname’ or create one if not existed.  |
| int | **[db_insert](/Modules/DatabaseAPI#function-db_insert)**(tableid_t table_id, int64_t key, char * value, uint16_t value_size)<br>Insert input (key, value) record with its size to data file at the right place.  |
| int | **[db_find](/Modules/DatabaseAPI#function-db_find)**(tableid_t table_id, int64_t key, char * ret_val, uint16_t * value_size)<br>Find the record corresponding the input key.  |
| int | **[db_delete](/Modules/DatabaseAPI#function-db_delete)**(tableid_t table_id, int64_t key)<br>Find the matching record and delete it if found.  |
| int | **[shutdown_db](/Modules/DatabaseAPI#function-shutdown_db)**()<br>Shutdown database management system.  |


## Functions Documentation

### function init_db

```
int init_db(
    int num_buf =1024
)
```

Initialize database management system. 

**Return**: If success, return 0. Otherwise return non-zero value. 

### function open_table

```
tableid_t open_table(
    char * pathname
)
```

Open existing data file using ‘pathname’ or create one if not existed. 

**Parameters**: 

  * **pathname** Table file path. 


**Return**: unique table id which represents the own table in this database. return negative value otherwise. 

### function db_insert

```
int db_insert(
    tableid_t table_id,
    int64_t key,
    char * value,
    uint16_t value_size
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

```
int db_find(
    tableid_t table_id,
    int64_t key,
    char * ret_val,
    uint16_t * value_size
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


### function db_delete

```
int db_delete(
    tableid_t table_id,
    int64_t key
)
```

Find the matching record and delete it if found. 

**Parameters**: 

  * **table_id** table id obtained with <code><a href="/Modules/DatabaseAPI#function-open-table">open&#95;table()</a></code>. 
  * **key** record key. 


**Return**: 0 if success. negative value otherwise. 

### function shutdown_db

```
int shutdown_db()
```

Shutdown database management system. 

**Return**: If success, return 0. Otherwise return non-zero value. 





-------------------------------

Updated on 2021-10-25 at 17:08:33 +0900