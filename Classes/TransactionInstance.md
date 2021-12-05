

# TransactionInstance

**Module:** **[TransactionManager](/Modules/TransactionManager)**



Transaction instance. 


`#include <transaction.h>`

## Public Attributes

|                | Name           |
| -------------- | -------------- |
| <a href="/Modules/TransactionManager#enum-transactionstate">TransactionState</a> | **[state](/Classes/TransactionInstance#variable-state)** <br>Transaction running state.  |
| trxlogid_t | **[log_tail](/Classes/TransactionInstance#variable-log_tail)** <br>The last log of this transaction.  |
| <a href="/Classes/Lock">Lock</a> * | **[lock_head](/Classes/TransactionInstance#variable-lock_head)** <br>Transaction lock head.  |
| <a href="/Classes/Lock">Lock</a> * | **[lock_tail](/Classes/TransactionInstance#variable-lock_tail)** <br>Transaction lock tail.  |

## Public Attributes Documentation

### variable state

```cpp
TransactionState state;
```

Transaction running state. 

### variable log_tail

```cpp
trxlogid_t log_tail;
```

The last log of this transaction. 

### variable lock_head

```cpp
Lock * lock_head;
```

Transaction lock head. 

### variable lock_tail

```cpp
Lock * lock_tail;
```

Transaction lock tail. 

-------------------------------

Updated on 2021-12-05 at 18:37:58 +0900