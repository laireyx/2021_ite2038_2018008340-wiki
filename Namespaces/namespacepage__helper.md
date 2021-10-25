---
title: page_helper
summary: Page helper. 

---

# page_helper

**Module:** **[DiskSpaceManager](/Modules/group__DiskSpaceManager)**

Page helper.  [More...](#detailed-description)

## Functions

|                | Name           |
| -------------- | -------------- |
| <a href="/Classes/structPageSlot">PageSlot</a> * | **[get_page_slot](/Namespaces/namespacepage__helper#function-get-page-slot)**(<a href="/Classes/structLeafPage">LeafPage</a> * page)<br>Get page slots.  |
| void | **[get_leaf_value](/Namespaces/namespacepage__helper#function-get-leaf-value)**(<a href="/Classes/structLeafPage">LeafPage</a> * page, int value_idx, char * value, uint16_t * value_size =nullptr)<br>Get leaf value.  |
| void | **[get_leaf_value](/Namespaces/namespacepage__helper#function-get-leaf-value)**(<a href="/Classes/structLeafPage">LeafPage</a> * page, uint16_t value_offset, uint16_t value_size, char * value)<br>Get leaf value.  |
| bool | **[has_enough_space](/Namespaces/namespacepage__helper#function-has-enough-space)**(<a href="/Classes/structLeafPage">LeafPage</a> * page, uint16_t value_size)<br>Check if given page has enough space for given value size.  |
| uint64_t * | **[get_free_space](/Namespaces/namespacepage__helper#function-get-free-space)**(<a href="/Classes/structLeafPage">LeafPage</a> * page)<br>Get free space amount.  |
| pagenum_t * | **[get_sibling_idx](/Namespaces/namespacepage__helper#function-get-sibling-idx)**(<a href="/Classes/structLeafPage">LeafPage</a> * page)<br>Get next sibling index.  |
| bool | **[add_leaf_value](/Namespaces/namespacepage__helper#function-add-leaf-value)**(<a href="/Classes/structLeafPage">LeafPage</a> * page, int64_t key, const char * value, uint16_t value_size)<br>Add a leaf value into the last position of the leaf page.  |
| bool | **[remove_leaf_value](/Namespaces/namespacepage__helper#function-remove-leaf-value)**(<a href="/Classes/structLeafPage">LeafPage</a> * page, int64_t key)<br>Remove a record and compact reserved area in the leaf page.  |
| bool | **[add_internal_key](/Namespaces/namespacepage__helper#function-add-internal-key)**(<a href="/Classes/structInternalPage">InternalPage</a> * page, int64_t key, pagenum_t page_idx)<br>Add a page branch into the last position of the internal page.  |
| bool | **[remove_internal_key](/Namespaces/namespacepage__helper#function-remove-internal-key)**(<a href="/Classes/structInternalPage">InternalPage</a> * page, int64_t key)<br>Remove a page branch and realign branches.  |
| pagenum_t * | **[get_leftmost_child_idx](/Namespaces/namespacepage__helper#function-get-leftmost-child-idx)**(<a href="/Classes/structInternalPage">InternalPage</a> * page)<br>Get leftmost child page index.  |

## Detailed Description

Page helper. 

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


### function get_leaf_value

```cpp
void get_leaf_value(
    LeafPage * page,
    int value_idx,
    char * value,
    uint16_t * value_size =nullptr
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
    uint16_t value_size,
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
    uint16_t value_size
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
    int64_t key,
    const char * value,
    uint16_t value_size
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
    int64_t key
)
```

Remove a record and compact reserved area in the leaf page. 

**Parameters**: 

  * **page** leaf page. 
  * **key** record key. 


**Return**: <code>true</code> if the key was inside the leaf record and deleted successfully, <code>false</code> otherwise. 

### function add_internal_key

```cpp
bool add_internal_key(
    InternalPage * page,
    int64_t key,
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
    int64_t key
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

Updated on 2021-10-25 at 16:59:00 +0900