

# db/include/page.h



## Namespaces

| Name           |
| -------------- |
| **[page_helper](/Namespaces/page_helper)** <br><a href="/Classes/Page">Page</a> helper.  |

## Classes

|                | Name           |
| -------------- | -------------- |
| class | **[Page](/Classes/Page)** <br>struct for abstract page.  |
| class | **[FullPage](/Classes/FullPage)** <br>struct for any page.  |
| class | **[HeaderPage](/Classes/HeaderPage)** <br>struct for the header page.  |
| class | **[FreePage](/Classes/FreePage)** <br>struct for the free page.  |
| class | **[PageHeader](/Classes/PageHeader)** <br>page header for allocated(internal and leaf) node.  |
| struct | **[PageHeader::ReservedFooter](/Classes/PageHeader_1_1ReservedFooter)**  |
| class | **[PageSlot](/Classes/PageSlot)** <br>page slot for leaf node.  |
| class | **[PageBranch](/Classes/PageBranch)** <br>page slot for internal node.  |
| class | **[AllocatedPage](/Classes/AllocatedPage)** <br>struct for allocated page.  |
| class | **[AllocatedFullPage](/Classes/AllocatedFullPage)** <br>struct for any allocated page.  |
| class | **[InternalPage](/Classes/InternalPage)** <br>struct for allocated internal page.  |
| class | **[LeafPage](/Classes/LeafPage)** <br>struct for allocated leaf page.  |

## Types

|                | Name           |
| -------------- | -------------- |
| typedef <a href="/Classes/Page">Page</a> | **[page_t](/Modules/DiskSpaceManager#typedef-page_t)**  |
| typedef <a href="/Classes/FullPage">FullPage</a> | **[fullpage_t](/Modules/DiskSpaceManager#typedef-fullpage_t)**  |
| typedef <a href="/Classes/PageHeader">PageHeader</a> | **[pageheader_t](/Modules/DiskSpaceManager#typedef-pageheader_t)**  |
| typedef <a href="/Classes/HeaderPage">HeaderPage</a> | **[headerpage_t](/Modules/DiskSpaceManager#typedef-headerpage_t)**  |
| typedef <a href="/Classes/FreePage">FreePage</a> | **[freepage_t](/Modules/DiskSpaceManager#typedef-freepage_t)**  |
| typedef <a href="/Classes/AllocatedFullPage">AllocatedFullPage</a> | **[allocatedpage_t](/Modules/DiskSpaceManager#typedef-allocatedpage_t)**  |
| typedef <a href="/Classes/InternalPage">InternalPage</a> | **[internalpage_t](/Modules/DiskSpaceManager#typedef-internalpage_t)**  |
| typedef <a href="/Classes/LeafPage">LeafPage</a> | **[leafpage_t](/Modules/DiskSpaceManager#typedef-leafpage_t)**  |

## Functions

|                | Name           |
| -------------- | -------------- |
| struct <a href="/Classes/PageSlot">PageSlot</a> | **[__attribute__](/Modules/DiskSpaceManager#function-__attribute__)**((packed) ) |

## Attributes

|                | Name           |
| -------------- | -------------- |
| recordkey_t | **[key](/Files/db/include/page.h#variable-key)** <br>The page key.  |
| valsize_t | **[value_size](/Files/db/include/page.h#variable-value_size)** <br>The value size(in bytes).  |
| uint16_t | **[value_offset](/Files/db/include/page.h#variable-value_offset)** <br>The value offset(in bytes).  |
| trxid_t | **[trx_id](/Files/db/include/page.h#variable-trx_id)** <br>Transaction id which is implicitly locked this record.  |
| struct <a href="/Classes/PageBranch">PageBranch</a> | **[__attribute__](/Modules/DiskSpaceManager#variable-__attribute__)**  |
| uint8_t | **[reserved](/Files/db/include/page.h#variable-reserved)** <br>Reserved area for normal allocated page.  |

## Types Documentation

### typedef page_t

```cpp
typedef Page page_t;
```


### typedef fullpage_t

```cpp
typedef FullPage fullpage_t;
```


### typedef pageheader_t

```cpp
typedef PageHeader pageheader_t;
```


### typedef headerpage_t

```cpp
typedef HeaderPage headerpage_t;
```


### typedef freepage_t

```cpp
typedef FreePage freepage_t;
```


### typedef allocatedpage_t

```cpp
typedef AllocatedFullPage allocatedpage_t;
```


### typedef internalpage_t

```cpp
typedef InternalPage internalpage_t;
```


### typedef leafpage_t

```cpp
typedef LeafPage leafpage_t;
```



## Functions Documentation

### function __attribute__

```cpp
struct PageSlot __attribute__(
    (packed) 
)
```



## Attributes Documentation

### variable key

```cpp
recordkey_t key;
```

The page key. 

### variable value_size

```cpp
valsize_t value_size;
```

The value size(in bytes). 

### variable value_offset

```cpp
uint16_t value_offset;
```

The value offset(in bytes). 

### variable trx_id

```cpp
trxid_t trx_id;
```

Transaction id which is implicitly locked this record. 

### variable __attribute__

```cpp
struct PageBranch __attribute__;
```


### variable reserved

```cpp
uint8_t reserved;
```

Reserved area for normal allocated page. 


## Source code

```cpp

#pragma once
#include <const.h>
#include <types.h>

#include <cstdint>

struct Page {};

struct FullPage : public Page {
    uint8_t reserved[PAGE_SIZE];
};

struct HeaderPage : public Page {
    pagenum_t free_page_idx;
    uint64_t page_num;
    pagenum_t root_page_idx;

    uint8_t reserved[PAGE_SIZE - 24];
};

struct FreePage : public Page {
    pagenum_t next_free_idx;

    uint8_t reserved[PAGE_SIZE - 8];
};

struct PageHeader {
    pagenum_t parent_page_idx;
    uint32_t is_leaf_page;
    uint32_t key_num;

    uint8_t reserved[PAGE_HEADER_SIZE - 16 - 16];

    struct ReservedFooter {
        uint64_t footer_1;
        uint64_t footer_2;
    } reserved_footer;
};

struct PageSlot {
    recordkey_t key;
    valsize_t value_size;
    uint16_t value_offset;
    trxid_t trx_id;
} __attribute__((packed));

struct PageBranch {
    recordkey_t key;
    uint16_t page_idx;
};

struct AllocatedPage : public Page {
    PageHeader page_header;
};

struct AllocatedFullPage : public AllocatedPage {
    uint8_t reserved[PAGE_SIZE - PAGE_HEADER_SIZE];
};

struct InternalPage : public AllocatedPage {
    PageBranch page_branches[MAX_PAGE_BRANCHES];
};

struct LeafPage : public AllocatedPage {
    uint8_t reserved[PAGE_SIZE - PAGE_HEADER_SIZE];
} __attribute__((packed));

namespace page_helper {

PageSlot* get_page_slot(LeafPage* page);
int get_record_idx(LeafPage* page, recordkey_t key);
void get_leaf_value(LeafPage* page, int value_idx, char* value,
                    valsize_t* value_size = nullptr);
void get_leaf_value(LeafPage* page, uint16_t value_offset, valsize_t value_size,
                    char* value);
bool has_enough_space(LeafPage* page, valsize_t value_size);
uint64_t* get_free_space(LeafPage* page);
pagenum_t* get_sibling_idx(LeafPage* page);

bool add_leaf_value(LeafPage* page, recordkey_t key, const char* value,
                    valsize_t value_size);
bool remove_leaf_value(LeafPage* page, recordkey_t key);

void set_leaf_value(LeafPage* page, int key_idx, char* old_value, valsize_t* old_val_size,
                    const char* new_value, valsize_t new_val_size);

bool add_internal_key(InternalPage* page, recordkey_t key, pagenum_t page_idx);
bool remove_internal_key(InternalPage* page, recordkey_t key);
pagenum_t* get_leftmost_child_idx(InternalPage* page);
}  // namespace page_helper

typedef Page page_t;
typedef FullPage fullpage_t;
typedef PageHeader pageheader_t;
typedef HeaderPage headerpage_t;
typedef FreePage freepage_t;
typedef AllocatedFullPage allocatedpage_t;
typedef InternalPage internalpage_t;
typedef LeafPage leafpage_t;
```


-------------------------------

Updated on 2021-12-05 at 22:45:19 +0900
