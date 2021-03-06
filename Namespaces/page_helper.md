

# page_helper

**Module:** **[DiskSpaceManager](/Modules/DiskSpaceManager)**

<a href="/Classes/Page">Page</a> helper.  [More...](#detailed-description)

## Functions

|                | Name           |
| -------------- | -------------- |
| <a href="/Classes/PageSlot">PageSlot</a> * | **[get_page_slot](/Namespaces/page_helper#function-get_page_slot)**(<a href="/Classes/LeafPage">LeafPage</a> * page)<br>Get page slots.  |
| int | **[get_record_idx](/Namespaces/page_helper#function-get_record_idx)**(<a href="/Classes/LeafPage">LeafPage</a> * page, recordkey_t key)<br>Get the record index.  |
| void | **[get_leaf_value](/Namespaces/page_helper#function-get_leaf_value)**(<a href="/Classes/LeafPage">LeafPage</a> * page, int value_idx, char * value, valsize_t * value_size =nullptr)<br>Get leaf value.  |
| void | **[get_leaf_value](/Namespaces/page_helper#function-get_leaf_value)**(<a href="/Classes/LeafPage">LeafPage</a> * page, uint16_t value_offset, valsize_t value_size, char * value)<br>Get leaf value.  |
| bool | **[has_enough_space](/Namespaces/page_helper#function-has_enough_space)**(<a href="/Classes/LeafPage">LeafPage</a> * page, valsize_t value_size)<br>Check if given page has enough space for given value size.  |
| uint64_t * | **[get_free_space](/Namespaces/page_helper#function-get_free_space)**(<a href="/Classes/LeafPage">LeafPage</a> * page)<br>Get free space amount.  |
| pagenum_t * | **[get_sibling_idx](/Namespaces/page_helper#function-get_sibling_idx)**(<a href="/Classes/LeafPage">LeafPage</a> * page)<br>Get next sibling index.  |
| bool | **[add_leaf_value](/Namespaces/page_helper#function-add_leaf_value)**(<a href="/Classes/LeafPage">LeafPage</a> * page, recordkey_t key, const char * value, valsize_t value_size)<br>Add a leaf value into the last position of the leaf page.  |
| bool | **[remove_leaf_value](/Namespaces/page_helper#function-remove_leaf_value)**(<a href="/Classes/LeafPage">LeafPage</a> * page, recordkey_t key)<br>Remove a record and compact reserved area in the leaf page.  |
| void | **[set_leaf_value](/Namespaces/page_helper#function-set_leaf_value)**(<a href="/Classes/LeafPage">LeafPage</a> * page, int key_idx, char * old_value, valsize_t * old_val_size, const char * new_value, valsize_t new_val_size)<br>Update the record value in the page and returns old record value size.  |
| bool | **[add_internal_key](/Namespaces/page_helper#function-add_internal_key)**(<a href="/Classes/InternalPage">InternalPage</a> * page, recordkey_t key, pagenum_t page_idx)<br>Add a page branch into the last position of the internal page.  |
| bool | **[remove_internal_key](/Namespaces/page_helper#function-remove_internal_key)**(<a href="/Classes/InternalPage">InternalPage</a> * page, recordkey_t key)<br>Remove a page branch and realign branches.  |
| pagenum_t * | **[get_leftmost_child_idx](/Namespaces/page_helper#function-get_leftmost_child_idx)**(<a href="/Classes/InternalPage">InternalPage</a> * page)<br>Get leftmost child page index.  |

## Detailed Description

<a href="/Classes/Page">Page</a> helper. 

This namespace includes some helper functions which are used by APIs such as tree manager which uses direct page access. These functions do not call any other APIs, but modifying only given data. 


## Functions Documentation

### function get_page_slot

```cpp
PageSlot * get_page_slot(
    LeafPage * page
)
```

Get page slots. 

**Parameters**: 

  * **page** leaf page. 


**Return**: <code>PageSlot&#42;</code>

It just return the reserved area as <code>PageSlot&#42;</code>, which means it does not give any hints for the number of slots. Anyway, you can still get the number of the slots by using <code>page-&gt;page&#95;header.key&#95;num</code>


### function get_record_idx

```cpp
int get_record_idx(
    LeafPage * page,
    recordkey_t key
)
```

Get the record index. 

**Parameters**: 

  * **page** leaf page. 
  * **key** record key 


**Return**: record index if found, <code>-1</code> otherwise. 

Search record key from the leaf page slot and return its index.


### function get_leaf_value

```cpp
void get_leaf_value(
    LeafPage * page,
    int value_idx,
    char * value,
    valsize_t * value_size =nullptr
)
```

Get leaf value. 

**Parameters**: 

  * **page** leaf page. 
  * **value_idx** index number of value. 
  * **value** value will be set into this pointer if not null. 
  * **value_size** value size will be set into this pointer if not null. 


Get a leaf value corresponds to index <code>value&#95;idx</code>, using the page slot information. Then copies the value and its size into given pointer, which is given by caller.


### function get_leaf_value

```cpp
void get_leaf_value(
    LeafPage * page,
    uint16_t value_offset,
    valsize_t value_size,
    char * value
)
```

Get leaf value. 

**Parameters**: 

  * **page** leaf page. 
  * **value_offset** value offset. 
  * **value_size** value size. 
  * **value** value will be set into this pointer if not null. 


Get a leaf value using exact offset and size. Usually it does not called from the outside, and just help the other function. Then copies the value and its size into given pointer, which is given by caller.


### function has_enough_space

```cpp
bool has_enough_space(
    LeafPage * page,
    valsize_t value_size
)
```

Check if given page has enough space for given value size. 

**Parameters**: 

  * **page** leaf page. 
  * **value_size** value size. 


**Return**: <code>true</code> if space is enough, <code>false</code> otherwise. 

Required size for the given <code>value&#95;size</code> is <code>value&#95;size + sizeof(PageSlot)</code>.


### function get_free_space

```cpp
uint64_t * get_free_space(
    LeafPage * page
)
```

Get free space amount. 

**Parameters**: 

  * **page** Leaf page. 


**Return**: The pointer to the free space amount, which will be more useful than raw value in purpose like modifying. 

In leaf page, <code>page-&gt;page&#95;header.reserved&#95;footer.footer&#95;1</code> represents current free space amount.


### function get_sibling_idx

```cpp
pagenum_t * get_sibling_idx(
    LeafPage * page
)
```

Get next sibling index. 

**Return**: right sibling index if page is not rightmost child. <code>0</code> if it is. 

In leaf page, <code>page-&gt;page&#95;header.reserved&#95;footer.footer&#95;2</code> represents the very next(it means right) sibling index.


### function add_leaf_value

```cpp
bool add_leaf_value(
    LeafPage * page,
    recordkey_t key,
    const char * value,
    valsize_t value_size
)
```

Add a leaf value into the last position of the leaf page. 

**Parameters**: 

  * **page** leaf page. 
  * **key** record key. 
  * **value** record value. 
  * **value_size** record value size. 


**Return**: <code>true</code> if the page has enough space and appending is successful, <code>false</code> otherwise. 

### function remove_leaf_value

```cpp
bool remove_leaf_value(
    LeafPage * page,
    recordkey_t key
)
```

Remove a record and compact reserved area in the leaf page. 

**Parameters**: 

  * **page** leaf page. 
  * **key** record key. 


**Return**: <code>true</code> if the key was inside the leaf record and deleted successfully, <code>false</code> otherwise. 

### function set_leaf_value

```cpp
void set_leaf_value(
    LeafPage * page,
    int key_idx,
    char * old_value,
    valsize_t * old_val_size,
    const char * new_value,
    valsize_t new_val_size
)
```

Update the record value in the page and returns old record value size. 

**Parameters**: 

  * **page** record page. 
  * **key_idx** record key index. 
  * **old_value** old record value. 
  * **old_val_size** old record value size. 
  * **new_value** new record value. 
  * **new_val_size** new record value size. 


Todoupdate at here 

Todoupdate at here 


### function add_internal_key

```cpp
bool add_internal_key(
    InternalPage * page,
    recordkey_t key,
    pagenum_t page_idx
)
```

Add a page branch into the last position of the internal page. 

**Parameters**: 

  * **page** internal page. 
  * **key** branch key. 
  * **page_idx** branch page index. 


**Return**: <code>true</code> if the page has enough space and appending is successful, <code>false</code> otherwise. 

### function remove_internal_key

```cpp
bool remove_internal_key(
    InternalPage * page,
    recordkey_t key
)
```

Remove a page branch and realign branches. 

**Parameters**: 

  * **page** internal page. 
  * **key** branch key. 


**Return**: <code>true</code> if the key was inside the internal branch and deleted successfully, <code>false</code> otherwise. 

### function get_leftmost_child_idx

```cpp
pagenum_t * get_leftmost_child_idx(
    InternalPage * page
)
```

Get leftmost child page index. 

**Parameters**: 

  * **page** internal page. 


**Return**: leftmost child page index. 

In internal page, <code>page-&gt;page&#95;header.reserved&#95;footer.footer&#95;2</code> represents current free space amount.






-------------------------------

Updated on 2021-12-05 at 22:45:19 +0900