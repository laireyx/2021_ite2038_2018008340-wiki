---
title: db/include/page.h

---

# db/include/page.h



## Namespaces

| Name           |
| -------------- |
| **[page_helper](/Namespaces/namespacepage__helper)** <br><a href="/Classes/structPage">Page</a> helper.  |

## Classes

|                | Name           |
| -------------- | -------------- |
| class | **[Page](/Classes/structPage)** <br>struct for abstract page.  |
| class | **[HeaderPage](/Classes/structHeaderPage)** <br>struct for the header page.  |
| class | **[FreePage](/Classes/structFreePage)** <br>struct for the free page.  |
| class | **[PageHeader](/Classes/structPageHeader)** <br>page header for allocated(internal and leaf) node.  |
| struct | **[PageHeader::ReservedFooter](/Classes/structPageHeader_1_1ReservedFooter)**  |
| class | **[PageSlot](/Classes/structPageSlot)** <br>page slot for allocated(internal and leaf) node.  |
| class | **[PageBranch](/Classes/structPageBranch)** <br>page slot for internal node.  |
| class | **[AllocatedPage](/Classes/structAllocatedPage)** <br>struct for allocated page.  |
| class | **[AllocatedFullPage](/Classes/structAllocatedFullPage)** <br>struct for any allocated page.  |
| class | **[InternalPage](/Classes/structInternalPage)** <br>struct for allocated internal page.  |
| class | **[LeafPage](/Classes/structLeafPage)** <br>struct for allocated leaf page.  |

## Types

|                | Name           |
| -------------- | -------------- |
| typedef <a href="/Classes/structPage">Page</a> | **[page_t](/Modules/group__DiskSpaceManager#typedef-page-t)**  |
| typedef <a href="/Classes/structPageHeader">PageHeader</a> | **[pageheader_t](/Modules/group__DiskSpaceManager#typedef-pageheader-t)**  |
| typedef <a href="/Classes/structHeaderPage">HeaderPage</a> | **[headerpage_t](/Modules/group__DiskSpaceManager#typedef-headerpage-t)**  |
| typedef <a href="/Classes/structFreePage">FreePage</a> | **[freepage_t](/Modules/group__DiskSpaceManager#typedef-freepage-t)**  |
| typedef <a href="/Classes/structAllocatedFullPage">AllocatedFullPage</a> | **[allocatedpage_t](/Modules/group__DiskSpaceManager#typedef-allocatedpage-t)**  |
| typedef <a href="/Classes/structInternalPage">InternalPage</a> | **[internalpage_t](/Modules/group__DiskSpaceManager#typedef-internalpage-t)**  |
| typedef <a href="/Classes/structLeafPage">LeafPage</a> | **[leafpage_t](/Modules/group__DiskSpaceManager#typedef-leafpage-t)**  |

## Functions

|                | Name           |
| -------------- | -------------- |
| struct <a href="/Classes/structPageSlot">PageSlot</a> | **[__attribute__](/Modules/group__DiskSpaceManager#function---attribute--)**((packed) ) |

## Attributes

|                | Name           |
| -------------- | -------------- |
| constexpr int | **[PAGE_SIZE](/Modules/group__DiskSpaceManager#variable-page-size)** <br>Size of each page(in bytes).  |
| constexpr int | **[PAGE_HEADER_SIZE](/Modules/group__DiskSpaceManager#variable-page-header-size)** <br>Size of page header(in bytes).  |
| constexpr int | **[MAX_PAGE_BRANCHES](/Modules/group__DiskSpaceManager#variable-max-page-branches)** <br>Maximum number of page branches.  |
| int64_t | **[key](/Files/page_8h#variable-key)** <br>The page key.  |
| uint16_t | **[value_size](/Files/page_8h#variable-value-size)** <br>The value size(in bytes).  |
| uint16_t | **[value_offset](/Files/page_8h#variable-value-offset)** <br>The value offset(in bytes).  |
| struct <a href="/Classes/structPageBranch">PageBranch</a> | **[__attribute__](/Modules/group__DiskSpaceManager#variable---attribute--)**  |
| uint8_t | **[reserved](/Files/page_8h#variable-reserved)** <br>Reserved area for normal allocated page.  |

## Types Documentation

### typedef page_t

```cpp
typedef Page page_t;
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

### variable PAGE_SIZE

```cpp
constexpr int PAGE_SIZE = 4096;
```

Size of each page(in bytes). 

### variable PAGE_HEADER_SIZE

```cpp
constexpr int PAGE_HEADER_SIZE = 128;
```

Size of page header(in bytes). 

### variable MAX_PAGE_BRANCHES

```cpp
constexpr int MAX_PAGE_BRANCHES = 248;
```

Maximum number of page branches. 

### variable key

```cpp
int64_t key;
```

The page key. 

### variable value_size

```cpp
uint16_t value_size;
```

The value size(in bytes). 

### variable value_offset

```cpp
uint16_t value_offset;
```

The value offset(in bytes). 

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
#include "types.h"

#include <cstdint>

constexpr int PAGE_SIZE = 4096;

constexpr int PAGE_HEADER_SIZE = 128;

constexpr int MAX_PAGE_BRANCHES = 248;

struct Page {};

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
    int64_t key;
    uint16_t value_size;
    uint16_t value_offset;
} __attribute__((packed));

struct PageBranch {
    int64_t key;
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
void get_leaf_value(LeafPage* page, int value_idx, char* value,
                    uint16_t* value_size = nullptr);
void get_leaf_value(LeafPage* page, uint16_t value_offset, uint16_t value_size,
                    char* value);
bool has_enough_space(LeafPage* page, uint16_t value_size);
uint64_t* get_free_space(LeafPage* page);
pagenum_t* get_sibling_idx(LeafPage* page);

bool add_leaf_value(LeafPage* page, int64_t key, const char* value,
                    uint16_t value_size);
bool remove_leaf_value(LeafPage* page, int64_t key);

bool add_internal_key(InternalPage* page, int64_t key, pagenum_t page_idx);
bool remove_internal_key(InternalPage* page, int64_t key);
pagenum_t* get_leftmost_child_idx(InternalPage* page);
}

typedef Page page_t;
typedef PageHeader pageheader_t;
typedef HeaderPage headerpage_t;
typedef FreePage freepage_t;
typedef AllocatedFullPage allocatedpage_t;
typedef InternalPage internalpage_t;
typedef LeafPage leafpage_t;
```


-------------------------------

Updated on 2021-10-16 at 22:36:54 +0900
