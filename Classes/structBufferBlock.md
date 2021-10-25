---
title: BufferBlock

---

# BufferBlock

**Module:** **[BufferManager](/Modules/group__BufferManager)**






`#include <buffer.h>`

## Public Attributes

|                | Name           |
| -------------- | -------------- |
| <a href="/Modules/group__DiskSpaceManager#typedef-fullpage-t">fullpage_t</a> | **[page](/Classes/structBufferBlock#variable-page)** <br>buffered page.  |
| <a href="/Modules/group__BufferManager#typedef-pagelocation">PageLocation</a> | **[page_location](/Classes/structBufferBlock#variable-page-location)** <br>page location.  |
| bool | **[is_dirty](/Classes/structBufferBlock#variable-is-dirty)** <br><code>true</code> if this buffer has been modified, <code>false</code> otherwise.  |
| int | **[is_pinned](/Classes/structBufferBlock#variable-is-pinned)** <br><code>true</code> if this buffer is currently using, <code>false</code> otherwise.  |
| int | **[prev_block_idx](/Classes/structBufferBlock#variable-prev-block-idx)** <br>revious buffer block index of Recently-Used linked list.  |
| int | **[next_block_idx](/Classes/structBufferBlock#variable-next-block-idx)** <br>next buffer block index of Recently-Used linked list.  |

## Public Attributes Documentation

### variable page

```cpp
fullpage_t page;
```

buffered page. 

### variable page_location

```cpp
PageLocation page_location;
```

page location. 

### variable is_dirty

```cpp
bool is_dirty;
```

<code>true</code> if this buffer has been modified, <code>false</code> otherwise. 

### variable is_pinned

```cpp
int is_pinned;
```

<code>true</code> if this buffer is currently using, <code>false</code> otherwise. 

### variable prev_block_idx

```cpp
int prev_block_idx;
```

revious buffer block index of Recently-Used linked list. 

<code>-1</code> if this buffer is the first item. 


### variable next_block_idx

```cpp
int next_block_idx;
```

next buffer block index of Recently-Used linked list. 

<code>-1</code> if this buffer is the last item. 


-------------------------------

Updated on 2021-10-25 at 16:53:02 +0900