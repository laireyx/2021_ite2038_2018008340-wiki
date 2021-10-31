

# IndexManager



## Functions

|                | Name           |
| -------------- | -------------- |
| pagenum_t | **[make_leaf](/Modules/IndexManager#function-make_leaf)**(tableid_t table_id, pagenum_t parent_page_idx =0)<br>Allocate and make a leaf page.  |
| pagenum_t | **[make_node](/Modules/IndexManager#function-make_node)**(tableid_t table_id, pagenum_t parent_page_idx =0)<br>Allocate and make an internal page.  |
| pagenum_t | **[create_tree](/Modules/IndexManager#function-create_tree)**(tableid_t table_id, int64_t key, const char * value, uint16_t value_size)<br>Create a new tree.  |
| pagenum_t | **[find_leaf](/Modules/IndexManager#function-find_leaf)**(tableid_t table_id, int64_t key)<br>Find a leaf node which contains given key.  |
| bool | **[find_by_key](/Modules/IndexManager#function-find_by_key)**(tableid_t table_id, int64_t key, char * value =nullptr, uint16_t * value_size =nullptr)<br>Find a record with key.  |
| pagenum_t | **[insert_into_node](/Modules/IndexManager#function-insert_into_node)**(tableid_t table_id, pagenum_t parent_page_idx, pagenum_t left_page_idx, int64_t key, pagenum_t right_page_idx)<br>Insert a <code>(key, right&#95;page&#95;idx)</code> tuple in parent page.  |
| pagenum_t | **[insert_into_node_after_splitting](/Modules/IndexManager#function-insert_into_node_after_splitting)**(tableid_t table_id, pagenum_t parent_page_idx, int64_t key, pagenum_t right_page_idx)<br>Insert a <code>(key, right&#95;page&#95;idx)</code> tuple in parent page, and split it into two pages.  |
| pagenum_t | **[insert_into_parent](/Modules/IndexManager#function-insert_into_parent)**(tableid_t table_id, pagenum_t left_page_idx, int64_t key, pagenum_t right_page_idx)<br>Choose right method between just inserting and <code>insert&#95;into&#95;node&#95;after&#95;splitting</code> and call it.  |
| pagenum_t | **[insert_into_leaf_after_splitting](/Modules/IndexManager#function-insert_into_leaf_after_splitting)**(tableid_t table_id, pagenum_t leaf_page_idx, int64_t key, const char * value, uint16_t value_size)<br>Insert <code>(key, value)</code>into leaf node and split it into two pages.  |
| pagenum_t | **[insert_node](/Modules/IndexManager#function-insert_node)**(tableid_t table_id, int64_t key, const char * value, uint16_t value_size)<br>Find appropriate leaf page and insert a record into it.  |
| pagenum_t | **[adjust_root](/Modules/IndexManager#function-adjust_root)**(tableid_t table_id)<br>Adjust root page.  |
| pagenum_t | **[coalesce_internal_nodes](/Modules/IndexManager#function-coalesce_internal_nodes)**(tableid_t table_id, pagenum_t left_page_idx, int64_t seperate_key, int seperate_key_idx, pagenum_t right_page_idx)<br>Coalesces two internal pages.  |
| pagenum_t | **[coalesce_leaf_nodes](/Modules/IndexManager#function-coalesce_leaf_nodes)**(tableid_t table_id, pagenum_t left_page_idx, pagenum_t right_page_idx)<br>Coalesces two leaf pages.  |
| pagenum_t | **[delete_internal_key](/Modules/IndexManager#function-delete_internal_key)**(tableid_t table_id, pagenum_t internal_page_idx, int64_t key)<br>Delete a page branch from internal page.  |
| pagenum_t | **[delete_leaf_key](/Modules/IndexManager#function-delete_leaf_key)**(tableid_t table_id, pagenum_t leaf_page_idx, int64_t key)<br>Delete a record from leaf page.  |
| pagenum_t | **[delete_node](/Modules/IndexManager#function-delete_node)**(tableid_t table_id, int64_t key)<br>Entrance for remove a record from table.  |
| pagenum_t | **[insert_into_new_root](/Modules/IndexManager#function-insert_into_new_root)**(tableid_t table_id, pagenum_t left_page_idx, int64_t key, pagenum_t right_page_idx) |

## Attributes

|                | Name           |
| -------------- | -------------- |
| constexpr int | **[REDISTRIBUTE_THRESHOLD](/Modules/IndexManager#variable-redistribute_threshold)**  |
| constexpr int | **[MAX_VALUE_SIZE](/Modules/IndexManager#variable-max_value_size)**  |


## Functions Documentation

### function make_leaf

```
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

### function make_node

```
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

### function create_tree

```
pagenum_t create_tree(
    tableid_t table_id,
    int64_t key,
    const char * value,
    uint16_t value_size
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

```
pagenum_t find_leaf(
    tableid_t table_id,
    int64_t key
)
```

Find a leaf node which contains given key. 

**Parameters**: 

  * **table_id** table id. 
  * **key** key to query with. 


**Return**: page index if found. <code>0</code> if the key does not exist. 

### function find_by_key

```
bool find_by_key(
    tableid_t table_id,
    int64_t key,
    char * value =nullptr,
    uint16_t * value_size =nullptr
)
```

Find a record with key. 

**Parameters**: 

  * **table_id** table id. 
  * **key** key to query with. 
  * **value** If the record is found successful, then this value is set to record value. Caller should allocate enough(MAX_VALUE_SIZE) memory for it. 
  * **value_size** If the record is found successful, then this value is set to record size. 


**Return**: <code>true</code> if found successful. <code>false</code> otherwise. 

### function insert_into_node

```
pagenum_t insert_into_node(
    tableid_t table_id,
    pagenum_t parent_page_idx,
    pagenum_t left_page_idx,
    int64_t key,
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

```
pagenum_t insert_into_node_after_splitting(
    tableid_t table_id,
    pagenum_t parent_page_idx,
    int64_t key,
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

```
pagenum_t insert_into_parent(
    tableid_t table_id,
    pagenum_t left_page_idx,
    int64_t key,
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

```
pagenum_t insert_into_leaf_after_splitting(
    tableid_t table_id,
    pagenum_t leaf_page_idx,
    int64_t key,
    const char * value,
    uint16_t value_size
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

```
pagenum_t insert_node(
    tableid_t table_id,
    int64_t key,
    const char * value,
    uint16_t value_size
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

```
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

```
pagenum_t coalesce_internal_nodes(
    tableid_t table_id,
    pagenum_t left_page_idx,
    int64_t seperate_key,
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

```
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

```
pagenum_t delete_internal_key(
    tableid_t table_id,
    pagenum_t internal_page_idx,
    int64_t key
)
```

Delete a page branch from internal page. 

**Parameters**: 

  * **table_id** table id. 
  * **internal_page_idx** internal page index. 
  * **key** branch key. 


**Return**: root page number. 

### function delete_leaf_key

```
pagenum_t delete_leaf_key(
    tableid_t table_id,
    pagenum_t leaf_page_idx,
    int64_t key
)
```

Delete a record from leaf page. 

**Parameters**: 

  * **table_id** table id. 
  * **leaf_page_idx** leaf page index. 
  * **key** record key. 


**Return**: root page number. 

### function delete_node

```
pagenum_t delete_node(
    tableid_t table_id,
    int64_t key
)
```

Entrance for remove a record from table. 

**Parameters**: 

  * **table_id** table id. 
  * **key** record key. 


**Return**: root page number. 

### function insert_into_new_root

```
pagenum_t insert_into_new_root(
    tableid_t table_id,
    pagenum_t left_page_idx,
    int64_t key,
    pagenum_t right_page_idx
)
```



## Attributes Documentation

### variable REDISTRIBUTE_THRESHOLD

```
constexpr int REDISTRIBUTE_THRESHOLD = 2500;
```


### variable MAX_VALUE_SIZE

```
constexpr int MAX_VALUE_SIZE = 112;
```





-------------------------------

Updated on 2021-10-31 at 22:47:05 +0900