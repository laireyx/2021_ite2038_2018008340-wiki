

# db/src/file.cc



## Namespaces

| Name           |
| -------------- |
| **[file_helper](/Namespaces/file_helper)** <br>Filemanager helper.  |

## Functions

|                | Name           |
| -------------- | -------------- |
| int | **[file_open_database_file](/Files/db/src/file.cc#function-file_open_database_file)**(const char * path)<br>Open existing database file or create one if not existed.  |
| pagenum_t | **[file_alloc_page](/Files/db/src/file.cc#function-file_alloc_page)**(int fd)<br>Allocate an on-disk page from the free page list.  |
| void | **[file_free_page](/Files/db/src/file.cc#function-file_free_page)**(int fd, pagenum_t pagenum)<br>Free an on-disk page to the free page list.  |
| void | **[file_read_page](/Files/db/src/file.cc#function-file_read_page)**(int fd, pagenum_t pagenum, <a href="/Classes/Page">page_t</a> * dest)<br>Read an on-disk page into the in-memory page structure(dest)  |
| void | **[file_write_page](/Files/db/src/file.cc#function-file_write_page)**(int fd, pagenum_t pagenum, const <a href="/Classes/Page">page_t</a> * src)<br>Write an in-memory page(src) to the on-disk page.  |
| void | **[file_close_database_file](/Files/db/src/file.cc#function-file_close_database_file)**()<br>Stop referencing the database file.  |

## Attributes

|                | Name           |
| -------------- | -------------- |
| int | **[database_instance_count](/Files/db/src/file.cc#variable-database_instance_count)** <br>current database instance number  |
| <a href="/Classes/DatabaseInstance">DatabaseInstance</a> | **[database_instances](/Files/db/src/file.cc#variable-database_instances)** <br>all database instances  |
| int | **[database_fd](/Files/db/src/file.cc#variable-database_fd)** <br>currently opened database file descriptor  |
| <a href="/Classes/HeaderPage">headerpage_t</a> | **[header_page](/Files/db/src/file.cc#variable-header_page)** <br>currently opened database header page  |


## Functions Documentation

### function file_open_database_file

```cpp
int file_open_database_file(
    const char * path
)
```

Open existing database file or create one if not existed. 

**Parameters**: 

  * **path** Database file path. 


**Return**: ID of the opened database file. 

### function file_alloc_page

```cpp
pagenum_t file_alloc_page(
    int fd
)
```

Allocate an on-disk page from the free page list. 

**Parameters**: 

  * **fd** Database file descriptor obtained with <code>file&#95;open&#95;database&#95;file()</code>. 


**Return**: Allocated page index. 

### function file_free_page

```cpp
void file_free_page(
    int fd,
    pagenum_t pagenum
)
```

Free an on-disk page to the free page list. 

**Parameters**: 

  * **fd** Database file descriptor obtained with <code>file&#95;open&#95;database&#95;file()</code>. 
  * **pagenum** page index. 


### function file_read_page

```cpp
void file_read_page(
    int fd,
    pagenum_t pagenum,
    page_t * dest
)
```

Read an on-disk page into the in-memory page structure(dest) 

**Parameters**: 

  * **fd** Database file descriptor obtained with <code>file&#95;open&#95;database&#95;file()</code>. 
  * **pagenum** page index. 
  * **dest** the pointer of the page data. 


### function file_write_page

```cpp
void file_write_page(
    int fd,
    pagenum_t pagenum,
    const page_t * src
)
```

Write an in-memory page(src) to the on-disk page. 

**Parameters**: 

  * **fd** Database file descriptor obtained with <code>file&#95;open&#95;database&#95;file()</code>. 
  * **pagenum** page index. 
  * **src** the pointer of the page data. 


### function file_close_database_file

```cpp
void file_close_database_file()
```

Stop referencing the database file. 


## Attributes Documentation

### variable database_instance_count

```cpp
int database_instance_count = 0;
```

current database instance number 

### variable database_instances

```cpp
DatabaseInstance database_instances;
```

all database instances 

### variable database_fd

```cpp
int database_fd = 0;
```

currently opened database file descriptor 

### variable header_page

```cpp
headerpage_t header_page;
```

currently opened database header page 


## Source code

```cpp

#include <sys/types.h>
#include <sys/stat.h>
#include <fcntl.h>
#include <unistd.h>

#include <stdlib.h>
#include <string.h>
#include <assert.h>
#include "file.h"
#include "errors.h"

int database_instance_count = 0;
DatabaseInstance database_instances[MAX_DATABASE_INSTANCE];

int database_fd = 0;

headerpage_t header_page;

namespace file_helper {
    void switch_to_fd(int fd) {
        assert(fd > 0);
        if(database_fd == fd) return;

        database_fd = fd;
        error::check(pread64(database_fd, &header_page, PAGE_SIZE, 0));
    }

    void extend_capacity(pagenum_t newsize = 0) {
        if (
            newsize > header_page.page_num ||
            header_page.free_page_idx == 0
        ) {
            if (newsize == 0) {
                newsize = header_page.page_num * 2;
            }

            for (
                pagenum_t free_page_idx = header_page.page_num;
                free_page_idx < newsize;
                free_page_idx++
            ) {

                freepage_t free_page;

                if (free_page_idx < newsize - 1)
                    free_page.next_free_idx = free_page_idx + 1;
                else
                    free_page.next_free_idx = 0;

                error::check(pwrite64(database_fd, &free_page, PAGE_SIZE, free_page_idx * PAGE_SIZE));
            }

            error::check(fsync(database_fd));
            header_page.free_page_idx = header_page.page_num;
            header_page.page_num = newsize;
        }
    }

    void flush_header() {
        assert(database_fd > 0);
        error::check(pwrite64(database_fd, &header_page, PAGE_SIZE, 0));
        error::check(fdatasync(database_fd));
    }
};

int file_open_database_file(const char* path) {
    
    char* real_path = NULL;

    if((real_path = realpath(path, NULL)) > 0) {
        for (
            int instance_idx = 0;
            instance_idx < database_instance_count;
            instance_idx++
        ) {
            if (
                strcmp(
                    database_instances[instance_idx].file_path,
                    real_path
                ) == 0
            ) {
                free(real_path);
                return database_instances[instance_idx].file_descriptor;
            }
        }

        if (database_instance_count >= MAX_DATABASE_INSTANCE) {
            free(real_path);
            return -1;
        }

        free(real_path);
    }

    DatabaseInstance& new_instance = database_instances[database_instance_count++];

    if ((database_fd = open(path, O_RDWR)) < 1) {
        if(errno == ENOENT) {
            error::check(database_fd = open(path, O_RDWR | O_CREAT | O_EXCL, 0644));

            header_page.free_page_idx = 0;
            header_page.page_num = 1;

            file_helper::extend_capacity(2560);

            file_helper::flush_header();
        } else {
            return error::print();
        }
    }
    else {
        error::check(read(database_fd, &header_page, PAGE_SIZE));
    }

    new_instance.file_path = realpath(path, NULL);

    return (new_instance.file_descriptor = database_fd);
}

pagenum_t file_alloc_page(int fd) {
    file_helper::switch_to_fd(fd);
    file_helper::extend_capacity();

    pagenum_t free_page_idx = header_page.free_page_idx;
    freepage_t free_page;

    error::check(pread64(database_fd, &free_page, PAGE_SIZE, free_page_idx * PAGE_SIZE));
    header_page.free_page_idx = free_page.next_free_idx;

    file_helper::flush_header();

    return free_page_idx;
}

void file_free_page(int fd, pagenum_t pagenum) {
    file_helper::switch_to_fd(fd);

    pagenum_t old_free_page_idx = header_page.free_page_idx;
    freepage_t new_free_page;

    new_free_page.next_free_idx = old_free_page_idx;
    error::check(pwrite64(database_fd, &new_free_page, PAGE_SIZE, pagenum * PAGE_SIZE));
    error::check(fdatasync(database_fd));
    header_page.free_page_idx = pagenum;

    file_helper::flush_header();

    return;
}

void file_read_page(int fd, pagenum_t pagenum, page_t* dest) {
    file_helper::switch_to_fd(fd);
    error::check(pread64(database_fd, dest, PAGE_SIZE, pagenum * PAGE_SIZE));
}

void file_write_page(int fd, pagenum_t pagenum, const page_t* src) {
    file_helper::switch_to_fd(fd);
    error::check(pwrite64(database_fd, src, PAGE_SIZE, pagenum * PAGE_SIZE));
    error::check(fdatasync(database_fd));
}

void file_close_database_file() {
    for (
        int instance_idx = 0;
        instance_idx < database_instance_count;
        instance_idx++
    ) {
        close(database_instances[instance_idx].file_descriptor);
        free(database_instances[instance_idx].file_path);
    }

    database_instance_count = 0;
    database_fd = 0;
}
```


-------------------------------

Updated on 2021-09-30 at 19:53:44 +0900