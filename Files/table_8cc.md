---
title: db/src/table.cc

---

# db/src/table.cc



## Functions

|                | Name           |
| -------------- | -------------- |
| int | **[init_db](/Modules/group__TableManager#function-init-db)**()<br>Initialize database management system.  |
| tableid_t | **[open_table](/Modules/group__TableManager#function-open-table)**(char * pathname)<br>Open existing data file using ‘pathname’ or create one if not existed.  |
| int | **[db_insert](/Modules/group__TableManager#function-db-insert)**(tableid_t table_id, int64_t key, char * value, uint16_t value_size)<br>Insert input (key, value) record with its size to data file at the right place.  |
| int | **[db_find](/Modules/group__TableManager#function-db-find)**(tableid_t table_id, int64_t key, char * ret_val, uint16_t * value_size)<br>Find the record corresponding the input key.  |
| int | **[db_delete](/Modules/group__TableManager#function-db-delete)**(tableid_t table_id, int64_t key)<br>Find the matching record and delete it if found.  |
| int | **[shutdown_db](/Modules/group__TableManager#function-shutdown-db)**()<br>Shutdown database management system.  |


## Functions Documentation

### function init_db

```cpp
int init_db()
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

  * **table_id** Table id obtained with <code><a href="/Modules/group__TableManager#function-open-table">open&#95;table()</a></code>. 
  * **key** Record key. 
  * **value** Record value. 
  * **value_size** Record value size. 


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

  * **table_id** Table id obtained with <code><a href="/Modules/group__TableManager#function-open-table">open&#95;table()</a></code>. 
  * **key** Record key. 
  * **value** Record value. 
  * **value_size** Record value size. 


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

  * **table_id** Table id obtained with <code><a href="/Modules/group__TableManager#function-open-table">open&#95;table()</a></code>. 
  * **key** Record key. 


**Return**: 0 if success. negative value otherwise. 

### function shutdown_db

```cpp
int shutdown_db()
```

Shutdown database management system. 

**Return**: If success, return 0. Otherwise return non-zero value. 



## Source code

```cpp

#include "table.h"

#include "file.h"
#include "tree.h"

#include <cstring>

int init_db() { return 0; }

tableid_t open_table(char* pathname) {
    return file_open_table_file(pathname);
}

int db_insert(tableid_t table_id, int64_t key, char* value,
              uint16_t value_size) {
    return insert_node(table_id, key, value, value_size) != 0 ? 0 : -1;
}

int db_find(tableid_t table_id, int64_t key, char* ret_val,
            uint16_t* value_size) {
    if (!find_by_key(table_id, key, ret_val, value_size)) {
        return -1;
    }
    return 0;
}

int db_delete(tableid_t table_id, int64_t key) {
    if (!delete_node(table_id, key)) {
        return -1;
    }
    return 0;
}

int shutdown_db() {
    file_close_table_files();
    return 0;
}
```


-------------------------------

Updated on 2021-10-25 at 17:06:26 +0900
