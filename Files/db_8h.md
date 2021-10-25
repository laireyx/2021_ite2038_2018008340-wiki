---
title: db/include/db.h

---

# db/include/db.h



## Functions

|                | Name           |
| -------------- | -------------- |
| int | **[init_db](/Modules/group__DatabaseAPI#function-init-db)**(int num_buf =1024)<br>Initialize database management system.  |
| tableid_t | **[open_table](/Modules/group__DatabaseAPI#function-open-table)**(char * pathname)<br>Open existing data file using ‘pathname’ or create one if not existed.  |
| int | **[db_insert](/Modules/group__DatabaseAPI#function-db-insert)**(tableid_t table_id, int64_t key, char * value, uint16_t value_size)<br>Insert input (key, value) record with its size to data file at the right place.  |
| int | **[db_find](/Modules/group__DatabaseAPI#function-db-find)**(tableid_t table_id, int64_t key, char * ret_val, uint16_t * value_size)<br>Find the record corresponding the input key.  |
| int | **[db_delete](/Modules/group__DatabaseAPI#function-db-delete)**(tableid_t table_id, int64_t key)<br>Find the matching record and delete it if found.  |
| int | **[shutdown_db](/Modules/group__DatabaseAPI#function-shutdown-db)**()<br>Shutdown database management system.  |


## Functions Documentation

### function init_db

```cpp
int init_db(
    int num_buf =1024
)
```

Initialize database management system. 

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
    int64_t key,
    char * value,
    uint16_t value_size
)
```

Insert input (key, value) record with its size to data file at the right place. 

**Parameters**: 

  * **table_id** table id obtained with <code><a href="/Modules/group__DatabaseAPI#function-open-table">open&#95;table()</a></code>. 
  * **key** record key. 
  * **value** record value. 
  * **value_size** record value size. 


**Return**: 0 if success. negative value otherwise. 

### function db_find

```cpp
int db_find(
    tableid_t table_id,
    int64_t key,
    char * ret_val,
    uint16_t * value_size
)
```

Find the record corresponding the input key. 

**Parameters**: 

  * **table_id** table id obtained with <code><a href="/Modules/group__DatabaseAPI#function-open-table">open&#95;table()</a></code>. 
  * **key** record key. 
  * **value** record value. 
  * **value_size** record value size. 


**Return**: 0 if success. negative value otherwise. 

If found, ret_val and value_size will be set to matching value and its size.


### function db_delete

```cpp
int db_delete(
    tableid_t table_id,
    int64_t key
)
```

Find the matching record and delete it if found. 

**Parameters**: 

  * **table_id** table id obtained with <code><a href="/Modules/group__DatabaseAPI#function-open-table">open&#95;table()</a></code>. 
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

#pragma once

#include "types.h"

int init_db(int num_buf = 1024);

tableid_t open_table(char* pathname);

int db_insert(tableid_t table_id, int64_t key, char* value,
              uint16_t value_size);

int db_find(tableid_t table_id, int64_t key, char* ret_val,
            uint16_t* value_size);

int db_delete(tableid_t table_id, int64_t key);

int shutdown_db();
```


-------------------------------

Updated on 2021-10-25 at 16:53:02 +0900
