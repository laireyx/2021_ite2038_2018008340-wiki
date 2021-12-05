

# Lock

**Module:** **[LockManager](/Modules/LockManager)**






`#include <lock.h>`

## Public Attributes

|                | Name           |
| -------------- | -------------- |
| <a href="/Modules/LockManager#enum-lockmode">LockMode</a> | **[lock_mode](/Classes/Lock#variable-lock_mode)** <br><a href="/Classes/Lock">Lock</a> mode.  |
| bool | **[acquired](/Classes/Lock#variable-acquired)** <br><code>true</code> if acquired, <code>false</code> if sleeping.  |
| pthread_cond_t * | **[cond](/Classes/Lock#variable-cond)** <br>Conditional variable for this lock.  |
| <a href="/Classes/LockList">LockList</a> * | **[list](/Classes/Lock#variable-list)** <br><a href="/Classes/Lock">Lock</a> list.  |
| trxid_t | **[trx_id](/Classes/Lock#variable-trx_id)** <br>Transaction id.  |
| lockmask_t | **[mask](/Classes/Lock#variable-mask)** <br><a href="/Classes/Lock">Lock</a> mask for shared lock compression.  |
| <a href="/Classes/Lock">Lock</a> * | **[prev](/Classes/Lock#variable-prev)** <br>Previous waiting lock.  |
| <a href="/Classes/Lock">Lock</a> * | **[next](/Classes/Lock#variable-next)** <br>Next waiting lock.  |
| <a href="/Classes/Lock">Lock</a> * | **[next_trx](/Classes/Lock#variable-next_trx)** <br>Next transaction lock.  |

## Public Attributes Documentation

### variable lock_mode

```cpp
LockMode lock_mode;
```

<a href="/Classes/Lock">Lock</a> mode. 

### variable acquired

```cpp
bool acquired;
```

<code>true</code> if acquired, <code>false</code> if sleeping. 

### variable cond

```cpp
pthread_cond_t * cond;
```

Conditional variable for this lock. 

### variable list

```cpp
LockList * list;
```

<a href="/Classes/Lock">Lock</a> list. 

### variable trx_id

```cpp
trxid_t trx_id;
```

Transaction id. 

### variable mask

```cpp
lockmask_t mask;
```

<a href="/Classes/Lock">Lock</a> mask for shared lock compression. 

### variable prev

```cpp
Lock * prev;
```

Previous waiting lock. 

### variable next

```cpp
Lock * next;
```

Next waiting lock. 

### variable next_trx

```cpp
Lock * next_trx;
```

Next transaction lock. 

-------------------------------

Updated on 2021-12-05 at 18:36:40 +0900