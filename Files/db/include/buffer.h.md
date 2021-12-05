

# db/include/buffer.h



## Namespaces

| Name           |
| -------------- |
| **[buffer_helper](/Namespaces/buffer_helper)** <br>BufferManager helper.  |

## Classes

|                | Name           |
| -------------- | -------------- |
| struct | **[BufferBlock](/Classes/BufferBlock)**  |

## Types

|                | Name           |
| -------------- | -------------- |
| typedef struct <a href="/Classes/BufferBlock">BufferBlock</a> | **[BufferBlock](/Modules/BufferManager#typedef-bufferblock)**  |

## Functions

|                | Name           |
| -------------- | -------------- |
| int | **[init_buffer](/Modules/BufferManager#function-init_buffer)**(int buffer_size)<br>Initialize buffer manager.  |
| tableid_t | **[buffered_open_table_file](/Modules/BufferManager#function-buffered_open_table_file)**(const char * path)<br>Open existing table file or create one if not existed.  |
| pagenum_t | **[buffered_alloc_page](/Modules/BufferManager#function-buffered_alloc_page)**(tableid_t table_id, trxid_t trx_id =0)<br>Allocate an on-disk page from the free page list.  |
| void | **[buffered_free_page](/Modules/BufferManager#function-buffered_free_page)**(tableid_t table_id, pagenum_t pagenum, trxid_t trx_id =0)<br>Free an on-disk page to the free page list.  |
| void | **[buffered_read_page](/Modules/BufferManager#function-buffered_read_page)**(tableid_t table_id, pagenum_t pagenum, <a href="/Modules/DiskSpaceManager#typedef-page-t">page_t</a> * dest, trxid_t trx_id =0, bool pin =true)<br>Read an on-disk page into the in-memory page structure(dest)  |
| void | **[buffered_write_page](/Modules/BufferManager#function-buffered_write_page)**(tableid_t table_id, pagenum_t pagenum, const <a href="/Modules/DiskSpaceManager#typedef-page-t">page_t</a> * src)<br>Write an in-memory page(src) to the on-disk page.  |
| void | **[buffered_release_page](/Modules/BufferManager#function-buffered_release_page)**(tableid_t table_id, pagenum_t pagenum)<br>Releases an in-memory buffer.  |
| int | **[shutdown_buffer](/Modules/BufferManager#function-shutdown_buffer)**()<br>Shutdown buffer manager.  |

## Types Documentation

### typedef BufferBlock

```cpp
typedef struct BufferBlock BufferBlock;
```



## Functions Documentation

### function init_buffer

```cpp
int init_buffer(
    int buffer_size
)
```

Initialize buffer manager. 

**Parameters**: 

  * **buffer_size** Buffer size. 


**Return**: <code>0</code> if success, non-zero value otherwise. 

### function buffered_open_table_file

```cpp
tableid_t buffered_open_table_file(
    const char * path
)
```

Open existing table file or create one if not existed. 

**Parameters**: 

  * **path** Table file path. 


**Return**: ID of the opened table file. 

### function buffered_alloc_page

```cpp
pagenum_t buffered_alloc_page(
    tableid_t table_id,
    trxid_t trx_id =0
)
```

Allocate an on-disk page from the free page list. 

**Parameters**: 

  * **table_id** table id obtained with <code><a href="/Modules/BufferManager#function-buffered-open-table-file">buffered&#95;open&#95;table&#95;file()</a></code>. 
  * **trx_id** transaction id. 


**Return**: >0 <a href="/Classes/Page">Page</a> index number if allocation success. 0 Zero if allocation failed. 

### function buffered_free_page

```cpp
void buffered_free_page(
    tableid_t table_id,
    pagenum_t pagenum,
    trxid_t trx_id =0
)
```

Free an on-disk page to the free page list. 

**Parameters**: 

  * **table_id** table id obtained with <code><a href="/Modules/BufferManager#function-buffered-open-table-file">buffered&#95;open&#95;table&#95;file()</a></code>. 
  * **trx_id** transaction id. 
  * **pagenum** page index. 


### function buffered_read_page

```cpp
void buffered_read_page(
    tableid_t table_id,
    pagenum_t pagenum,
    page_t * dest,
    trxid_t trx_id =0,
    bool pin =true
)
```

Read an on-disk page into the in-memory page structure(dest) 

**Parameters**: 

  * **table_id** table id obtained with <code><a href="/Modules/BufferManager#function-buffered-open-table-file">buffered&#95;open&#95;table&#95;file()</a></code>. 
  * **pagenum** page index. 
  * **dest** the pointer of the page data. 
  * **trx_id** transaction id. 
  * **pin** <code>true</code> if this buffer will be writed after. 


### function buffered_write_page

```cpp
void buffered_write_page(
    tableid_t table_id,
    pagenum_t pagenum,
    const page_t * src
)
```

Write an in-memory page(src) to the on-disk page. 

**Parameters**: 

  * **table_id** table id obtained with <code><a href="/Modules/BufferManager#function-buffered-open-table-file">buffered&#95;open&#95;table&#95;file()</a></code>. 
  * **pagenum** page index. 
  * **src** the pointer of the page data. 


### function buffered_release_page

```cpp
void buffered_release_page(
    tableid_t table_id,
    pagenum_t pagenum
)
```

Releases an in-memory buffer. 

**Parameters**: 

  * **table_id** table id obtained with <code><a href="/Modules/BufferManager#function-buffered-open-table-file">buffered&#95;open&#95;table&#95;file()</a></code>. 
  * **pagenum** page index. 


In case of conditional writing, instead of using R(read without pin) - R(read with pin) - W(write to clean pin) method, Just clearing pin without write any data is needed.


### function shutdown_buffer

```cpp
int shutdown_buffer()
```

Shutdown buffer manager. 

**Return**: <code>0</code> if success, non-zero value otherwise. 



## Source code

```cpp

#pragma once

#include <page.h>
#include <pthread.h>
#include <types.h>

typedef struct BufferBlock {
    fullpage_t page;
    PageLocation page_location;

    pthread_cond_t latch;
    trxid_t pin_owner;
    int pin_count;

    bool is_dirty;

    int prev_block_idx;
    int next_block_idx;
} BufferBlock;

namespace buffer_helper {
BufferBlock* load_buffer(tableid_t table_id, pagenum_t pagenum, page_t* page,
                         trxid_t trx_id = 0, bool pin = true);
bool apply_buffer(tableid_t table_id, pagenum_t pagenum, const page_t* page);
void release_buffer(tableid_t table_id, pagenum_t pagenum);
bool is_full();
int evict();
void move_to_head(int buffer_idx);
}  // namespace buffer_helper

int init_buffer(int buffer_size);

tableid_t buffered_open_table_file(const char* path);

pagenum_t buffered_alloc_page(tableid_t table_id, trxid_t trx_id = 0);

void buffered_free_page(tableid_t table_id, pagenum_t pagenum,
                        trxid_t trx_id = 0);

void buffered_read_page(tableid_t table_id, pagenum_t pagenum, page_t* dest,
                        trxid_t trx_id = 0, bool pin = true);

void buffered_write_page(tableid_t table_id, pagenum_t pagenum,
                         const page_t* src);

void buffered_release_page(tableid_t table_id, pagenum_t pagenum);

int shutdown_buffer();
```


-------------------------------

Updated on 2021-12-05 at 22:45:19 +0900
