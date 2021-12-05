

# BufferBlock

**Module:** **[DiskSpaceManager](/Modules/DiskSpaceManager)** **/** **[BufferManager](/Modules/BufferManager)**






`#include <buffer.h>`

## Public Attributes

|                | Name           |
| -------------- | -------------- |
| <a href="/Modules/DiskSpaceManager#typedef-fullpage-t">fullpage_t</a> | **[page](/Classes/BufferBlock#variable-page)** <br>buffered page.  |
| PageLocation | **[page_location](/Classes/BufferBlock#variable-page_location)** <br>page location.  |
| pthread_cond_t | **[latch](/Classes/BufferBlock#variable-latch)** <br>page latch.  |
| trxid_t | **[pin_owner](/Classes/BufferBlock#variable-pin_owner)**  |
| int | **[pin_count](/Classes/BufferBlock#variable-pin_count)** <br>how many threads are using(or waiting) this buffer block.  |
| bool | **[is_dirty](/Classes/BufferBlock#variable-is_dirty)** <br><code>true</code> if this buffer has been modified, <code>false</code> otherwise.  |
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

### variable latch

```cpp
pthread_cond_t latch;
```

page latch. 

### variable pin_owner

```cpp
trxid_t pin_owner;
```


### variable pin_count

```cpp
int pin_count;
```

how many threads are using(or waiting) this buffer block. 

### variable is_dirty

```cpp
bool is_dirty;
```

<code>true</code> if this buffer has been modified, <code>false</code> otherwise. 

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

Updated on 2021-12-05 at 18:37:58 +0900