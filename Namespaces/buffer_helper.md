

# buffer_helper

**Module:** **[BufferManager](/Modules/BufferManager)**

BufferManager helper.  [More...](#detailed-description)

## Functions

|                | Name           |
| -------------- | -------------- |
| <a href="/Classes/BufferBlock">BufferBlock</a> * | **[load_buffer](/Namespaces/buffer_helper#function-load_buffer)**(const <a href="/Modules/BufferManager#typedef-pagelocation">PageLocation</a> & page_location, <a href="/Modules/DiskSpaceManager#typedef-page-t">page_t</a> * page, bool pin =true)<br>Load a page into buffer.  |
| bool | **[apply_buffer](/Namespaces/buffer_helper#function-apply_buffer)**(const <a href="/Modules/BufferManager#typedef-pagelocation">PageLocation</a> & page_location, const <a href="/Modules/DiskSpaceManager#typedef-page-t">page_t</a> * page)<br>Apply a page into buffer.  |
| bool | **[is_full](/Namespaces/buffer_helper#function-is_full)**()<br>Check if there buffer slot is full.  |
| int | **[evict](/Namespaces/buffer_helper#function-evict)**()<br>Evict a buffer with the lowest priority.  |
| void | **[detach_from_tree](/Namespaces/buffer_helper#function-detach_from_tree)**(int buffer_idx)<br>detach a buffer from Recently-Used list.  |
| void | **[prepend_to_head](/Namespaces/buffer_helper#function-prepend_to_head)**(int buffer_idx)<br>prepend a buffer to the head of Recently-Used list.  |

## Detailed Description

BufferManager helper. 

This namespace includes some helper functions which are used by buffermanager API. Those functions are not a part of buffermanager API, but frequently used part of it so I wrapped these functions. 


## Functions Documentation

### function load_buffer

```cpp
BufferBlock * load_buffer(
    const PageLocation & page_location,
    page_t * page,
    bool pin =true
)
```

Load a page into buffer. 

**Parameters**: 

  * **page_location** page location. 
  * **page** page. 
  * **pin** pin. 


**Return**: loaded buffer. 

Return a buffer block if exists. If not, automatically evict a buffer with the lowest priority and load a buffer in that position. If there are no room for more buffer, fallback direct I/O method will be used.


### function apply_buffer

```cpp
bool apply_buffer(
    const PageLocation & page_location,
    const page_t * page
)
```

Apply a page into buffer. 

**Parameters**: 

  * **page_location** page location. 
  * **page** page content. 


**Return**: <code>true</code> if buffer write success, <code>false</code> if fallback method is used. 

Apply page content into buffer block if exists. If not, return <code>false</code> to notify fallback direct I/O method should be used.


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


### function detach_from_tree

```cpp
void detach_from_tree(
    int buffer_idx
)
```

detach a buffer from Recently-Used list. 

**Parameters**: 

  * **buffer_idx** index of buffer which will be detached. 


It links previous and next index of given buffer.


### function prepend_to_head

```cpp
void prepend_to_head(
    int buffer_idx
)
```

prepend a buffer to the head of Recently-Used list. 

**Parameters**: 

  * **buffer_idx** index of buffer which will be prepended. 






-------------------------------

Updated on 2021-10-25 at 17:08:33 +0900