---
title: DiskBasedBpt

---

# DiskBasedBpt



## Functions

|                | Name           |
| -------------- | -------------- |
| pagenum_t | **[make_leaf](/Modules/group__DiskBasedBpt#function-make-leaf)**(tableid_t table_id, pagenum_t parent_page_idx =0)<br>Allocate and make a leaf page.  |
| pagenum_t | **[make_node](/Modules/group__DiskBasedBpt#function-make-node)**(tableid_t table_id, pagenum_t parent_page_idx =0)<br>Allocate and make an internal page.  |
| pagenum_t | **[create_tree](/Modules/group__DiskBasedBpt#function-create-tree)**(tableid_t table_id, int64_t key, const char * value, uint16_t value_size)<br>Create a new tree.  |
| pagenum_t | **[find_leaf](/Modules/group__DiskBasedBpt#function-find-leaf)**(tableid_t table_id, int64_t key)<br>Find a leaf node which contains given key.  |
| bool | **[find_by_key](/Modules/group__DiskBasedBpt#function-find-by-key)**(tableid_t table_id, int64_t key, char * value =nullptr, uint16_t * value_size =nullptr)<br>Find a record with key.  |
| pagenum_t | **[insert_into_node](/Modules/group__DiskBasedBpt#function-insert-into-node)**(tableid_t table_id, pagenum_t parent_page_idx, pagenum_t left_page_idx, int64_t key, pagenum_t right_page_idx)<br>Insert a <code>(key, right&#95;page&#95;idx)</code> tuple in parent page.  |
| pagenum_t | **[insert_into_node_after_splitting](/Modules/group__DiskBasedBpt#function-insert-into-node-after-splitting)**(tableid_t table_id, pagenum_t parent_page_idx, int64_t key, pagenum_t right_page_idx)<br>Insert a <code>(key, right&#95;page&#95;idx)</code> tuple in parent page, and split it into two pages.  |
| pagenum_t | **[insert_into_parent](/Modules/group__DiskBasedBpt#function-insert-into-parent)**(tableid_t table_id, pagenum_t left_page_idx, int64_t key, pagenum_t right_page_idx)<br>Choose right method between just inserting and <code>insert&#95;into&#95;node&#95;after&#95;splitting</code> and call it.  |
| pagenum_t | **[insert_into_leaf_after_splitting](/Modules/group__DiskBasedBpt#function-insert-into-leaf-after-splitting)**(tableid_t table_id, pagenum_t leaf_page_idx, int64_t key, const char * value, uint16_t value_size)<br>Insert <code>(key, value)</code>into leaf node and split it into two pages.  |
| pagenum_t | **[insert_node](/Modules/group__DiskBasedBpt#function-insert-node)**(tableid_t table_id, int64_t key, const char * value, uint16_t value_size)<br>Find appropriate leaf page and insert a record into it.  |
| pagenum_t | **[adjust_root](/Modules/group__DiskBasedBpt#function-adjust-root)**(tableid_t table_id)<br>Adjust root page.  |
| pagenum_t | **[coalesce_internal_nodes](/Modules/group__DiskBasedBpt#function-coalesce-internal-nodes)**(tableid_t table_id, pagenum_t left_page_idx, int64_t seperate_key, int seperate_key_idx, pagenum_t right_page_idx)<br>Coalesces two internal pages.  |
| pagenum_t | **[coalesce_leaf_nodes](/Modules/group__DiskBasedBpt#function-coalesce-leaf-nodes)**(tableid_t table_id, pagenum_t left_page_idx, pagenum_t right_page_idx)<br>Coalesces two leaf pages.  |
| pagenum_t | **[delete_internal_key](/Modules/group__DiskBasedBpt#function-delete-internal-key)**(tableid_t table_id, pagenum_t internal_page_idx, int64_t key)<br>Delete a page branch from internal page.  |
| pagenum_t | **[delete_leaf_key](/Modules/group__DiskBasedBpt#function-delete-leaf-key)**(tableid_t table_id, pagenum_t leaf_page_idx, int64_t key)<br>Delete a record from leaf page.  |
| pagenum_t | **[delete_node](/Modules/group__DiskBasedBpt#function-delete-node)**(tableid_t table_id, int64_t key)<br>Entrance for remove a record from table.  |
| pagenum_t | **[insert_into_new_root](/Modules/group__DiskBasedBpt#function-insert-into-new-root)**(tableid_t table_id, pagenum_t left_page_idx, int64_t key, pagenum_t right_page_idx) |

## Attributes

|                | Name           |
| -------------- | -------------- |
| constexpr int | **[REDISTRIBUTE_THRESHOLD](/Modules/group__DiskBasedBpt#variable-redistribute-threshold)**  |
| constexpr int | **[MAX_VALUE_SIZE](/Modules/group__DiskBasedBpt#variable-max-value-size)**  |


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

  * **table_id** Table id. 
  * **parent_page_idx** Parent page index. 


**Return**: Created page index. 

### function make_node

```
pagenum_t make_node(
    tableid_t table_id,
    pagenum_t parent_page_idx =0
)
```

Allocate and make an internal page. 

**Parameters**: 

  * **table_id** Table id. 
  * **parent_page_idx** Parent page index. 


**Return**: Created page index. 

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

  * **table_id** Table id. 
  * **key** Initial record key. 
  * **value** Initial record value. 
  * **value_size** Initial record value size. 


**Return**: Root page index. 

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

  * **table_id** Table id. 
  * **key** Key to query with. 


**Return**: <a href="/Classes/structPage">Page</a> index if found. <code>0</code> if the key does not exist. 

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

  * **table_id** Table id. 
  * **key** Key to query with. 
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

  * **table_id** Table id. 
  * **parent_page_idx** Parent page index. 
  * **left_page_idx** Left page index. 
  * **key** Key which means right page. 
  * **right_page_idx** Right page index. 


**Return**: Root page number. 

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

  * **table_id** Table id. 
  * **parent_page_idx** Parent page index. 
  * **key** Key which means right page. 
  * **right_page_idx** Right page index. 


**Return**: Root page number. 

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

  * **table_id** Table id. 
  * **left_page_idx** Left page index. 
  * **key** Key which means right page. 
  * **right_page_idx** Right page index. 


**Return**: Root page number. 

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

  * **table_id** Table id. 
  * **leaf_page_idx** Leaf page index. 
  * **key** Record key. 
  * **value** Record value. 
  * **value_size** Record value size. 


**Return**: Root page number. 

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

  * **table_id** Table id. 
  * **key** Record key. 
  * **value** Record value. 
  * **value_size** Record value size. 


**Return**: Root page number. 

### function adjust_root

```
pagenum_t adjust_root(
    tableid_t table_id
)
```

Adjust root page. 

**Parameters**: 

  * **table_id** Table id. 


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

  * **table_id** Table id. 
  * **left_page_idx** Left page index. 
  * **seperate_key** Key which can seperate between left and right page. 
  * **seperate_key_idx** Parent branch index of <code>seperate&#95;key</code>. 
  * **right_page_idx** Right page index. 


**Return**: Root page number. 

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

  * **table_id** Table id. 
  * **left_page_idx** Left page index. 
  * **right_page_idx** Right page index. 


**Return**: Root page number. 

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

  * **table_id** Table id. 
  * **internal_page_idx** Internal page index. 
  * **key** Branch key. 


**Return**: Root page number. 

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

  * **table_id** Table id. 
  * **leaf_page_idx** Leaf page index. 
  * **key** Record key. 


**Return**: Root page number. 

### function delete_node

```
pagenum_t delete_node(
    tableid_t table_id,
    int64_t key
)
```

Entrance for remove a record from table. 

**Parameters**: 

  * **table_id** Table id. 
  * **key** Record key. 


**Return**: Root page number. 

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

Updated on 2021-10-16 at 22:36:54 +0900