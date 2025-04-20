cmake_minimum_required(VERSION 3.10)

# 设置包含目录
include_directories(
    ${CMAKE_CURRENT_SOURCE_DIR}/src/interface
    ${CMAKE_SOURCE_DIR}/include
)

# 接口库源文件
set(INTERFACE_SOURCES
    src/interface/Warrior.cpp
    src/interface/Warrior.hpp
)

# 主程序源文件
set(HOST_SOURCES
    src/host/Main.cpp
    src/host/SimpleWarrior.hpp
)

# 插件源文件
set(PLUGIN_SOURCES
    src/plugin/Connector.cpp
    src/plugin/Eagle.hpp
    src/plugin/Jaguar.hpp
)

# 添加接口库
add_library(warrior_interface STATIC ${INTERFACE_SOURCES})
target_link_libraries(warrior_interface pluma)

# 确保接口库使用PIC编译
set_property(TARGET warrior_interface PROPERTY POSITION_INDEPENDENT_CODE ON)

# 添加主程序
add_executable(warrior_host ${HOST_SOURCES})
target_link_libraries(warrior_host pluma warrior_interface)

# 添加插件
add_library(warrior_plugin MODULE ${PLUGIN_SOURCES})
target_link_libraries(warrior_plugin pluma warrior_interface)

# 定义插件输出目录
# 对于Visual Studio，需要考虑Debug/Release子目录
if(MSVC)
    set(PLUGIN_OUTPUT_DIR "${CMAKE_BINARY_DIR}/bin/$<CONFIG>/plugins")
else()
    set(PLUGIN_OUTPUT_DIR "${CMAKE_BINARY_DIR}/bin/plugins")
endif()

# 设置插件输出目录
set_target_properties(warrior_plugin PROPERTIES
    LIBRARY_OUTPUT_DIRECTORY "${PLUGIN_OUTPUT_DIR}"
    LIBRARY_OUTPUT_DIRECTORY_DEBUG "${PLUGIN_OUTPUT_DIR}"
    LIBRARY_OUTPUT_DIRECTORY_RELEASE "${PLUGIN_OUTPUT_DIR}"
    RUNTIME_OUTPUT_DIRECTORY "${PLUGIN_OUTPUT_DIR}"
    RUNTIME_OUTPUT_DIRECTORY_DEBUG "${PLUGIN_OUTPUT_DIR}"
    RUNTIME_OUTPUT_DIRECTORY_RELEASE "${PLUGIN_OUTPUT_DIR}"
)

# 在Windows上，MODULE库的后缀是.dll，与SHARED库相同
if(WIN32)
    set_target_properties(warrior_plugin PROPERTIES SUFFIX ".dll")
endif()

# 复制插件到输出目录
add_custom_command(TARGET warrior_plugin POST_BUILD
    COMMAND ${CMAKE_COMMAND} -E make_directory "${PLUGIN_OUTPUT_DIR}"
    COMMAND ${CMAKE_COMMAND} -E copy $<TARGET_FILE:warrior_plugin> "${PLUGIN_OUTPUT_DIR}/"
)

# 为主程序创建plugins目录
add_custom_command(TARGET warrior_host POST_BUILD
    COMMAND ${CMAKE_COMMAND} -E make_directory "${CMAKE_BINARY_DIR}/bin/$<CONFIG>/plugins"
    COMMAND ${CMAKE_COMMAND} -E copy $<TARGET_FILE:warrior_plugin> "${CMAKE_BINARY_DIR}/bin/$<CONFIG>/plugins/"
)
