cmake_minimum_required(VERSION 3.8)
project(cmake_lib LANGUAGES CXX)

# For clang tooling
set(CMAKE_EXPORT_COMPILE_COMMANDS ON)

# Create executable target
add_library(cmake_lib STATIC src/library.cpp)
add_library(cmake_lib::library ALIAS cmake_lib)

target_include_directories(cmake_lib
    PUBLIC
        ./include
    PRIVATE
        ./src
)

# Add custom target to copy compile_commands.json at project root
add_custom_target(cmake_lib_db
    ALL
    COMMAND
        ${CMAKE_COMMAND} -E copy_if_different ${CMAKE_BINARY_DIR}/compile_commands.json
                                              ${CMAKE_SOURCE_DIR}/compile_commands.json
    DEPENDS
        ${CMAKE_BINARY_DIR}/compile_commands.json
)

