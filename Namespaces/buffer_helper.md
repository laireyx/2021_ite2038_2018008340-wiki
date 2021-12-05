

# buffer_helper

**Module:** **[DiskSpaceManager](/Modules/DiskSpaceManager)** **/** **[BufferManager](/Modules/BufferManager)**

BufferManager helper.  [More...](#detailed-description)

## Functions

|                | Name           |
| -------------- | -------------- |
| <a href="/Classes/BufferBlock">BufferBlock</a> * | **[load_buffer](/Namespaces/buffer_helper#function-load_buffer)**(tableid_t table_id, pagenum_t pagenum, <a href="/Modules/DiskSpaceManager#typedef-page-t">page_t</a> * page, trxid_t trx_id =0, bool pin =true)<br>Load a page into buffer.  |
| bool | **[apply_buffer](/Namespaces/buffer_helper#function-apply_buffer)**(tableid_t table_id, pagenum_t pagenum, const <a href="/Modules/DiskSpaceManager#typedef-page-t">page_t</a> * page)<br>Apply a page into buffer.  |
| void | **[release_buffer](/Namespaces/buffer_helper#function-release_buffer)**(tableid_t table_id, pagenum_t pagenum)<br>Release a buffer page.  |
| bool | **[is_full](/Namespaces/buffer_helper#function-is_full)**()<br>Check if there buffer slot is full.  |
| int | **[evict](/Namespaces/buffer_helper#function-evict)**()<br>Evict a buffer with the lowest priority.  |
| void | **[move_to_head](/Namespaces/buffer_helper#function-move_to_head)**(int buffer_idx)<br>Move a buffer to head of Recently-Used list.  |

## Detailed Description

BufferManager helper. 

This namespace includes some helper functions which are used by buffermanager API. Those functions are not a part of buffermanager API, but frequently used part of it so I wrapped these functions. 


## Functions Documentation

### function load_buffer

```cpp
BufferBlock * load_buffer(
    tableid_t table_id,
    pagenum_t pagenum,
    page_t * page,
    trxid_t trx_id =0,
    bool pin =true
)
```

Load a page into buffer. 

**Parameters**: 

  * **table_id** table id. 
  * **pagenum** page number. 
  * **page** page. 
  * **trx_id** transaction id. 
  * **pin** pin. 


**Return**: loaded buffer. 

Return a buffer block if exists. If not, automatically evict a buffer with the lowest priority and load a buffer in that position. If there are no room for more buffer, fallback direct I/O method will be used.


### function apply_buffer

```cpp
bool apply_buffer(
    tableid_t table_id,
    pagenum_t pagenum,
    const page_t * page
)
```

Apply a page into buffer. 

**Parameters**: 

  * **table_id** table id. 
  * **pagenum** page number. 
  * **page** page content. 


**Return**: <code>true</code> if buffer write success, <code>false</code> if fallback method is used. 

Apply page content into buffer block if exists. If not, return <code>false</code> to notify fallback direct I/O method should be used.


### function release_buffer

```cpp
void release_buffer(
    tableid_t table_id,
    pagenum_t pagenum
)
```

Release a buffer page. 

**Parameters**: 

  * **page_location** page location. 
  * **page** page content. 


Remove the pin from the buffer.


### function is_full

```cpp
bool is_full()
```

Check if there buffer slot is full. 

**Return**: true if there are no room 

### function evict

```cpp
int evict()
```

Evict a buffer with the lowest priority. 

**Return**: evicted buffer slot index if success, <code>&lt;0</code> otherwise. 

<code><a href="/Namespaces/buffer_helper#function-load-buffer">load&#95;buffer()</a></code> will determine using of fallback method with return value of <code><a href="/Namespaces/buffer_helper#function-evict">evict()</a></code>.


### function move_to_head

```cpp
void move_to_head(
    int buffer_idx
)
```

Move a buffer to head of Recently-Used list. 

**Parameters**: 

  * **buffer_idx** index of buffer which will be detached. 






-------------------------------

Updated on 2021-12-05 at 18:36:40 +0900