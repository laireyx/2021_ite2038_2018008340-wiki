---
title: BufferManager

---

# BufferManager



## Namespaces

| Name           |
| -------------- |
| **[std](/Namespaces/namespacestd)**  |
| **[buffer_helper](/Namespaces/namespacebuffer__helper)** <br>BufferManager helper.  |

## Classes

|                | Name           |
| -------------- | -------------- |
| struct | **[BufferBlock](/Classes/structBufferBlock)**  |

## Types

|                | Name           |
| -------------- | -------------- |
| typedef std::pair< tableid_t, pagenum_t > | **[PageLocation](/Modules/group__BufferManager#typedef-pagelocation)**  |
| typedef struct <a href="/Classes/structBufferBlock">BufferBlock</a> | **[BufferBlock](/Modules/group__BufferManager#typedef-bufferblock)**  |

## Functions

|                | Name           |
| -------------- | -------------- |
| int | **[init_buffer](/Modules/group__BufferManager#function-init-buffer)**(int buffer_size)<br>Initialize buffer manager.  |
| tableid_t | **[buffered_open_table_file](/Modules/group__BufferManager#function-buffered-open-table-file)**(const char * path)<br>Open existing table file or create one if not existed.  |
| pagenum_t | **[buffered_alloc_page](/Modules/group__BufferManager#function-buffered-alloc-page)**(tableid_t table_id)<br>Allocate an on-disk page from the free page list.  |
| void | **[buffered_free_page](/Modules/group__BufferManager#function-buffered-free-page)**(tableid_t table_id, pagenum_t pagenum)<br>Free an on-disk page to the free page list.  |
| void | **[buffered_read_page](/Modules/group__BufferManager#function-buffered-read-page)**(tableid_t table_id, pagenum_t pagenum, <a href="/Modules/group__DiskSpaceManager#typedef-page-t">page_t</a> * dest, bool pin =true)<br>Read an on-disk page into the in-memory page structure(dest)  |
| void | **[buffered_write_page](/Modules/group__BufferManager#function-buffered-write-page)**(tableid_t table_id, pagenum_t pagenum, const <a href="/Modules/group__DiskSpaceManager#typedef-page-t">page_t</a> * src)<br>Write an in-memory page(src) to the on-disk page.  |
| int | **[shutdown_buffer](/Modules/group__BufferManager#function-shutdown-buffer)**()<br>Shutdown buffer manager.  |

## Attributes

|                | Name           |
| -------------- | -------------- |
| <a href="/Classes/structBufferBlock">BufferBlock</a> * | **[buffer_slot](/Modules/group__BufferManager#variable-buffer-slot)** <br>buffer slot.  |
| int | **[buffer_head_idx](/Modules/group__BufferManager#variable-buffer-head-idx)** <br>head index of Recently-Used list.  |
| int | **[buffer_tail_idx](/Modules/group__BufferManager#variable-buffer-tail-idx)** <br>tail index of Recently-Used list.  |
| int | **[buffer_size](/Modules/group__BufferManager#variable-buffer-size)** <br>size of buffer block.  |
| std::unordered_map< <a href="/Modules/group__BufferManager#typedef-pagelocation">PageLocation</a>, int > | **[buffer_index](/Modules/group__BufferManager#variable-buffer-index)**  |

## Types Documentation

### typedef PageLocation

```
typedef std::pair<tableid_t, pagenum_t> PageLocation;
```


### typedef BufferBlock

```
typedef struct BufferBlock BufferBlock;
```



## Functions Documentation

### function init_buffer

```
int init_buffer(
    int buffer_size
)
```

Initialize buffer manager. 

**Parameters**: 

  * **buffer_size** Buffer size. 


**Return**: <code>0</code> if success, non-zero value otherwise. 

### function buffered_open_table_file

```
tableid_t buffered_open_table_file(
    const char * path
)
```

Open existing table file or create one if not existed. 

**Parameters**: 

  * **path** Table file path. 


**Return**: ID of the opened table file. 

### function buffered_alloc_page

```
pagenum_t buffered_alloc_page(
    tableid_t table_id
)
```

Allocate an on-disk page from the free page list. 

**Parameters**: 

  * **table_id** table id obtained with <code><a href="/Modules/group__BufferManager#function-buffered-open-table-file">buffered&#95;open&#95;table&#95;file()</a></code>. 


**Return**: >0 <a href="/Classes/structPage">Page</a> index number if allocation success. 0 Zero if allocation failed. 

### function buffered_free_page

```
void buffered_free_page(
    tableid_t table_id,
    pagenum_t pagenum
)
```

Free an on-disk page to the free page list. 

**Parameters**: 

  * **table_id** table id obtained with <code><a href="/Modules/group__BufferManager#function-buffered-open-table-file">buffered&#95;open&#95;table&#95;file()</a></code>. 
  * **pagenum** page index. 


### function buffered_read_page

```
void buffered_read_page(
    tableid_t table_id,
    pagenum_t pagenum,
    page_t * dest,
    bool pin =true
)
```

Read an on-disk page into the in-memory page structure(dest) 

**Parameters**: 

  * **table_id** table id obtained with <code><a href="/Modules/group__BufferManager#function-buffered-open-table-file">buffered&#95;open&#95;table&#95;file()</a></code>. 
  * **pagenum** page index. 
  * **dest** the pointer of the page data. 
  * **pin** <code>true</code> if this buffer will be writed after. 


### function buffered_write_page

```
void buffered_write_page(
    tableid_t table_id,
    pagenum_t pagenum,
    const page_t * src
)
```

Write an in-memory page(src) to the on-disk page. 

**Parameters**: 

  * **table_id** table id obtained with <code><a href="/Modules/group__BufferManager#function-buffered-open-table-file">buffered&#95;open&#95;table&#95;file()</a></code>. 
  * **pagenum** page index. 
  * **src** the pointer of the page data. 


### function shutdown_buffer

```
int shutdown_buffer()
```

Shutdown buffer manager. 

**Return**: <code>0</code> if success, non-zero value otherwise. 


## Attributes Documentation

### variable buffer_slot

```
BufferBlock * buffer_slot = nullptr;
```

buffer slot. 

### variable buffer_head_idx

```
int buffer_head_idx = -1;
```

head index of Recently-Used list. 

### variable buffer_tail_idx

```
int buffer_tail_idx = -1;
```

tail index of Recently-Used list. 

### variable buffer_size

```
int buffer_size = 0;
```

size of buffer block. 

### variable buffer_index

```
std::unordered_map< PageLocation, int > buffer_index;
```





-------------------------------

Updated on 2021-10-25 at 17:04:35 +0900