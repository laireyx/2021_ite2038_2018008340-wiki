

# TransactionLog

**Module:** **[TransactionManager](/Modules/TransactionManager)**



Transaction log. 


`#include <transaction.h>`

## Public Attributes

|                | Name           |
| -------------- | -------------- |
| <a href="/Modules/TransactionManager#enum-logtype">LogType</a> | **[type](/Classes/TransactionLog#variable-type)** <br>Log type.  |
| tableid_t | **[table_id](/Classes/TransactionLog#variable-table_id)** <br>Updated table id.  |
| recordkey_t | **[key](/Classes/TransactionLog#variable-key)** <br>Updated record key.  |
| char * | **[old_value](/Classes/TransactionLog#variable-old_value)** <br>Old record value.  |
| valsize_t | **[old_val_size](/Classes/TransactionLog#variable-old_val_size)** <br>Old record value size.  |
| trxlogid_t | **[prev_trx_log](/Classes/TransactionLog#variable-prev_trx_log)** <br>Previous transaction log index.  |

## Public Attributes Documentation

### variable type

```cpp
LogType type;
```

Log type. 

### variable table_id

```cpp
tableid_t table_id;
```

Updated table id. 

### variable key

```cpp
recordkey_t key;
```

Updated record key. 

### variable old_value

```cpp
char * old_value;
```

Old record value. 

### variable old_val_size

```cpp
valsize_t old_val_size;
```

Old record value size. 

### variable prev_trx_log

```cpp
trxlogid_t prev_trx_log;
```

Previous transaction log index. 

-------------------------------

Updated on 2021-12-05 at 18:37:58 +0900