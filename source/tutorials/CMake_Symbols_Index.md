# CMake

> [Symbols Index](https://cmake.org/cmake/help/latest/genindex.html)

## [add_subdirectory](https://cmake.org/cmake/help/latest/command/add_subdirectory.html#add-subdirectory)

- 在构建中添加一个子目录。

```
add_subdirectory(source_dir [binary_dir] [EXCLUDE_FROM_ALL] [SYSTEM])
```

向构建添加一个子目录。指定源文件和代码文件所在的`source_dir`目录。`CMakeLists.txt`如果它是相对路径，它将相对于当前目录（典型用法）进行评估，但它也可能是绝对路径。指定`binary_dir`放置输出文件的目录。如果它是相对路径，则将根据当前输出目录对其进行评估，但它也可能是绝对路径。如果`binary_dir`未指定，则在展开任何相对路径之前将使用 的值 `source_dir`（典型用法）。`CMakeLists.txt`CMake 将立即处理指定源目录中的文件，然后在此命令之后继续对当前输入文件进行处理。

## [include_directories](https://cmake.org/cmake/help/latest/command/include_directories.html#command:include_directories)

- 将包含目录添加到构建中.一般用于导入头文件，**这个命令会给所有的目标添加头文件的搜索路径**

```
include_directories([AFTER|BEFORE] [SYSTEM] dir1 [dir2 ...])
```

## [target_include_directories](https://cmake.org/cmake/help/latest/command/target_include_directories.html#command:target_include_directories)

- 将包含目录添加到目标。**这个命令会给特定的目标（例如一个库或者一个可执行文件）添加头文件的搜索路径。添加的路径只对指定的目标有效，而不会影响其他目标。这种方式更推荐使用**

```
target_include_directories(<target> [SYSTEM] [AFTER|BEFORE]
  <INTERFACE|PUBLIC|PRIVATE> [items1...]
  [<INTERFACE|PUBLIC|PRIVATE> [items2...] ...])
```

指定编译给定目标时要使用的包含目录。命名的`<target>`必须是由命令创建的，例如[`add_executable()`](https://cmake.org/cmake/help/latest/command/add_executable.html#command:add_executable)或者[`add_library()`](https://cmake.org/cmake/help/latest/command/add_library.html#command:add_library)并且不能是 [ALIAS 目标](https://cmake.org/cmake/help/latest/manual/cmake-buildsystem.7.html#alias-targets)。