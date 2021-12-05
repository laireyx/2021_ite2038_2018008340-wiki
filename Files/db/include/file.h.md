

# db/include/file.h



## Namespaces

| Name           |
| -------------- |
| **[file_helper](/Namespaces/file_helper)** <br>Filemanager helper.  |

## Classes

|                | Name           |
| -------------- | -------------- |
| class | **[TableInstance](/Classes/TableInstance)** <br>Table file instance.  |

## Types

|                | Name           |
| -------------- | -------------- |
| typedef struct <a href="/Classes/TableInstance">TableInstance</a> | **[TableInstance](/Modules/DiskSpaceManager#typedef-tableinstance)**  |

## Functions

|                | Name           |
| -------------- | -------------- |
| tableid_t | **[file_open_table_file](/Modules/DiskSpaceManager#function-file_open_table_file)**(const char * path)<br>Open existing table file or create one if not existed.  |
| pagenum_t | **[file_alloc_page](/Modules/DiskSpaceManager#function-file_alloc_page)**(tableid_t table_id)<br>Allocate an on-disk page from the free page list.  |
| void | **[file_free_page](/Modules/DiskSpaceManager#function-file_free_page)**(tableid_t table_id, pagenum_t pagenum)<br>Free an on-disk page to the free page list.  |
| void | **[file_read_page](/Modules/DiskSpaceManager#function-file_read_page)**(tableid_t table_id, pagenum_t pagenum, <a href="/Modules/DiskSpaceManager#typedef-page-t">page_t</a> * dest)<br>Read an on-disk page into the in-memory page structure(dest)  |
| void | **[file_write_page](/Modules/DiskSpaceManager#function-file_write_page)**(tableid_t table_id, pagenum_t pagenum, const <a href="/Modules/DiskSpaceManager#typedef-page-t">page_t</a> * src)<br>Write an in-memory page(src) to the on-disk page.  |
| void | **[file_close_table_files](/Modules/DiskSpaceManager#function-file_close_table_files)**()<br>Stop referencing the table files.  |

## Types Documentation

### typedef TableInstance

```cpp
typedef struct TableInstance TableInstance;
```



## Functions Documentation

### function file_open_table_file

```cpp
tableid_t file_open_table_file(
    const char * path
)
```

Open existing table file or create one if not existed. 

**Parameters**: 

  * **path** Table file path. 


**Return**: ID of the opened table file. 

### function file_alloc_page

```cpp
pagenum_t file_alloc_page(
    tableid_t table_id
)
```

Allocate an on-disk page from the free page list. 

**Parameters**: 

  * **table_id** table id obtained with <code><a href="/Modules/DiskSpaceManager#function-file-open-table-file">file&#95;open&#95;table&#95;file()</a></code>. 


**Return**: >0 <a href="/Classes/Page">Page</a> index number if allocation success. 0 Zero if allocation failed. 

### function file_free_page

```cpp
void file_free_page(
    tableid_t table_id,
    pagenum_t pagenum
)
```

Free an on-disk page to the free page list. 

**Parameters**: 

  * **table_id** table id obtained with <code><a href="/Modules/DiskSpaceManager#function-file-open-table-file">file&#95;open&#95;table&#95;file()</a></code>. 
  * **pagenum** page index. 


### function file_read_page

```cpp
void file_read_page(
    tableid_t table_id,
    pagenum_t pagenum,
    page_t * dest
)
```

Read an on-disk page into the in-memory page structure(dest) 

**Parameters**: 

  * **table_id** table id obtained with <code><a href="/Modules/DiskSpaceManager#function-file-open-table-file">file&#95;open&#95;table&#95;file()</a></code>. 
  * **pagenum** page index. 
  * **dest** the pointer of the page data. 


### function file_write_page

```cpp
void file_write_page(
    tableid_t table_id,
    pagenum_t pagenum,
    const page_t * src
)
```

Write an in-memory page(src) to the on-disk page. 

**Parameters**: 

  * **table_id** table id obtained with <code><a href="/Modules/DiskSpaceManager#function-file-open-table-file">file&#95;open&#95;table&#95;file()</a></code>. 
  * **pagenum** page index. 
  * **src** the pointer of the page data. 


### function file_close_table_files

```cpp
void file_close_table_files()
```

Stop referencing the table files. 



## Source code

```cpp

#pragma once

#include <const.h>
#include <page.h>
#include <types.h>

typedef struct TableInstance {
    char* file_path;
    int file_descriptor;
} TableInstance;

namespace file_helper {
TableInstance& get_table_instance(tableid_t table_id);
void extend_capacity(tableid_t table_id, pagenum_t newsize = 0);

void flush_header(tableid_t table_id, headerpage_t* header_page);
};  // namespace file_helper

tableid_t file_open_table_file(const char* path);

pagenum_t file_alloc_page(tableid_t table_id);

void file_free_page(tableid_t table_id, pagenum_t pagenum);

void file_read_page(tableid_t table_id, pagenum_t pagenum, page_t* dest);

void file_write_page(tableid_t table_id, pagenum_t pagenum, const page_t* src);

void file_close_table_files();
```


-------------------------------

Updated on 2021-12-05 at 22:45:19 +0900
