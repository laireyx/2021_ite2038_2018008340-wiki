

# db/include/types.h



## Namespaces

| Name           |
| -------------- |
| **[std](/Namespaces/std)**  |

## Classes

|                | Name           |
| -------------- | -------------- |
| struct | **[std::hash< PageLocation >](/Classes/std::hash< PageLocation >)**  |

## Types

|                | Name           |
| -------------- | -------------- |
| typedef int | **[trxid_t](/Files/db/include/types.h#typedef-trxid_t)**  |
| typedef uint64_t | **[trxlogid_t](/Files/db/include/types.h#typedef-trxlogid_t)**  |
| typedef uint64_t | **[pagenum_t](/Files/db/include/types.h#typedef-pagenum_t)**  |
| typedef int64_t | **[tableid_t](/Files/db/include/types.h#typedef-tableid_t)**  |
| typedef int64_t | **[recordkey_t](/Files/db/include/types.h#typedef-recordkey_t)**  |
| typedef uint16_t | **[valsize_t](/Files/db/include/types.h#typedef-valsize_t)**  |
| typedef uint64_t | **[lockmask_t](/Files/db/include/types.h#typedef-lockmask_t)**  |
| typedef std::pair< tableid_t, pagenum_t > | **[PageLocation](/Files/db/include/types.h#typedef-pagelocation)**  |
| typedef std::pair< tableid_t, pagenum_t > | **[LockLocation](/Files/db/include/types.h#typedef-locklocation)**  |

## Types Documentation

### typedef trxid_t

```cpp
typedef int trxid_t;
```


### typedef trxlogid_t

```cpp
typedef uint64_t trxlogid_t;
```


### typedef pagenum_t

```cpp
typedef uint64_t pagenum_t;
```


### typedef tableid_t

```cpp
typedef int64_t tableid_t;
```


### typedef recordkey_t

```cpp
typedef int64_t recordkey_t;
```


### typedef valsize_t

```cpp
typedef uint16_t valsize_t;
```


### typedef lockmask_t

```cpp
typedef uint64_t lockmask_t;
```


### typedef PageLocation

```cpp
typedef std::pair<tableid_t, pagenum_t> PageLocation;
```


### typedef LockLocation

```cpp
typedef std::pair<tableid_t, pagenum_t> LockLocation;
```





## Source code

```cpp
#pragma once

#include <cstdint>
#include <functional>
#include <memory>

typedef int trxid_t;
typedef uint64_t trxlogid_t;

typedef uint64_t pagenum_t;
typedef int64_t tableid_t;

typedef int64_t recordkey_t;
typedef uint16_t valsize_t;

typedef uint64_t lockmask_t;

typedef std::pair<tableid_t, pagenum_t> PageLocation, LockLocation;

namespace std {
template <>
struct hash<PageLocation> {
    size_t operator()(const PageLocation& location) const {
        size_t hash_value = 17;
        hash_value = hash_value * 31 + std::hash<tableid_t>()(location.first);
        hash_value = hash_value * 31 + std::hash<pagenum_t>()(location.second);
        return hash_value;
    }
};
}  // namespace std
```


-------------------------------

Updated on 2021-12-05 at 18:53:29 +0900
