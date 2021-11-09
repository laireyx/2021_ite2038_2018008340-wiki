

# BufferBlock

**Module:** **[BufferManager](/Modules/BufferManager)**






`#include <buffer.h>`

## Public Attributes

|                | Name           |
| -------------- | -------------- |
| <a href="/Modules/DiskSpaceManager#typedef-fullpage-t">fullpage_t</a> | **[page](/Classes/BufferBlock#variable-page)** <br>buffered page.  |
| <a href="/Modules/BufferManager#typedef-pagelocation">PageLocation</a> | **[page_location](/Classes/BufferBlock#variable-page_location)** <br>page location.  |
| bool | **[is_dirty](/Classes/BufferBlock#variable-is_dirty)** <br><code>true</code> if this buffer has been modified, <code>false</code> otherwise.  |
| int | **[is_pinned](/Classes/BufferBlock#variable-is_pinned)** <br><code>true</code> if this buffer is currently using, <code>false</code> otherwise.  |
| int | **[prev_block_idx](/Classes/BufferBlock#variable-prev_block_idx)** <br>revious buffer block index of Recently-Used linked list.  |
| int | **[next_block_idx](/Classes/BufferBlock#variable-next_block_idx)** <br>next buffer block index of Recently-Used linked list.  |

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

Updated on 2021-11-09 at 23:03:19 +0900