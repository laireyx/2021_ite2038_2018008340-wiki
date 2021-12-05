

# db/src/buffer.cc



## Namespaces

| Name           |
| -------------- |
| **[buffer_helper](/Namespaces/buffer_helper)** <br>BufferManager helper.  |

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

## Attributes

|                | Name           |
| -------------- | -------------- |
| <a href="/Classes/BufferBlock">BufferBlock</a> * | **[buffer_slot](/Modules/BufferManager#variable-buffer_slot)** <br>buffer slot.  |
| int | **[buffer_head_idx](/Modules/BufferManager#variable-buffer_head_idx)** <br>head index of Recently-Used list.  |
| int | **[buffer_tail_idx](/Modules/BufferManager#variable-buffer_tail_idx)** <br>tail index of Recently-Used list.  |
| int | **[buffer_size](/Modules/BufferManager#variable-buffer_size)** <br>size of buffer block.  |
| std::unordered_map< PageLocation, int > | **[buffer_index](/Modules/BufferManager#variable-buffer_index)**  |
| pthread_mutex_t * | **[buffer_manager_mutex](/Modules/BufferManager#variable-buffer_manager_mutex)**  |


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


## Attributes Documentation

### variable buffer_slot

```cpp
BufferBlock * buffer_slot = nullptr;
```

buffer slot. 

### variable buffer_head_idx

```cpp
int buffer_head_idx = -1;
```

head index of Recently-Used list. 

### variable buffer_tail_idx

```cpp
int buffer_tail_idx = -1;
```

tail index of Recently-Used list. 

### variable buffer_size

```cpp
int buffer_size = 0;
```

size of buffer block. 

### variable buffer_index

```cpp
std::unordered_map< PageLocation, int > buffer_index;
```


### variable buffer_manager_mutex

```cpp
pthread_mutex_t * buffer_manager_mutex;
```



## Source code

```cpp

#include <buffer.h>
#include <errors.h>
#include <file.h>
#include <pthread.h>

#include <cstring>
#include <iostream>
#include <new>
#include <unordered_map>
#include <utility>

BufferBlock* buffer_slot = nullptr;
int buffer_head_idx = -1;
int buffer_tail_idx = -1;
int buffer_size = 0;

std::unordered_map<PageLocation, int> buffer_index;

pthread_mutex_t* buffer_manager_mutex;

namespace buffer_helper {
BufferBlock* load_buffer(tableid_t table_id, pagenum_t pagenum, page_t* page,
                         trxid_t trx_id, bool pin) {
    pthread_mutex_lock(buffer_manager_mutex);
    auto page_location = std::make_pair(table_id, pagenum);
    const auto& existing_buffer = buffer_index.find(page_location);
    if (existing_buffer != buffer_index.end()) {
        int buffer_page_idx = existing_buffer->second;
        BufferBlock* buffer_page = buffer_slot + buffer_page_idx;

        if (pin && buffer_page->pin_count > 0) {
            buffer_page->pin_count++;
            if(buffer_page->pin_owner == trx_id) {
                std::cout << "wow" << std::endl;
            }
            pthread_cond_wait(&buffer_page->latch, buffer_manager_mutex);
            //pthread_mutex_lock(&buffer_page->mutex);
            buffer_page->pin_owner = trx_id;
        }

        move_to_head(buffer_page_idx);

        if (page != nullptr) {
            memcpy(page, &(buffer_page->page), PAGE_SIZE);
        }

        pthread_mutex_unlock(buffer_manager_mutex);
        return buffer_slot + buffer_page_idx;
    }

    int evicted_idx = evict();
    if (evicted_idx >= 0) {
        // add
        BufferBlock* buffer_page = buffer_slot + evicted_idx;

        if (pin) {
            // buffer_page->someone_waiting++;
            // pthread_mutex_unlock(buffer_manager_mutex);
            // pthread_mutex_lock(&buffer_page->mutex);
            // pthread_mutex_lock(buffer_manager_mutex);
            // buffer_page->someone_waiting--;
            buffer_page->pin_count++;
            buffer_page->pin_owner = trx_id;
        }

        move_to_head(evicted_idx);

        buffer_index[page_location] = evicted_idx;

        buffer_page->is_dirty = false;

        buffer_page->page_location = page_location;

        file_read_page(table_id, pagenum, &(buffer_page->page));
        if (page != nullptr) {
            memcpy(page, &(buffer_page->page), PAGE_SIZE);
        }

        pthread_mutex_unlock(buffer_manager_mutex);
        return buffer_page;
    }

    // direct I/O fallback
    if (page != nullptr) {
        file_read_page(table_id, pagenum, page);
    }
    pthread_mutex_unlock(buffer_manager_mutex);
    return nullptr;
}

bool apply_buffer(tableid_t table_id, pagenum_t pagenum, const page_t* page) {
    pthread_mutex_lock(buffer_manager_mutex);
    auto page_location = std::make_pair(table_id, pagenum);
    const auto& existing_buffer = buffer_index.find(page_location);
    if (existing_buffer != buffer_index.end()) {
        int buffer_page_idx = existing_buffer->second;
        BufferBlock* buffer_page = buffer_slot + buffer_page_idx;

        move_to_head(buffer_page_idx);

        memcpy(&(buffer_page->page), page, PAGE_SIZE);
        buffer_page->is_dirty = true;
        buffer_page->pin_count--;
        buffer_page->pin_owner = -1;

        pthread_cond_signal(&buffer_page->latch);
        //pthread_mutex_unlock(&buffer_page->mutex);
        pthread_mutex_unlock(buffer_manager_mutex);
        return true;
    }

    // direct I/O fallback
    file_write_page(table_id, pagenum, page);
    pthread_mutex_unlock(buffer_manager_mutex);
    return false;
}

void release_buffer(tableid_t table_id, pagenum_t pagenum) {
    pthread_mutex_lock(buffer_manager_mutex);
    auto page_location = std::make_pair(table_id, pagenum);
    const auto& existing_buffer = buffer_index.find(page_location);
    if (existing_buffer != buffer_index.end()) {
        int buffer_page_idx = existing_buffer->second;
        BufferBlock* buffer_page = buffer_slot + buffer_page_idx;

        buffer_page->pin_count--;
        buffer_page->pin_owner = -1;
        pthread_cond_signal(&buffer_page->latch);
        //pthread_mutex_unlock(&buffer_page->mutex);
    }
    pthread_mutex_unlock(buffer_manager_mutex);
}

int evict() {
    int evicted_idx = buffer_tail_idx;
    BufferBlock* buffer_evict = buffer_slot + evicted_idx;

    while (evicted_idx != -1 && buffer_evict->pin_count > 0) {
        evicted_idx = buffer_evict->prev_block_idx;
        buffer_evict = buffer_slot + evicted_idx;
    }

    // All the buffers are using
    if (evicted_idx == -1) {
        return -1;
    }

    buffer_index.erase(buffer_evict->page_location);

    // Flush to file if dirty
    if (buffer_evict->is_dirty) {
        tableid_t table_id;
        pagenum_t page_num;
        std::tie(table_id, page_num) = buffer_evict->page_location;
        file_write_page(table_id, page_num, &buffer_evict->page);
    }
    return evicted_idx;
}

void move_to_head(int buffer_idx) {
    if(buffer_idx == buffer_head_idx) return;

    BufferBlock* buffer_page = buffer_slot + buffer_idx;
    BufferBlock* buffer_head = buffer_slot + buffer_head_idx;

    if (buffer_page->prev_block_idx != -1) {
        BufferBlock* buffer_prev = buffer_slot + (buffer_page->prev_block_idx);
        buffer_prev->next_block_idx = buffer_page->next_block_idx;
    }

    if (buffer_page->next_block_idx != -1) {
        BufferBlock* buffer_next = buffer_slot + (buffer_page->next_block_idx);
        buffer_next->prev_block_idx = buffer_page->prev_block_idx;
    }

    if(buffer_idx == buffer_tail_idx) {
        buffer_tail_idx = buffer_page->prev_block_idx;
    }

    buffer_page->prev_block_idx = -1;
    buffer_page->next_block_idx = buffer_head_idx;
    buffer_head->prev_block_idx = buffer_idx;

    buffer_head_idx = buffer_idx;
}
}  // namespace buffer_helper

int init_buffer(int _buffer_size) {
    try {
        buffer_manager_mutex = new pthread_mutex_t;
        if (pthread_mutex_init(buffer_manager_mutex, nullptr)) {
            return -1;
        }

        if (buffer_slot != nullptr) {
            if (_buffer_size == buffer_size) {
                return 0;
            } else {
                return -1;
            }
        }
        buffer_size = _buffer_size;
        buffer_slot = new BufferBlock[buffer_size];
        for (int i = 0; i < buffer_size; i++) {
            buffer_slot[i].page_location =
                std::make_pair(MAX_TABLE_INSTANCE + 1, 0);

            pthread_cond_init(&buffer_slot[i].latch, nullptr);
            buffer_slot[i].is_dirty = false;
            buffer_slot[i].pin_count = 0;
            buffer_slot[i].pin_owner = -1;

            if (i < buffer_size - 1) buffer_slot[i].next_block_idx = i + 1;
            if (i > 0) buffer_slot[i].prev_block_idx = i - 1;
        }

        buffer_slot[0].prev_block_idx = -1;
        buffer_slot[buffer_size - 1].next_block_idx = -1;

        buffer_head_idx = 0;
        buffer_tail_idx = buffer_size - 1;

        return 0;
    } catch (const std::bad_alloc& err) {
        return -1;
    }
}

tableid_t buffered_open_table_file(const char* path) {
    return file_open_table_file(path);
}

pagenum_t buffered_alloc_page(tableid_t table_id, trxid_t trx_id) {
    file_helper::extend_capacity(table_id);

    headerpage_t header_page;

    buffer_helper::load_buffer(table_id, 0, &header_page, trx_id);

    // Pop the first page from free page stack.
    pagenum_t free_page_idx = header_page.free_page_idx;

    freepage_t allocated_page;
    buffer_helper::load_buffer(table_id, free_page_idx, &allocated_page,
                               trx_id);

    // Move the first free page index to the next page.
    header_page.free_page_idx = allocated_page.next_free_idx;

    buffer_helper::apply_buffer(table_id, 0, &header_page);
    buffer_helper::apply_buffer(table_id, free_page_idx, &allocated_page);

    return free_page_idx;
}

void buffered_free_page(tableid_t table_id, pagenum_t pagenum, trxid_t trx_id) {
    headerpage_t header_page;
    buffer_helper::load_buffer(table_id, 0, &header_page, trx_id);

    // Current first free page index
    pagenum_t old_free_page_idx = header_page.free_page_idx;
    // Newly freed page
    freepage_t new_free_page;
    buffer_helper::load_buffer(table_id, pagenum, &new_free_page, trx_id);

    // Next free page index of newly freed page is current first free page
    // index. Its just pushing the pagenum into free page stack.
    new_free_page.next_free_idx = old_free_page_idx;
    buffer_helper::apply_buffer(table_id, pagenum, &new_free_page);

    // Set the first free page to freed page number.
    header_page.free_page_idx = pagenum;
    buffer_helper::apply_buffer(table_id, 0, &header_page);
}

void buffered_read_page(tableid_t table_id, pagenum_t pagenum, page_t* dest,
                        trxid_t trx_id, bool pin) {
    buffer_helper::load_buffer(table_id, pagenum, dest, trx_id, pin);
}

void buffered_write_page(tableid_t table_id, pagenum_t pagenum,
                         const page_t* src) {
    buffer_helper::apply_buffer(table_id, pagenum, src);
}

void buffered_release_page(tableid_t table_id, pagenum_t pagenum) {
    buffer_helper::release_buffer(table_id, pagenum);
}

int shutdown_buffer() {
    if (buffer_slot != nullptr) {
        for (int i = 0; i < buffer_size; i++) {
            tableid_t table_id;
            pagenum_t page_num;

            BufferBlock* buffer = buffer_slot + i;
            std::tie(table_id, page_num) = buffer->page_location;

            if (buffer->is_dirty) {
                file_write_page(table_id, page_num, &buffer->page);
            }
            pthread_cond_destroy(&buffer_slot[i].latch);
        }
        delete[] buffer_slot;
        buffer_slot = nullptr;
        buffer_size = 0;
        buffer_index.clear();
    }
    pthread_mutex_destroy(buffer_manager_mutex);
    delete buffer_manager_mutex;

    file_close_table_files();
    return 0;
}
```


-------------------------------

Updated on 2021-12-05 at 18:53:29 +0900
