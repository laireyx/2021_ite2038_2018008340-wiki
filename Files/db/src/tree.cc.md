

# db/src/tree.cc



## Functions

|                | Name           |
| -------------- | -------------- |
| pagenum_t | **[make_node](/Modules/IndexManager#function-make_node)**(tableid_t table_id, pagenum_t parent_page_idx =0)<br>Allocate and make an internal page.  |
| pagenum_t | **[make_leaf](/Modules/IndexManager#function-make_leaf)**(tableid_t table_id, pagenum_t parent_page_idx =0)<br>Allocate and make a leaf page.  |
| pagenum_t | **[create_tree](/Modules/IndexManager#function-create_tree)**(tableid_t table_id, recordkey_t key, const char * value, valsize_t value_size)<br>Create a new tree.  |
| pagenum_t | **[find_leaf](/Modules/IndexManager#function-find_leaf)**(tableid_t table_id, recordkey_t key, trxid_t trx_id =0)<br>Find a leaf node which contains given key.  |
| bool | **[find_by_key](/Modules/IndexManager#function-find_by_key)**(tableid_t table_id, recordkey_t key, char * value =nullptr, valsize_t * value_size =nullptr, trxid_t trx_id =0)<br>Find a record with key.  |
| pagenum_t | **[insert_into_new_root](/Modules/IndexManager#function-insert_into_new_root)**(tableid_t table_id, pagenum_t left_page_idx, recordkey_t key, pagenum_t right_page_idx) |
| pagenum_t | **[insert_into_node](/Modules/IndexManager#function-insert_into_node)**(tableid_t table_id, pagenum_t parent_page_idx, pagenum_t left_page_idx, recordkey_t key, pagenum_t right_page_idx)<br>Insert a <code>(key, right&#95;page&#95;idx)</code> tuple in parent page.  |
| pagenum_t | **[insert_into_node_after_splitting](/Modules/IndexManager#function-insert_into_node_after_splitting)**(tableid_t table_id, pagenum_t parent_page_idx, recordkey_t key, pagenum_t right_page_idx)<br>Insert a <code>(key, right&#95;page&#95;idx)</code> tuple in parent page, and split it into two pages.  |
| pagenum_t | **[insert_into_parent](/Modules/IndexManager#function-insert_into_parent)**(tableid_t table_id, pagenum_t left_page_idx, recordkey_t key, pagenum_t right_page_idx)<br>Choose right method between just inserting and <code>insert&#95;into&#95;node&#95;after&#95;splitting</code> and call it.  |
| pagenum_t | **[insert_into_leaf_after_splitting](/Modules/IndexManager#function-insert_into_leaf_after_splitting)**(tableid_t table_id, pagenum_t leaf_page_idx, recordkey_t key, const char * value, valsize_t value_size)<br>Insert <code>(key, value)</code>into leaf node and split it into two pages.  |
| pagenum_t | **[insert_node](/Modules/IndexManager#function-insert_node)**(tableid_t table_id, recordkey_t key, const char * value, valsize_t value_size)<br>Find appropriate leaf page and insert a record into it.  |
| pagenum_t | **[adjust_root](/Modules/IndexManager#function-adjust_root)**(tableid_t table_id)<br>Adjust root page.  |
| pagenum_t | **[coalesce_internal_nodes](/Modules/IndexManager#function-coalesce_internal_nodes)**(tableid_t table_id, pagenum_t left_page_idx, recordkey_t seperate_key, int seperate_key_idx, pagenum_t right_page_idx)<br>Coalesces two internal pages.  |
| pagenum_t | **[coalesce_leaf_nodes](/Modules/IndexManager#function-coalesce_leaf_nodes)**(tableid_t table_id, pagenum_t left_page_idx, pagenum_t right_page_idx)<br>Coalesces two leaf pages.  |
| pagenum_t | **[delete_internal_key](/Modules/IndexManager#function-delete_internal_key)**(tableid_t table_id, pagenum_t internal_page_idx, recordkey_t key)<br>Delete a page branch from internal page.  |
| pagenum_t | **[delete_leaf_key](/Modules/IndexManager#function-delete_leaf_key)**(tableid_t table_id, pagenum_t leaf_page_idx, recordkey_t key)<br>Delete a record from leaf page.  |
| pagenum_t | **[delete_node](/Modules/IndexManager#function-delete_node)**(tableid_t table_id, recordkey_t key)<br>Entrance for remove a record from table.  |
| pagenum_t | **[update_node](/Modules/IndexManager#function-update_node)**(tableid_t table_id, recordkey_t key, const char * value, valsize_t new_val_size, valsize_t * old_val_size, trxid_t trx_id)<br>Update a record value.  |


## Functions Documentation

### function make_node

```cpp
pagenum_t make_node(
    tableid_t table_id,
    pagenum_t parent_page_idx =0
)
```

Allocate and make an internal page. 

**Parameters**: 

  * **table_id** table id. 
  * **parent_page_idx** parent page index. 


**Return**: created page index. 

### function make_leaf

```cpp
pagenum_t make_leaf(
    tableid_t table_id,
    pagenum_t parent_page_idx =0
)
```

Allocate and make a leaf page. 

**Parameters**: 

  * **table_id** table id. 
  * **parent_page_idx** parent page index. 


**Return**: created page index. 

### function create_tree

```cpp
pagenum_t create_tree(
    tableid_t table_id,
    recordkey_t key,
    const char * value,
    valsize_t value_size
)
```

Create a new tree. 

**Parameters**: 

  * **table_id** table id. 
  * **key** initial record key. 
  * **value** initial record value. 
  * **value_size** initial record value size. 


**Return**: root page index. 

Create a new leaf page for root page and set initial record.


### function find_leaf

```cpp
pagenum_t find_leaf(
    tableid_t table_id,
    recordkey_t key,
    trxid_t trx_id =0
)
```

Find a leaf node which contains given key. 

**Parameters**: 

  * **table_id** table id. 
  * **key** key to query with. 
  * **trx_id** transaction id. 


**Return**: page index if found. <code>0</code> if the key does not exist. 

### function find_by_key

```cpp
bool find_by_key(
    tableid_t table_id,
    recordkey_t key,
    char * value =nullptr,
    valsize_t * value_size =nullptr,
    trxid_t trx_id =0
)
```

Find a record with key. 

**Parameters**: 

  * **table_id** table id. 
  * **key** key to query with. 
  * **value** If the record is found successful, then this value is set to record value. Caller should allocate enough(MAX_VALUE_SIZE) memory for it. 
  * **value_size** If the record is found successful, then this value is set to record size. 
  * **trx_id** transaction id. 


**Return**: <code>true</code> if found successful. <code>false</code> otherwise. 

### function insert_into_new_root

```cpp
pagenum_t insert_into_new_root(
    tableid_t table_id,
    pagenum_t left_page_idx,
    recordkey_t key,
    pagenum_t right_page_idx
)
```


### function insert_into_node

```cpp
pagenum_t insert_into_node(
    tableid_t table_id,
    pagenum_t parent_page_idx,
    pagenum_t left_page_idx,
    recordkey_t key,
    pagenum_t right_page_idx
)
```

Insert a <code>(key, right&#95;page&#95;idx)</code> tuple in parent page. 

**Parameters**: 

  * **table_id** table id. 
  * **parent_page_idx** parent page index. 
  * **left_page_idx** left page index. 
  * **key** key which means right page. 
  * **right_page_idx** right page index. 


**Return**: root page number. 

### function insert_into_node_after_splitting

```cpp
pagenum_t insert_into_node_after_splitting(
    tableid_t table_id,
    pagenum_t parent_page_idx,
    recordkey_t key,
    pagenum_t right_page_idx
)
```

Insert a <code>(key, right&#95;page&#95;idx)</code> tuple in parent page, and split it into two pages. 

**Parameters**: 

  * **table_id** table id. 
  * **parent_page_idx** parent page index. 
  * **key** key which means right page. 
  * **right_page_idx** right page index. 


**Return**: root page number. 

### function insert_into_parent

```cpp
pagenum_t insert_into_parent(
    tableid_t table_id,
    pagenum_t left_page_idx,
    recordkey_t key,
    pagenum_t right_page_idx
)
```

Choose right method between just inserting and <code>insert&#95;into&#95;node&#95;after&#95;splitting</code> and call it. 

**Parameters**: 

  * **table_id** table id. 
  * **left_page_idx** left page index. 
  * **key** key which means right page. 
  * **right_page_idx** right page index. 


**Return**: root page number. 

### function insert_into_leaf_after_splitting

```cpp
pagenum_t insert_into_leaf_after_splitting(
    tableid_t table_id,
    pagenum_t leaf_page_idx,
    recordkey_t key,
    const char * value,
    valsize_t value_size
)
```

Insert <code>(key, value)</code>into leaf node and split it into two pages. 

**Parameters**: 

  * **table_id** table id. 
  * **leaf_page_idx** leaf page index. 
  * **key** record key. 
  * **value** record value. 
  * **value_size** record value size. 


**Return**: root page number. 

### function insert_node

```cpp
pagenum_t insert_node(
    tableid_t table_id,
    recordkey_t key,
    const char * value,
    valsize_t value_size
)
```

Find appropriate leaf page and insert a record into it. 

**Parameters**: 

  * **table_id** table id. 
  * **key** record key. 
  * **value** record value. 
  * **value_size** record value size. 


**Return**: root page number. 

### function adjust_root

```cpp
pagenum_t adjust_root(
    tableid_t table_id
)
```

Adjust root page. 

**Parameters**: 

  * **table_id** table id. 


**Return**: <code>&gt;0</code> if succesful. <code>0</code> otherwise. 

Pull up the child page if the root page is empty internal page. Or free the root page if it is empty leaf page.


### function coalesce_internal_nodes

```cpp
pagenum_t coalesce_internal_nodes(
    tableid_t table_id,
    pagenum_t left_page_idx,
    recordkey_t seperate_key,
    int seperate_key_idx,
    pagenum_t right_page_idx
)
```

Coalesces two internal pages. 

**Parameters**: 

  * **table_id** table id. 
  * **left_page_idx** left page index. 
  * **seperate_key** key which can seperate between left and right page. 
  * **seperate_key_idx** parent branch index of <code>seperate&#95;key</code>. 
  * **right_page_idx** right page index. 


**Return**: root page number. 

Moves all right page branch into the left page.


### function coalesce_leaf_nodes

```cpp
pagenum_t coalesce_leaf_nodes(
    tableid_t table_id,
    pagenum_t left_page_idx,
    pagenum_t right_page_idx
)
```

Coalesces two leaf pages. 

**Parameters**: 

  * **table_id** table id. 
  * **left_page_idx** left page index. 
  * **right_page_idx** right page index. 


**Return**: root page number. 

Moves all right page record into the left page.


### function delete_internal_key

```cpp
pagenum_t delete_internal_key(
    tableid_t table_id,
    pagenum_t internal_page_idx,
    recordkey_t key
)
```

Delete a page branch from internal page. 

**Parameters**: 

  * **table_id** table id. 
  * **internal_page_idx** internal page index. 
  * **key** branch key. 


**Return**: root page number. 

### function delete_leaf_key

```cpp
pagenum_t delete_leaf_key(
    tableid_t table_id,
    pagenum_t leaf_page_idx,
    recordkey_t key
)
```

Delete a record from leaf page. 

**Parameters**: 

  * **table_id** table id. 
  * **leaf_page_idx** leaf page index. 
  * **key** record key. 


**Return**: root page number. 

### function delete_node

```cpp
pagenum_t delete_node(
    tableid_t table_id,
    recordkey_t key
)
```

Entrance for remove a record from table. 

**Parameters**: 

  * **table_id** table id. 
  * **key** record key. 


**Return**: root page number. 

### function update_node

```cpp
pagenum_t update_node(
    tableid_t table_id,
    recordkey_t key,
    const char * value,
    valsize_t new_val_size,
    valsize_t * old_val_size,
    trxid_t trx_id
)
```

Update a record value. 

**Parameters**: 

  * **table_id** table id. 
  * **key** record key. 
  * **value** new record value. 
  * **new_val_size** new record value size. 
  * **old_val_size** old record value size. 
  * **trx_id** transaction id. 


**Return**: updated record page number. 



## Source code

```cpp

#include <buffer.h>
#include <errors.h>
#include <page.h>
#include <transaction.h>
#include <tree.h>

#include <algorithm>
#include <utility>
#include <vector>

pagenum_t make_node(tableid_t table_id, pagenum_t parent_page_idx) {
    allocatedpage_t page = {};
    pagenum_t page_idx = buffered_alloc_page(table_id);

    buffered_read_page(table_id, page_idx, &page);
    page.page_header.is_leaf_page = 0;
    page.page_header.parent_page_idx = parent_page_idx;
    page.page_header.key_num = 0;

    buffered_write_page(table_id, page_idx, &page);

    return page_idx;
}

pagenum_t make_leaf(tableid_t table_id, pagenum_t parent_page_idx) {
    leafpage_t leaf_page;
    pagenum_t leaf_page_idx = buffered_alloc_page(table_id);

    buffered_read_page(table_id, leaf_page_idx, &leaf_page);

    leaf_page.page_header.is_leaf_page = 1;
    leaf_page.page_header.parent_page_idx = parent_page_idx;
    leaf_page.page_header.key_num = 0;

    leaf_page.page_header.reserved_footer.footer_1 = 3968;
    leaf_page.page_header.reserved_footer.footer_2 = 0;

    buffered_write_page(table_id, leaf_page_idx, &leaf_page);

    return leaf_page_idx;
}

pagenum_t create_tree(tableid_t table_id, recordkey_t key, const char* value,
                      valsize_t value_size) {
    headerpage_t header_page;

    leafpage_t leaf_page;
    pagenum_t leaf_page_idx = make_leaf(table_id);

    buffered_read_page(table_id, 0, &header_page);
    buffered_read_page(table_id, leaf_page_idx, &leaf_page);

    page_helper::add_leaf_value(&leaf_page, key, value, value_size);

    header_page.root_page_idx = leaf_page_idx;

    buffered_write_page(table_id, leaf_page_idx, &leaf_page);
    buffered_write_page(table_id, 0, &header_page);

    return leaf_page_idx;
}

pagenum_t find_leaf(tableid_t table_id, recordkey_t key, trxid_t trx_id) {
    headerpage_t header_page;
    buffered_read_page(table_id, 0, &header_page, 0, false);

    internalpage_t current_page;
    pagenum_t current_page_idx = header_page.root_page_idx;

    if (!current_page_idx) {
        return 0;
    }

    buffered_read_page(table_id, header_page.root_page_idx, &current_page,
                       trx_id, false);
    while (!current_page.page_header.is_leaf_page) {
        int i = 0;
        for (i = 0; i < current_page.page_header.key_num; i++) {
            if (key < current_page.page_branches[i].key) break;
        }
        i--;

        if (i >= 0) {
            current_page_idx = current_page.page_branches[i].page_idx;
        } else {
            current_page_idx = *page_helper::get_leftmost_child_idx(
                reinterpret_cast<internalpage_t*>(&current_page));
        }
        buffered_read_page(table_id, current_page_idx, &current_page, trx_id, false);
    }

    return current_page_idx;
}

bool find_by_key(tableid_t table_id, recordkey_t key, char* value,
                 valsize_t* value_size, trxid_t trx_id) {
    int i = 0;
    leafpage_t leaf_page;
    pagenum_t leaf_page_idx = find_leaf(table_id, key);

    if (!leaf_page_idx) return false;
    buffered_read_page(table_id, leaf_page_idx, &leaf_page, trx_id, false);
    int key_idx = page_helper::get_record_idx(&leaf_page, key);
    
    if (trx_id &&
        !trx_helper::lock_acquire(table_id, leaf_page_idx, key_idx, trx_id, SHARED))
        return false;

    PageSlot* leaf_slot = page_helper::get_page_slot(&leaf_page);

    for (i = 0; i < leaf_page.page_header.key_num; i++) {
        if (leaf_slot[i].key == key) break;
    }

    if (i == leaf_page.page_header.key_num)
        return false;
    else {
        page_helper::get_leaf_value(&leaf_page, i, value, value_size);
        return true;
    }
}

pagenum_t insert_into_new_root(tableid_t table_id, pagenum_t left_page_idx,
                               recordkey_t key, pagenum_t right_page_idx) {
    headerpage_t header_page;
    allocatedpage_t left_page, right_page;

    internalpage_t new_root_page;
    pagenum_t new_root_page_idx;

    new_root_page_idx = make_node(table_id);
    buffered_read_page(table_id, 0, &header_page);
    buffered_read_page(table_id, new_root_page_idx, &new_root_page);
    buffered_read_page(table_id, left_page_idx, &left_page);
    buffered_read_page(table_id, right_page_idx, &right_page);

    *page_helper::get_leftmost_child_idx(&new_root_page) = left_page_idx;
    page_helper::add_internal_key(&new_root_page, key, right_page_idx);

    left_page.page_header.parent_page_idx = new_root_page_idx;
    right_page.page_header.parent_page_idx = new_root_page_idx;

    buffered_write_page(table_id, new_root_page_idx, &new_root_page);
    buffered_write_page(table_id, left_page_idx, &left_page);
    buffered_write_page(table_id, right_page_idx, &right_page);

    header_page.root_page_idx = new_root_page_idx;
    buffered_write_page(table_id, 0, &header_page);

    return new_root_page_idx;
}

pagenum_t insert_into_node(tableid_t table_id, pagenum_t parent_page_idx,
                           pagenum_t left_page_idx, recordkey_t key,
                           pagenum_t right_page_idx) {
    int insert_position;
    internalpage_t parent_page;

    buffered_read_page(table_id, parent_page_idx, &parent_page);

    page_helper::add_internal_key(&parent_page, key, right_page_idx);
    std::sort(
        parent_page.page_branches,
        parent_page.page_branches + parent_page.page_header.key_num,
        [](const PageBranch& a, const PageBranch& b) { return a.key < b.key; });

    buffered_write_page(table_id, parent_page_idx, &parent_page);

    return parent_page_idx;
}

pagenum_t insert_into_node_after_splitting(tableid_t table_id,
                                           pagenum_t page_idx, recordkey_t key,
                                           pagenum_t right_page_idx) {
    pagenum_t new_page_idx;
    internalpage_t page, new_page;

    recordkey_t seperate_key;

    std::vector<PageBranch> temp_branches;

    buffered_read_page(table_id, page_idx, &page);

    /* Create the new node and copy
     * half the keys and pointers to the
     * old and half to the new.
     */
    new_page_idx = make_node(table_id, page.page_header.parent_page_idx);
    buffered_read_page(table_id, new_page_idx, &new_page);

    for (int i = 0; i < 248; i++) {
        temp_branches.push_back(page.page_branches[i]);
    }
    PageBranch new_branch;

    new_branch.key = key;
    new_branch.page_idx = right_page_idx;
    temp_branches.push_back(new_branch);
    std::sort(
        temp_branches.begin(), temp_branches.end(),
        [](const PageBranch& a, const PageBranch& b) { return a.key < b.key; });

    page.page_header.key_num = 0;
    for (int i = 0; i < 124; i++) {
        page_helper::add_internal_key(&page, temp_branches[i].key,
                                      temp_branches[i].page_idx);
    }

    seperate_key = temp_branches[124].key;
    *page_helper::get_leftmost_child_idx(&new_page) =
        temp_branches[124].page_idx;

    allocatedpage_t leftmost_child_page;

    buffered_read_page(table_id, temp_branches[124].page_idx,
                       &leftmost_child_page);
    leftmost_child_page.page_header.parent_page_idx = new_page_idx;
    buffered_write_page(table_id, temp_branches[124].page_idx,
                        &leftmost_child_page);

    for (int i = 125; i < 249; i++) {
        allocatedpage_t child_page;
        page_helper::add_internal_key(&new_page, temp_branches[i].key,
                                      temp_branches[i].page_idx);

        buffered_read_page(table_id, temp_branches[i].page_idx, &child_page);

        child_page.page_header.parent_page_idx = new_page_idx;

        buffered_write_page(table_id, temp_branches[i].page_idx, &child_page);
    }

    /* Insert a new key into the parent of the two
     * nodes resulting from the split, with
     * the old node to the left and the new to the right.
     */

    buffered_write_page(table_id, page_idx, &page);
    buffered_write_page(table_id, new_page_idx, &new_page);

    return insert_into_parent(table_id, page_idx, seperate_key, new_page_idx);
}

pagenum_t insert_into_parent(tableid_t table_id, pagenum_t left_page_idx,
                             recordkey_t key, pagenum_t right_page_idx) {
    allocatedpage_t left_page;

    internalpage_t parent_page;
    pagenum_t parent_page_idx;

    buffered_read_page(table_id, left_page_idx, &left_page, 0, false);

    if (left_page.page_header.parent_page_idx == 0) {
        return insert_into_new_root(table_id, left_page_idx, key,
                                    right_page_idx);
    }

    parent_page_idx = left_page.page_header.parent_page_idx;
    buffered_read_page(table_id, parent_page_idx, &parent_page, 0, false);

    if (parent_page.page_header.key_num < 248)
        return insert_into_node(table_id, parent_page_idx, left_page_idx, key,
                                right_page_idx);

    return insert_into_node_after_splitting(table_id, parent_page_idx, key,
                                            right_page_idx);
}

pagenum_t insert_into_leaf_after_splitting(tableid_t table_id,
                                           pagenum_t leaf_page_idx,
                                           recordkey_t key, const char* value,
                                           valsize_t value_size) {
    recordkey_t new_key;
    leafpage_t leaf_page, new_leaf_page;
    pagenum_t new_leaf_page_idx;
    PageSlot* leaf_slot = page_helper::get_page_slot(&leaf_page);

    buffered_read_page(table_id, leaf_page_idx, &leaf_page);

    int total_values_num = leaf_page.page_header.key_num + 1;
    std::vector<std::pair<PageSlot, const char*>> temp;

    new_leaf_page_idx =
        make_leaf(table_id, leaf_page.page_header.parent_page_idx);
    buffered_read_page(table_id, new_leaf_page_idx, &new_leaf_page);

    for (int i = 0; i < leaf_page.page_header.key_num; i++) {
        char* temp_value = new char[MAX_VALUE_SIZE];
        page_helper::get_leaf_value(&leaf_page, i, temp_value);

        temp.emplace_back(leaf_slot[i], temp_value);
    }
    PageSlot new_slot;
    char* new_value = new char[value_size];

    new_slot.key = key;
    new_slot.value_size = value_size;
    memcpy(new_value, value, value_size);

    temp.emplace_back(new_slot, new_value);

    std::sort(temp.begin(), temp.end(),
              [](const std::pair<PageSlot, const char*>& a,
                 const std::pair<PageSlot, const char*>& b) {
                  return a.first.key < b.first.key;
              });

    int split_start = 0;
    int acc_len = 0;
    for (; split_start < leaf_page.page_header.key_num + 1; split_start++) {
        acc_len += temp[split_start].first.value_size + sizeof(PageSlot);
        if (acc_len >= (PAGE_SIZE - PAGE_HEADER_SIZE) / 2) {
            break;
        }
    }

    leaf_page.page_header.key_num = 0;
    *page_helper::get_free_space(&leaf_page) = PAGE_SIZE - PAGE_HEADER_SIZE;

    for (int i = 0; i < split_start; i++) {
        page_helper::add_leaf_value(&leaf_page, temp[i].first.key,
                                    temp[i].second, temp[i].first.value_size);
    }

    new_key = temp[split_start].first.key;
    for (int i = split_start; i < total_values_num; i++) {
        page_helper::add_leaf_value(&new_leaf_page, temp[i].first.key,
                                    temp[i].second, temp[i].first.value_size);
    }

    uint64_t* leaf_sibling_idx = page_helper::get_sibling_idx(&leaf_page);
    uint64_t* new_leaf_sibling_idx =
        page_helper::get_sibling_idx(&new_leaf_page);

    *new_leaf_sibling_idx = *leaf_sibling_idx;
    *leaf_sibling_idx = new_leaf_page_idx;

    buffered_write_page(table_id, leaf_page_idx, &leaf_page);
    buffered_write_page(table_id, new_leaf_page_idx, &new_leaf_page);

    for (auto& temp_pair : temp) {
        delete[] temp_pair.second;
    }

    return insert_into_parent(table_id, leaf_page_idx, new_key,
                              new_leaf_page_idx);
}

pagenum_t insert_node(tableid_t table_id, recordkey_t key, const char* value,
                      valsize_t value_size) {
    headerpage_t header_page;

    leafpage_t leaf_page;
    pagenum_t leaf_page_idx;

    /* The current implementation ignores
     * duplicates.
     */

    if (find_by_key(table_id, key)) {
        return 0;
    }

    /* Case: the tree does not exist yet.
     * Start a new tree.
     */

    buffered_read_page(table_id, 0, &header_page, 0, false);
    if (header_page.root_page_idx == 0)
        return create_tree(table_id, key, value, value_size);

    /* Case: the tree already exists.
     * (Rest of function body.)
     */

    leaf_page_idx = find_leaf(table_id, key);
    buffered_read_page(table_id, leaf_page_idx, &leaf_page);

    /* Case: leaf has room for key and pointer.
     */

    if (page_helper::has_enough_space(&leaf_page, value_size)) {
        std::vector<std::pair<PageSlot, const char*>> temp;
        PageSlot* leaf_slot = page_helper::get_page_slot(&leaf_page);

        for (int i = 0; i < leaf_page.page_header.key_num; i++) {
            char* temp_value = new char[MAX_VALUE_SIZE];
            page_helper::get_leaf_value(&leaf_page, i, temp_value);

            temp.emplace_back(leaf_slot[i], temp_value);
        }

        PageSlot new_slot;
        char* new_value = new char[value_size];

        new_slot.key = key;
        new_slot.value_size = value_size;
        memcpy(new_value, value, value_size);

        temp.emplace_back(new_slot, new_value);

        std::sort(temp.begin(), temp.end(),
                  [](const std::pair<PageSlot, const char*>& a,
                     const std::pair<PageSlot, const char*>& b) {
                      return a.first.key < b.first.key;
                  });

        leaf_page.page_header.key_num = 0;
        *page_helper::get_free_space(&leaf_page) = PAGE_SIZE - PAGE_HEADER_SIZE;

        for (auto& temp_pair : temp) {
            page_helper::add_leaf_value(&leaf_page, temp_pair.first.key,
                                        temp_pair.second,
                                        temp_pair.first.value_size);
            delete[] temp_pair.second;
        }

        buffered_write_page(table_id, leaf_page_idx, &leaf_page);
        return leaf_page_idx;
    } else {
        buffered_release_page(table_id, leaf_page_idx);
    }

    /* Case:  leaf must be split.
     */

    return insert_into_leaf_after_splitting(table_id, leaf_page_idx, key, value,
                                            value_size);
}

pagenum_t adjust_root(tableid_t table_id) {
    headerpage_t header_page;
    allocatedpage_t root_page, new_root_page;

    buffered_read_page(table_id, 0, &header_page, 0, false);
    buffered_read_page(table_id, header_page.root_page_idx, &root_page, 0,
                       false);

    /* Case: nonempty root.
     * Key and pointer have already been deleted,
     * so nothing to be done.
     */

    if (root_page.page_header.key_num > 0) return header_page.root_page_idx;

    /* Case: empty root.
     */

    // If it has a child, promote
    // the first (only) child
    // as the new root.

    buffered_free_page(table_id, header_page.root_page_idx);
    buffered_read_page(table_id, 0, &header_page);

    if (!root_page.page_header.is_leaf_page) {
        header_page.root_page_idx = *page_helper::get_leftmost_child_idx(
            reinterpret_cast<internalpage_t*>(&root_page));
        buffered_read_page(table_id, header_page.root_page_idx, &new_root_page);
        new_root_page.page_header.parent_page_idx = 0;
        buffered_write_page(table_id, header_page.root_page_idx,
                            &new_root_page);
    } else {
        header_page.root_page_idx = 0;
        buffered_write_page(table_id, 0, &header_page);
        return 1;
    }

    buffered_write_page(table_id, 0, &header_page);

    return header_page.root_page_idx;
}

pagenum_t coalesce_internal_nodes(tableid_t table_id, pagenum_t left_page_idx,
                                  recordkey_t seperate_key,
                                  int seperate_key_idx,
                                  pagenum_t right_page_idx) {
    headerpage_t header_page;

    recordkey_t old_key;
    internalpage_t parent_page;
    internalpage_t left_page, right_page;
    allocatedpage_t child_page;
    PageSlot* right_slot;

    buffered_read_page(table_id, 0, &header_page, 0, false);
    buffered_read_page(table_id, left_page_idx, &left_page);
    buffered_read_page(table_id, right_page_idx, &right_page, 0, false);
    buffered_read_page(table_id, left_page.page_header.parent_page_idx,
                       &parent_page);

    page_helper::add_internal_key(
        &left_page, seperate_key,
        *page_helper::get_leftmost_child_idx(&right_page));

    buffered_read_page(table_id,
                       *page_helper::get_leftmost_child_idx(&right_page),
                       &child_page);
    child_page.page_header.parent_page_idx = left_page_idx;
    buffered_write_page(table_id,
                        *page_helper::get_leftmost_child_idx(&right_page),
                        &child_page);

    for (int i = 0; i < right_page.page_header.key_num; i++) {
        allocatedpage_t child_page;
        page_helper::add_internal_key(&left_page,
                                      right_page.page_branches[i].key,
                                      right_page.page_branches[i].page_idx);

        buffered_read_page(table_id, right_page.page_branches[i].page_idx,
                           &child_page);
        child_page.page_header.parent_page_idx = left_page_idx;
        buffered_write_page(table_id, right_page.page_branches[i].page_idx,
                            &child_page);
    }

    buffered_write_page(table_id, left_page_idx, &left_page);
    buffered_free_page(table_id, right_page_idx);
    buffered_release_page(table_id, left_page.page_header.parent_page_idx);

    for (int i = 0; i < parent_page.page_header.key_num; i++) {
        if (parent_page.page_branches[i].page_idx == right_page_idx) {
            delete_internal_key(table_id, left_page.page_header.parent_page_idx,
                                parent_page.page_branches[i].key);
            break;
        }
    }

    return header_page.root_page_idx;
}

pagenum_t coalesce_leaf_nodes(tableid_t table_id, pagenum_t left_page_idx,
                              pagenum_t right_page_idx) {
    headerpage_t header_page;

    recordkey_t old_key;
    internalpage_t parent_page;
    leafpage_t left_page, right_page;
    PageSlot* right_slot;

    buffered_read_page(table_id, 0, &header_page, 0, false);
    buffered_read_page(table_id, left_page_idx, &left_page);
    buffered_read_page(table_id, right_page_idx, &right_page, 0, false);
    buffered_read_page(table_id, left_page.page_header.parent_page_idx,
                       &parent_page, 0, false);

    /* In a leaf, append the keys and pointers of
     * n to the neighbor.
     * Set the neighbor's last pointer to point to
     * what had been n's right neighbor.
     */

    right_slot = page_helper::get_page_slot(&right_page);

    for (int i = 0; i < right_page.page_header.key_num; i++) {
        char right_slot_value[MAX_VALUE_SIZE];
        page_helper::get_leaf_value(&right_page, i, right_slot_value);
        page_helper::add_leaf_value(&left_page, right_slot[i].key,
                                    right_slot_value, right_slot[i].value_size);
    }
    *page_helper::get_sibling_idx(&left_page) =
        *page_helper::get_sibling_idx(&right_page);

    buffered_write_page(table_id, left_page_idx, &left_page);
    buffered_free_page(table_id, right_page_idx);

    for (int i = 0; i < parent_page.page_header.key_num; i++) {
        if (parent_page.page_branches[i].page_idx == right_page_idx) {
            delete_internal_key(table_id, left_page.page_header.parent_page_idx,
                                parent_page.page_branches[i].key);
            break;
        }
    }

    return header_page.root_page_idx;
}

pagenum_t delete_internal_key(tableid_t table_id, pagenum_t internal_page_idx,
                              recordkey_t key) {
    headerpage_t header_page;

    int seperate_key_idx;
    recordkey_t seperate_key;
    bool left_sibling = false;

    pagenum_t sibling_page_idx = 0;
    internalpage_t internal_page, sibling_page;

    pagenum_t parent_page_idx;
    internalpage_t parent_page;

    buffered_read_page(table_id, 0, &header_page, 0, false);
    buffered_read_page(table_id, internal_page_idx, &internal_page);
    page_helper::remove_internal_key(&internal_page, key);
    buffered_write_page(table_id, internal_page_idx, &internal_page);

    parent_page_idx = internal_page.page_header.parent_page_idx;

    if (internal_page_idx == header_page.root_page_idx)
        return adjust_root(table_id);

    if (internal_page.page_header.key_num >= 124)
        return header_page.root_page_idx;

    buffered_read_page(table_id, parent_page_idx, &parent_page);
    if (*page_helper::get_leftmost_child_idx(&parent_page) ==
        internal_page_idx) {
        seperate_key_idx = 0;
        seperate_key = parent_page.page_branches[0].key;
        sibling_page_idx = parent_page.page_branches[0].page_idx;
    }
    for (int i = 0; i < parent_page.page_header.key_num - 1; i++) {
        if (parent_page.page_branches[i].page_idx == internal_page_idx) {
            seperate_key_idx = i + 1;
            seperate_key = parent_page.page_branches[i + 1].key;
            sibling_page_idx = parent_page.page_branches[i + 1].page_idx;
        }
    }

    if (sibling_page_idx == 0) {
        if (parent_page.page_header.key_num < 2) {
            seperate_key_idx = 0;
            seperate_key = parent_page.page_branches[0].key;
            sibling_page_idx =
                *page_helper::get_leftmost_child_idx(&parent_page);
        } else {
            seperate_key_idx = parent_page.page_header.key_num - 1;
            seperate_key =
                parent_page.page_branches[parent_page.page_header.key_num - 1]
                    .key;
            sibling_page_idx =
                parent_page.page_branches[parent_page.page_header.key_num - 2]
                    .page_idx;
        }
        left_sibling = true;
    }

    buffered_read_page(table_id, sibling_page_idx, &sibling_page);

    /* Coalescence. */

    if (internal_page.page_header.key_num + sibling_page.page_header.key_num <
        248) {
        buffered_release_page(table_id, parent_page_idx);
        buffered_release_page(table_id, sibling_page_idx);
        if (!left_sibling)
            return coalesce_internal_nodes(table_id, internal_page_idx,
                                           seperate_key, seperate_key_idx,
                                           sibling_page_idx);
        else
            return coalesce_internal_nodes(table_id, sibling_page_idx,
                                           seperate_key, seperate_key_idx,
                                           internal_page_idx);
    } else {
        buffered_read_page(table_id, internal_page_idx, &internal_page);

        allocatedpage_t leftmost_child_page;
        if (!left_sibling) {
            parent_page.page_branches[seperate_key_idx].key =
                sibling_page.page_branches[0].key;
            page_helper::add_internal_key(
                &internal_page, seperate_key,
                *page_helper::get_leftmost_child_idx(&sibling_page));

            buffered_read_page(
                table_id, *page_helper::get_leftmost_child_idx(&sibling_page),
                &leftmost_child_page);
            leftmost_child_page.page_header.parent_page_idx = internal_page_idx;
            buffered_write_page(
                table_id, *page_helper::get_leftmost_child_idx(&sibling_page),
                &leftmost_child_page);

            *page_helper::get_leftmost_child_idx(&sibling_page) =
                sibling_page.page_branches[0].page_idx;

            page_helper::remove_internal_key(&sibling_page,
                                             sibling_page.page_branches[0].key);
        } else {
            for (int i = internal_page.page_header.key_num; i > 0; i--) {
                internal_page.page_branches[i] =
                    internal_page.page_branches[i - 1];
            }
            internal_page.page_branches[0].key = seperate_key;
            internal_page.page_branches[0].page_idx =
                *page_helper::get_leftmost_child_idx(&internal_page);

            internal_page.page_header.key_num++;

            parent_page.page_branches[seperate_key_idx].key =
                sibling_page.page_branches[sibling_page.page_header.key_num - 1]
                    .key;

            *page_helper::get_leftmost_child_idx(&internal_page) =
                sibling_page.page_branches[sibling_page.page_header.key_num - 1]
                    .page_idx;

            buffered_read_page(
                table_id, *page_helper::get_leftmost_child_idx(&internal_page),
                &leftmost_child_page);
            leftmost_child_page.page_header.parent_page_idx = internal_page_idx;
            buffered_write_page(
                table_id, *page_helper::get_leftmost_child_idx(&internal_page),
                &leftmost_child_page);

            page_helper::remove_internal_key(
                &sibling_page,
                sibling_page.page_branches[sibling_page.page_header.key_num - 1]
                    .key);
        }

        buffered_write_page(table_id, internal_page_idx, &internal_page);
        buffered_write_page(table_id, sibling_page_idx, &sibling_page);
        buffered_write_page(table_id, parent_page_idx, &parent_page);
    }

    return header_page.root_page_idx;
}

pagenum_t delete_leaf_key(tableid_t table_id, pagenum_t leaf_page_idx,
                          recordkey_t key) {
    headerpage_t header_page;

    int seperate_key_idx = 99999;
    bool left_sibling = false;

    pagenum_t sibling_page_idx;
    leafpage_t leaf_page, sibling_page;

    pagenum_t parent_page_idx;
    internalpage_t parent_page;

    buffered_read_page(table_id, 0, &header_page, 0, false);
    buffered_read_page(table_id, leaf_page_idx, &leaf_page);
    page_helper::remove_leaf_value(&leaf_page, key);
    buffered_write_page(table_id, leaf_page_idx, &leaf_page);

    parent_page_idx = leaf_page.page_header.parent_page_idx;

    if (leaf_page_idx == header_page.root_page_idx)
        return adjust_root(table_id);

    buffered_read_page(table_id, parent_page_idx, &parent_page);

    if (*page_helper::get_free_space(&leaf_page) < REDISTRIBUTE_THRESHOLD) {
        buffered_release_page(table_id, parent_page_idx);
        return leaf_page_idx;
    }

    sibling_page_idx = *page_helper::get_sibling_idx(&leaf_page);
    for (int i = 0; i < parent_page.page_header.key_num; i++) {
        if (parent_page.page_branches[i].page_idx == sibling_page_idx) {
            seperate_key_idx = i;
        }
    }

    if (sibling_page_idx == 0) {
        if (parent_page.page_header.key_num < 2) {
            seperate_key_idx = 0;
            sibling_page_idx =
                *page_helper::get_leftmost_child_idx(&parent_page);
        } else {
            seperate_key_idx = parent_page.page_header.key_num - 1;
            sibling_page_idx =
                parent_page.page_branches[parent_page.page_header.key_num - 2]
                    .page_idx;
        }
        left_sibling = true;
    }
    buffered_read_page(table_id, sibling_page_idx, &sibling_page);

    if (sibling_page.page_header.parent_page_idx !=
        leaf_page.page_header.parent_page_idx) {
        buffered_release_page(table_id, sibling_page_idx);
        if (parent_page.page_header.key_num < 2) {
            seperate_key_idx = 0;
            sibling_page_idx =
                *page_helper::get_leftmost_child_idx(&parent_page);
        } else {
            seperate_key_idx = parent_page.page_header.key_num - 1;
            sibling_page_idx =
                parent_page.page_branches[parent_page.page_header.key_num - 2]
                    .page_idx;
        }
        left_sibling = true;
        buffered_read_page(table_id, sibling_page_idx, &sibling_page);
    }

    if (*page_helper::get_free_space(&leaf_page) +
            *page_helper::get_free_space(&sibling_page) >=
        PAGE_SIZE - PAGE_HEADER_SIZE) {
        buffered_release_page(table_id, parent_page_idx);
        buffered_release_page(table_id, sibling_page_idx);
        if (!left_sibling)
            return coalesce_leaf_nodes(table_id, leaf_page_idx,
                                       sibling_page_idx);
        else
            return coalesce_leaf_nodes(table_id, sibling_page_idx,
                                       leaf_page_idx);
    } else {
        buffered_read_page(table_id, leaf_page_idx, &leaf_page);
        if (!left_sibling) {
            PageSlot temp_slot;
            PageSlot* sibling_slot = page_helper::get_page_slot(&sibling_page);

            while (sibling_page.page_header.key_num > 0 &&
                   *page_helper::get_free_space(&leaf_page) >=
                       REDISTRIBUTE_THRESHOLD) {
                char* temp_value = new char[MAX_VALUE_SIZE];
                page_helper::get_leaf_value(&sibling_page, 0, temp_value);
                page_helper::add_leaf_value(&leaf_page, sibling_slot[0].key,
                                            temp_value,
                                            sibling_slot[0].value_size);

                page_helper::remove_leaf_value(&sibling_page,
                                               sibling_slot[0].key);
                delete[] temp_value;
            }

            parent_page.page_branches[seperate_key_idx].key =
                sibling_slot[0].key;
        } else {
            std::vector<std::pair<PageSlot, const char*>> temp;

            PageSlot* leaf_slot = page_helper::get_page_slot(&leaf_page);
            PageSlot* sibling_slot = page_helper::get_page_slot(&sibling_page);
            uint16_t temp_free_space = *page_helper::get_free_space(&leaf_page);

            while (sibling_page.page_header.key_num > 0 &&
                   temp_free_space >= REDISTRIBUTE_THRESHOLD) {
                char* temp_value = new char[MAX_VALUE_SIZE];
                page_helper::get_leaf_value(
                    &sibling_page, sibling_page.page_header.key_num - 1,
                    temp_value);

                temp.emplace_back(
                    sibling_slot[sibling_page.page_header.key_num - 1],
                    temp_value);
                temp_free_space -=
                    sibling_slot[sibling_page.page_header.key_num - 1]
                        .value_size +
                    sizeof(PageSlot);

                page_helper::remove_leaf_value(
                    &sibling_page,
                    sibling_slot[sibling_page.page_header.key_num - 1].key);
            }

            std::reverse(temp.begin(), temp.end());

            for (int i = 0; i < leaf_page.page_header.key_num; i++) {
                char* temp_value = new char[MAX_VALUE_SIZE];
                page_helper::get_leaf_value(&leaf_page, i, temp_value);

                temp.emplace_back(leaf_slot[i], temp_value);
            }

            leaf_page.page_header.key_num = 0;
            *page_helper::get_free_space(&leaf_page) =
                PAGE_SIZE - PAGE_HEADER_SIZE;

            for (auto& temp_pair : temp) {
                page_helper::add_leaf_value(&leaf_page, temp_pair.first.key,
                                            temp_pair.second,
                                            temp_pair.first.value_size);
                delete[] temp_pair.second;
            }

            parent_page.page_branches[seperate_key_idx].key = leaf_slot[0].key;
        }

        buffered_write_page(table_id, leaf_page_idx, &leaf_page);
        buffered_write_page(table_id, sibling_page_idx, &sibling_page);
        buffered_write_page(table_id, parent_page_idx, &parent_page);
    }

    return header_page.root_page_idx;
}

pagenum_t delete_node(tableid_t table_id, recordkey_t key) {
    pagenum_t leaf_page_idx = find_leaf(table_id, key);

    if (find_by_key(table_id, key) && leaf_page_idx) {
        return delete_leaf_key(table_id, leaf_page_idx, key);
        // free(key_record);
    }

    return 0;
}

pagenum_t update_node(tableid_t table_id, recordkey_t key, const char* value,
                      valsize_t new_val_size, valsize_t* old_val_size,
                      trxid_t trx_id) {
    if (!find_by_key(table_id, key)) {
        return 0;
    }
    pagenum_t leaf_page_idx = find_leaf(table_id, key);

    if (leaf_page_idx == 0) return 0;

    leafpage_t leaf_page;
    buffered_read_page(table_id, leaf_page_idx, &leaf_page, trx_id, false);

    int key_idx = page_helper::get_record_idx(&leaf_page, key);

    if (!trx_helper::lock_acquire(table_id, leaf_page_idx, key_idx, trx_id,
                                  EXCLUSIVE)) {
        return 0;
    }
    buffered_read_page(table_id, leaf_page_idx, &leaf_page, trx_id, true);

    char* old_value = new char[MAX_VALUE_SIZE];

    if (page_helper::set_leaf_value(&leaf_page, key, old_value, old_val_size, value,
                                    new_val_size)) {
        //trx_helper::log_update(table_id, key, old_value, *old_val_size, trx_id);
        buffered_write_page(table_id, leaf_page_idx, &leaf_page);
        
        delete[] old_value;
        return leaf_page_idx;
    }

    buffered_release_page(table_id, leaf_page_idx);

    delete[] old_value;
    return 0;
}
```


-------------------------------

Updated on 2021-12-05 at 18:36:40 +0900
