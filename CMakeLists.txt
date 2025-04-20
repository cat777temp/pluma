cmake_minimum_required(VERSION 3.10)

project(pluma VERSION 1.0.0 LANGUAGES CXX)

# 设置 C++ 标准
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)

# 输出目录
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY "${CMAKE_BINARY_DIR}/bin")
set(CMAKE_LIBRARY_OUTPUT_DIRECTORY "${CMAKE_BINARY_DIR}/lib")
set(CMAKE_ARCHIVE_OUTPUT_DIRECTORY "${CMAKE_BINARY_DIR}/lib")

# 编译选项
if(MSVC)
    add_compile_options(/W4)
    add_definitions(-DPLUMA_STATIC -DPLUMA_EXPORTS)
else()
    add_compile_options(-Wall -Wextra -O3 -fPIC)
    add_definitions(-DPLUMA_STATIC -DPLUMA_EXPORTS)
endif()

# 启用位置无关代码（PIC）
set(CMAKE_POSITION_INDEPENDENT_CODE ON)

# Pluma 库源文件
set(PLUMA_SOURCES
    src/Pluma/Dir.cpp
    src/Pluma/Dir.hpp
    src/Pluma/DLibrary.cpp
    src/Pluma/DLibrary.hpp
    src/Pluma/Host.cpp
    src/Pluma/PluginManager.cpp
    src/Pluma/Provider.cpp
    src/Pluma/uce-dirent.h
)

# 添加 Pluma 库
add_library(pluma STATIC ${PLUMA_SOURCES})

# 设置包含目录
target_include_directories(pluma
    PUBLIC
        $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>
        $<INSTALL_INTERFACE:include>
    PRIVATE
        ${CMAKE_CURRENT_SOURCE_DIR}/src
)

# 链接库
if(UNIX AND NOT APPLE)
    target_link_libraries(pluma dl)
endif()

# 安装规则
install(TARGETS pluma
    EXPORT pluma-targets
    LIBRARY DESTINATION lib
    ARCHIVE DESTINATION lib
    RUNTIME DESTINATION bin
    INCLUDES DESTINATION include
)

install(DIRECTORY include/ DESTINATION include)

# 添加示例子目录
add_subdirectory(example)
