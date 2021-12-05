

# lock_helper

**Module:** **[LockManager](/Modules/LockManager)**



## Functions

|                | Name           |
| -------------- | -------------- |
| constexpr int | **[get_bit](/Namespaces/lock_helper#function-get_bit)**(<a href="/Classes/Lock">Lock</a> * lock, int pos)<br>Get the pos-th bit of the lock key mask.  |
| constexpr int | **[set_bit](/Namespaces/lock_helper#function-set_bit)**(<a href="/Classes/Lock">Lock</a> * lock, int pos)<br>Set the pos-th bit of the lock key mask.  |
| constexpr int | **[clear_bit](/Namespaces/lock_helper#function-clear_bit)**(uint64_t & mask, int pos)<br>Clear the pos-th bit of the mask.  |
| bool | **[_find_deadlock](/Namespaces/lock_helper#function-_find_deadlock)**(trxid_t current, trxid_t root, std::unordered_set< trxid_t > visit) |
| bool | **[find_deadlock](/Namespaces/lock_helper#function-find_deadlock)**(trxid_t root) |


## Functions Documentation

### function get_bit

```cpp
constexpr int get_bit(
    Lock * lock,
    int pos
)
```

Get the pos-th bit of the lock key mask. 

**Parameters**: 

  * **lock** lock object. 
  * **pos** position. 


**Return**: 0 or 1. 

### function set_bit

```cpp
constexpr int set_bit(
    Lock * lock,
    int pos
)
```

Set the pos-th bit of the lock key mask. 

**Parameters**: 

  * **lock** lock object. 
  * **pos** position. 


**Return**: 0 or 1. 

### function clear_bit

```cpp
constexpr int clear_bit(
    uint64_t & mask,
    int pos
)
```

Clear the pos-th bit of the mask. 

**Parameters**: 

  * **mask** bit mask. 
  * **pos** position. 


**Return**: 0 or 1. 

### function _find_deadlock

```cpp
bool _find_deadlock(
    trxid_t current,
    trxid_t root,
    std::unordered_set< trxid_t > visit
)
```


### function find_deadlock

```cpp
bool find_deadlock(
    trxid_t root
)
```






-------------------------------

Updated on 2021-12-05 at 18:36:40 +0900