

# test/trx_test.cc



## Classes

|                | Name           |
| -------------- | -------------- |
| class | **[BasicTableTest](/Classes/BasicTableTest)**  |

## Functions

|                | Name           |
| -------------- | -------------- |
| | **[TEST_F](/Modules/TestCode#function-test_f)**(<a href="/Classes/BasicTableTest">BasicTableTest</a> , TransactionTest )<br>Tests database insertion API.  |

## Attributes

|                | Name           |
| -------------- | -------------- |
| constexpr int | **[test_count](/Modules/TestCode#variable-test_count)**  |
| constexpr int | **[trx_count](/Modules/TestCode#variable-trx_count)**  |
| tableid_t | **[table_id](/Modules/TestCode#variable-table_id)**  |
| uint8_t | **[temp_size](/Modules/TestCode#variable-temp_size)**  |
| uint8_t | **[temp_value](/Modules/TestCode#variable-temp_value)**  |


## Functions Documentation

### function TEST_F

```cpp
TEST_F(
    BasicTableTest ,
    TransactionTest 
)
```

Tests database insertion API. 



1. Open a database and write random values in random order.
    * Find the value using the key and compare it to the value. 



## Attributes Documentation

### variable test_count

```cpp
constexpr int test_count = 20000;
```


### variable trx_count

```cpp
constexpr int trx_count = 4;
```


### variable table_id

```cpp
tableid_t table_id;
```


### variable temp_size

```cpp
uint8_t temp_size = {};
```


### variable temp_value

```cpp
static uint8_t temp_value = {};
```



## Source code

```cpp

#include <db.h>
#include <transaction.h>
#include <gtest/gtest.h>
#include <sys/stat.h>
#include <sys/types.h>
#include <pthread.h>

#include <cstdlib>
#include <cstring>
#include <ctime>

constexpr int test_count = 20000;
constexpr int trx_count = 4;

tableid_t table_id;
uint8_t temp_size[test_count] = {};
static uint8_t temp_value[test_count][128] = {};

class BasicTableTest : public ::testing::Test {
   protected:
    int test_order[test_count];

    BasicTableTest() {
        init_db();
        srand(1);

        // Generate random indexes
        for (int i = 0; i < test_count; i++) {
            test_order[i] = i;
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
    ~BasicTableTest() { shutdown_db(); }
};

TEST_F(BasicTableTest, TransactionTest) {
    table_id = open_table("test_trx.db");
    // Check if the file is opened
    ASSERT_TRUE(table_id >=
                0);  // change the condition to your design's behavior

    for (int i = 0; i < test_count; i++) {
        valsize_t value_size;

        temp_size[test_order[i]] = 50 + rand() % 63;
        for (int j = 0; j < 128; j++) {
            temp_value[test_order[i]][j] = rand() % 256;
        }

        ASSERT_EQ(db_insert(table_id, test_order[i],
                            reinterpret_cast<char*>(temp_value[test_order[i]]),
                            temp_size[test_order[i]]),
                  0);
    }

    trxid_t trx_ids[trx_count];
    for(int i = 0; i < trx_count; i++) {
        trx_ids[i] = trx_begin();
        ASSERT_TRUE(trx_ids[i] > 0);
    }

    valsize_t return_size;
    uint8_t return_value[128] = {};
    for(int i = 0; i < trx_count; i++) {
        EXPECT_EQ(db_update(table_id, i, reinterpret_cast<char*>(temp_value[i + 1]), temp_size[i + 1], &return_size, trx_ids[i]), 0);
        EXPECT_EQ(db_find(table_id, i, reinterpret_cast<char*>(return_value), &return_size, trx_ids[i]), 0);
    }

    for(int i = 0; i < trx_count; i++) {
        EXPECT_EQ(db_update(table_id, i, reinterpret_cast<char*>(temp_value[i]), temp_size[i], &return_size, trx_ids[i]), 0);
    }

    for (int i = 0; i < test_count; i++) {
        valsize_t value_size;
        uint8_t return_value[128] = {};

        ASSERT_FALSE(db_find(table_id, test_order[i],
                             reinterpret_cast<char*>(return_value),
                             &value_size) < 0);
        ASSERT_EQ(value_size, temp_size[test_order[i]]);
        ASSERT_EQ(memcmp(temp_value[test_order[i]], return_value,
                         temp_size[test_order[i]]),
                  0);
    }
}
```


-------------------------------

Updated on 2021-12-05 at 18:37:58 +0900
