

# db/include/const.h



## Attributes

|                | Name           |
| -------------- | -------------- |
| constexpr int | **[INITIAL_TABLE_FILE_SIZE](/Modules/DiskSpaceManager#variable-initial_table_file_size)** <br>Initial size(in bytes) of newly created table file.  |
| constexpr int | **[MAX_TABLE_INSTANCE](/Modules/DiskSpaceManager#variable-max_table_instance)** <br>Maximum number of table instances count.  |
| constexpr int | **[INITIAL_TABLE_CAPS](/Modules/DiskSpaceManager#variable-initial_table_caps)** <br>Initial number of page count in newly created table file.  |
| constexpr int | **[PAGE_SIZE](/Modules/DiskSpaceManager#variable-page_size)** <br>Size of each page(in bytes).  |
| constexpr int | **[PAGE_HEADER_SIZE](/Modules/DiskSpaceManager#variable-page_header_size)** <br>Size of page header(in bytes).  |
| constexpr int | **[MAX_PAGE_BRANCHES](/Modules/DiskSpaceManager#variable-max_page_branches)** <br>Maximum number of page branches.  |
| constexpr int | **[DEFAULT_BUFFER_SIZE](/Modules/BufferManager#variable-default_buffer_size)** <br>Initial number of buffer pages when initializing db.  |
| constexpr int | **[REDISTRIBUTE_THRESHOLD](/Modules/IndexManager#variable-redistribute_threshold)** <br>Redistribution threshold for split leaf nodes.  |
| constexpr int | **[MAX_VALUE_SIZE](/Modules/IndexManager#variable-max_value_size)** <br>Maximum size of the leaf node record size.  |



## Attributes Documentation

### variable INITIAL_TABLE_FILE_SIZE

```cpp
constexpr int INITIAL_TABLE_FILE_SIZE = 10 * 1024 * 1024;
```

Initial size(in bytes) of newly created table file. 

It means 10MiB. 


### variable MAX_TABLE_INSTANCE

```cpp
constexpr int MAX_TABLE_INSTANCE = 32;
```

Maximum number of table instances count. 

### variable INITIAL_TABLE_CAPS

```cpp
constexpr int INITIAL_TABLE_CAPS = INITIAL_TABLE_FILE_SIZE / MAX_TABLE_INSTANCE;
```

Initial number of page count in newly created table file. 

Its value is 2560. 


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

### variable DEFAULT_BUFFER_SIZE

```cpp
constexpr int DEFAULT_BUFFER_SIZE = 1024;
```

Initial number of buffer pages when initializing db. 

### variable REDISTRIBUTE_THRESHOLD

```cpp
constexpr int REDISTRIBUTE_THRESHOLD = 2500;
```

Redistribution threshold for split leaf nodes. 

### variable MAX_VALUE_SIZE

```cpp
constexpr int MAX_VALUE_SIZE = 112;
```

Maximum size of the leaf node record size. 


## Source code

```cpp
#pragma once

constexpr int INITIAL_TABLE_FILE_SIZE = 10 * 1024 * 1024;

constexpr int MAX_TABLE_INSTANCE = 32;

constexpr int INITIAL_TABLE_CAPS = INITIAL_TABLE_FILE_SIZE / MAX_TABLE_INSTANCE;

constexpr int PAGE_SIZE = 4096;

constexpr int PAGE_HEADER_SIZE = 128;

constexpr int MAX_PAGE_BRANCHES = 248;

constexpr int DEFAULT_BUFFER_SIZE = 1024;

constexpr int REDISTRIBUTE_THRESHOLD = 2500;
constexpr int MAX_VALUE_SIZE = 112;
```


-------------------------------

Updated on 2021-12-05 at 18:53:29 +0900
