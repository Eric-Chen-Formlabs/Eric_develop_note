


# CMake实用笔记（精简版）

## 1. 基本概念
```cmake
cmake_minimum_required(VERSION 3.15)
project(ProjectName)

# 添加可执行文件
add_executable(myapp main.cpp utils.cpp)

# 添加库
add_library(mylib STATIC lib.cpp)

# 链接库
target_link_libraries(myapp PRIVATE mylib)
```

## 2. 项目结构
```cmake
# 主CMakeLists.txt
add_subdirectory(src)
add_subdirectory(sanic)

# 子目录CMakeLists.txt自动被处理
```

## 3. 编译配置
```cmake
# 设置编译标志
target_compile_options(myapp PRIVATE
    -Wall -O2 -mcpu=cortex-m4
)

# 添加宏定义
target_compile_definitions(myapp PRIVATE
    STM32F303x8
    DEBUG=1
)

# 添加头文件路径
target_include_directories(myapp PRIVATE
    include/
    ../lib/vendor/HAL/Inc
)
```

## 4. 工具链设置
```cmake
# toolchain文件
set(CMAKE_C_COMPILER arm-none-eabi-gcc)
set(CMAKE_CXX_COMPILER arm-none-eabi-g++)

# 使用：
# cmake -DCMAKE_TOOLCHAIN_FILE=gcc-arm.toolchain ..
```

## 5. 常用变量
```cmake
set(SOURCES main.c gpio.c uart.c)
set(CMAKE_BUILD_TYPE Release)  # Debug/Release

# 条件编译
if(CMAKE_BUILD_TYPE STREQUAL "Debug")
    target_compile_definitions(myapp PRIVATE DEBUG_MODE)
endif()
```

## 6. 依赖管理
```cmake
# 内部依赖
target_link_libraries(app PRIVATE driver_lib)

# 查找系统库
find_package(Threads REQUIRED)
target_link_libraries(app PRIVATE Threads::Threads)
```

## 7. 自定义命令（嵌入式常用）
```cmake
# 生成.bin文件
add_custom_command(TARGET myapp POST_BUILD
    COMMAND ${CMAKE_OBJCOPY} -O binary 
    $<TARGET_FILE:myapp> myapp.bin
)
```

## 8. 常用命令行
```bash
# 配置
mkdir build && cd build
cmake -DCMAKE_BUILD_TYPE=Release ..

# 构建
cmake --build .                    # 构建所有
cmake --build . --target myapp     # 构建特定目标
cmake --build . --parallel 8       # 并行构建

# 清理
cmake --build . --target clean
```

## 9. 调试技巧
```cmake
# 打印信息
message(STATUS "Build type: ${CMAKE_BUILD_TYPE}")

# 打印变量值
message(STATUS "Sources: ${SOURCES}")
```

```bash
# 查看所有可用目标
cmake --build . --target help

# 查看配置变量
cmake -L
```

## 10. 最佳实践
- ✅ 使用`target_*`命令（推荐）
- ✅ 明确指定PRIVATE/PUBLIC
- ✅ Out-of-source构建（build目录）
- ❌ 避免全局的`include_directories()`
- ❌ 避免`file(GLOB)`收集源文件

## 11. 典型嵌入式项目结构
```
project/
├── CMakeLists.txt          # 主配置
├── toolchain/
│   └── gcc-arm.toolchain   # 工具链文件
├── src/
│   ├── CMakeLists.txt      # 源码配置
│   └── main.c
├── lib/                    # 第三方库
└── build/                  # 构建目录
```

