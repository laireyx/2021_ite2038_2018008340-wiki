

# test/buffer_test.cc



## Classes

|                | Name           |
| -------------- | -------------- |
| class | **[BasicBufferManagerTest](/Classes/BasicBufferManagerTest)**  |
| class | **[CrossTableTest](/Classes/CrossTableTest)**  |

## Functions

|                | Name           |
| -------------- | -------------- |
| | **[TEST_F](/Modules/TestCode#function-test_f)**(<a href="/Classes/BasicBufferManagerTest">BasicBufferManagerTest</a> , HandlesPageAllocation )<br>Tests page allocation and free.  |
| | **[TEST_F](/Modules/TestCode#function-test_f)**(<a href="/Classes/BasicBufferManagerTest">BasicBufferManagerTest</a> , CheckReadWriteOperation )<br>Tests page read/write operations.  |
| | **[TEST_F](/Modules/TestCode#function-test_f)**(<a href="/Classes/BasicBufferManagerTest">BasicBufferManagerTest</a> , UniqueIdTest )<br>Tests unique table fd.  |
| | **[TEST_F](/Modules/TestCode#function-test_f)**(<a href="/Classes/BasicBufferManagerTest">BasicBufferManagerTest</a> , SequentialAllocateTest )<br>Tests sequential allocation.  |
| | **[TEST_F](/Modules/TestCode#function-test_f)**(<a href="/Classes/BasicBufferManagerTest">BasicBufferManagerTest</a> , RandomAllocateTest )<br>Tests random allocation.  |
| | **[TEST_F](/Modules/TestCode#function-test_f)**(<a href="/Classes/CrossTableTest">CrossTableTest</a> , RandomAllocateTest )<br>Test for cross table buffer management.  |

## Attributes

|                | Name           |
| -------------- | -------------- |
| constexpr int | **[test_count](/Modules/TestCode#variable-test_count)**  |
| const char * | **[TABLE_PATH](/Modules/TestCode#variable-table_path)**  |
| const char * | **[TABLE_PATH_ALIAS](/Modules/TestCode#variable-table_path_alias)**  |
| const char * | **[ANOTHER_TABLE_PATH](/Modules/TestCode#variable-another_table_path)**  |


## Functions Documentation

### function TEST_F

```cpp
TEST_F(
    BasicBufferManagerTest ,
    HandlesPageAllocation 
)
```

Tests page allocation and free. 



1. Allocate 2 pages and free one of them, traverse the free page list and check the existence/absence of the freed/allocated page 


### function TEST_F

```cpp
TEST_F(
    BasicBufferManagerTest ,
    CheckReadWriteOperation 
)
```

Tests page read/write operations. 



1. Write/Read a page with some random content and check if the data matches 


### function TEST_F

```cpp
TEST_F(
    BasicBufferManagerTest ,
    UniqueIdTest 
)
```

Tests unique table fd. 

Create table files with different path, but same realpath to check if table uses unique id for that file. Also checks real different file and assure that two different table id should not be same. 


### function TEST_F

```cpp
TEST_F(
    BasicBufferManagerTest ,
    SequentialAllocateTest 
)
```

Tests sequential allocation. 

Check if pages are correctly allocated and freed. it does not include any expectation, so test procedure purely depends on filemanager's own error checking method. 


### function TEST_F

```cpp
TEST_F(
    BasicBufferManagerTest ,
    RandomAllocateTest 
)
```

Tests random allocation. 

This disk space manager uses LIFO for free page management. Allocate and free page randomly first then allocate again and check if second allocation result is equal to first free result in reverse order. 


### function TEST_F

```cpp
TEST_F(
    CrossTableTest ,
    RandomAllocateTest 
)
```

Test for cross table buffer management. 


## Attributes Documentation

### variable test_count

```cpp
constexpr int test_count = 100;
```


### variable TABLE_PATH

```cpp
const char * TABLE_PATH = "test.db";
```


### variable TABLE_PATH_ALIAS

```cpp
const char * TABLE_PATH_ALIAS = "./test.db";
```


### variable ANOTHER_TABLE_PATH

```cpp
const char * ANOTHER_TABLE_PATH = "test_another.db";
```



## Source code

```cpp

#include <buffer.h>
#include <db.h>
#include <gtest/gtest.h>
#include <sys/stat.h>
#include <sys/types.h>
#include <unistd.h>

#include <cstdlib>
#include <ctime>

constexpr int test_count = 100;

const char* TABLE_PATH = "test.db";
const char* TABLE_PATH_ALIAS = "./test.db";
const char* ANOTHER_TABLE_PATH = "test_another.db";

class BasicBufferManagerTest : public ::testing::Test {
   protected:
    int test_order[test_count];
    tableid_t table_id = 0;

    BasicBufferManagerTest() {
        init_db();
        table_id = buffered_open_table_file(TABLE_PATH);
        srand(1);

        // Generate random indexes
        for (int i = 0; i < test_count; i++) {
            test_order[i] = i + 1;
        }

        for (int i = 0; i < test_count; i++) {
            int x, y, temp;
            x = rand() % test_count;
            y = rand() % test_count;

            temp = test_order[x];
            test_order[x] = test_order[y];
            test_order[y] = temp;
        }
    }
    ~BasicBufferManagerTest() { shutdown_db(); }
};

TEST_F(BasicBufferManagerTest, HandlesPageAllocation) {
    pagenum_t allocated_page, freed_page;

    // Allocate the pages
    allocated_page = buffered_alloc_page(table_id);
    freed_page = buffered_alloc_page(table_id);

    // Free one page
    buffered_free_page(table_id, freed_page);

    // Traverse the free page list and check the existence of the
    // freed/allocated pages. You might need to open a few APIs soley for
    // testing.
    headerpage_t header_page;
    buffered_read_page(table_id, 0, reinterpret_cast<page_t*>(&header_page));

    // Flag which means if freed page is in free page list correctly.
    bool is_freed_correctly = false;
    int current_page_idx = header_page.free_page_idx;
    freepage_t free_page;

    while (current_page_idx != 0) {
        is_freed_correctly =
            is_freed_correctly || (current_page_idx == freed_page);

        // Expect that an allocated page should not be in free page list.
        EXPECT_NE(current_page_idx, allocated_page);

        buffered_read_page(table_id, current_page_idx,
                           reinterpret_cast<page_t*>(&free_page));
        current_page_idx = free_page.next_free_idx;
    }
    EXPECT_TRUE(is_freed_correctly);

    buffered_free_page(table_id, allocated_page);
}

TEST_F(BasicBufferManagerTest, CheckReadWriteOperation) {
    int free_page_num = buffered_alloc_page(table_id);

    ASSERT_EQ(sizeof(internalpage_t), 4096);

    internalpage_t page;
    uint8_t* page_data = reinterpret_cast<uint8_t*>(page.page_branches);

    // Generate random values for write
    uint8_t random_values[PAGE_SIZE - PAGE_HEADER_SIZE] = {};
    for (int i = 0; i < PAGE_SIZE - PAGE_HEADER_SIZE; i++) {
        random_values[i] = rand() % 256;
        // Set that data into allocated page area
        page_data[i] = random_values[i];
    }

    // Write page into file
    buffered_write_page(table_id, free_page_num, &page);
    // Read page from file
    buffered_read_page(table_id, free_page_num, &page);

    for (int i = 0; i < PAGE_SIZE - PAGE_HEADER_SIZE; i++) {
        // Validation
        EXPECT_EQ(random_values[i], page_data[i]);
    }

    buffered_free_page(table_id, free_page_num);
}

TEST_F(BasicBufferManagerTest, UniqueIdTest) {
    EXPECT_EQ(table_id, buffered_open_table_file(TABLE_PATH));
    EXPECT_EQ(table_id, buffered_open_table_file(TABLE_PATH_ALIAS));

    EXPECT_NE(table_id, buffered_open_table_file(ANOTHER_TABLE_PATH));
}

TEST_F(BasicBufferManagerTest, SequentialAllocateTest) {
    int allocation_result[test_count] = {
        0,
    };

    for (int i = 0; i < test_count; i++) {
        allocation_result[i] = buffered_alloc_page(table_id);
    }

    for (int i = 0; i < test_count; i++) {
        buffered_free_page(table_id, allocation_result[i]);
    }
}

TEST_F(BasicBufferManagerTest, RandomAllocateTest) {
    for (int i = 0; i < test_count; i++) {
        buffered_alloc_page(table_id);
    }

    // Randomly free page
    for (int i = 0; i < test_count; i++) {
        buffered_free_page(table_id, test_order[i]);
    }

    // Check if the allocated page id is equal to freed page in reverse order.
    for (int i = 0; i < test_count; i++) {
        EXPECT_EQ(test_order[test_count - 1 - i],
                  buffered_alloc_page(table_id));
    }

    for (int i = 0; i < test_count; i++) {
        buffered_free_page(table_id, test_order[i]);
    }
}

class CrossTableTest : public ::testing::Test {
   protected:
    int test_order[test_count];
    tableid_t table_id[2] = {-1, -1};

    CrossTableTest() {
        init_db();
        table_id[0] = buffered_open_table_file(TABLE_PATH);
        table_id[1] = buffered_open_table_file(ANOTHER_TABLE_PATH);
        srand(time(NULL));

        // Generate random indexes
        for (int i = 0; i < test_count; i++) {
            test_order[i] = i + 1;
        }

        for (int i = 0; i < test_count; i++) {
            int x, y, temp;
            x = rand() % test_count;
            y = rand() % test_count;

            temp = test_order[x];
            test_order[x] = test_order[y];
            test_order[y] = temp;
        }
    }
    ~CrossTableTest() { shutdown_db(); }
};

TEST_F(CrossTableTest, RandomAllocateTest) {
    for (int i = 0; i < test_count; i++) {
        buffered_alloc_page(table_id[0]);
        buffered_alloc_page(table_id[1]);
    }

    // Randomly free page
    for (int i = 0; i < test_count; i++) {
        buffered_free_page(table_id[0], test_order[i]);
        buffered_free_page(table_id[1], test_order[test_count - 1 - i]);
    }

    // Check if the allocated page id is equal to freed page in reverse order.
    for (int i = 0; i < test_count; i++) {
        EXPECT_EQ(test_order[test_count - 1 - i],
                  buffered_alloc_page(table_id[0]));
        EXPECT_EQ(test_order[i], buffered_alloc_page(table_id[1]));
    }

    for (int i = 0; i < test_count; i++) {
        buffered_free_page(table_id[0], test_order[i]);
        buffered_free_page(table_id[1], test_order[i]);
    }
}
```


-------------------------------

Updated on 2021-10-25 at 17:08:33 +0900
