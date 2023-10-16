cmake_minimum_required(VERSION 3.27.4)

set(CMAKE_CXX_STANDARD 23)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

project("Phoenix")

file(GLOB_RECURSE HEADERS "${CMAKE_SOURCE_DIR}/source/*.hpp")
file(GLOB_RECURSE SOURCES "${CMAKE_SOURCE_DIR}/source/*.cpp")

add_library(phoenixStatic STATIC 
    ${HEADERS}
    ${SOURCES}
)

add_library(phoenixShared SHARED 
    ${HEADERS}
    ${SOURCES}
)

# Configuration
set(INCLUDE_DIRECTORIES 
    "${CMAKE_SOURCE_DIR}/source/"
)

set(COMPILATION_FLAGS
    -std=c++23 -fPIC 
    -Wall -Wextra -Werror -Wpedantic -g 
)

set(LIBRARY_LINK_FLAGS
    phoenixStatic 
    phoenixShared 
)

# Compilation flags
target_compile_options(phoenixStatic PRIVATE 
    ${COMPILATION_FLAGS}
)

target_compile_options(phoenixShared PRIVATE 
    ${COMPILATION_FLAGS}
)

# Header include
target_include_directories(phoenixStatic PRIVATE 
    ${INCLUDE_DIRECTORIES}
)

target_include_directories(phoenixShared PRIVATE 
    ${INCLUDE_DIRECTORIES}
)

# Target properties
set_target_properties(phoenixStatic phoenixShared PROPERTIES 
    OUTPUT_NAME "Phoenix"
    ARCHIVE_OUTPUT_DIRECTORY "${CMAKE_SOURCE_DIR}/"
    LIBRARY_OUTPUT_DIRECTORY "${CMAKE_SOURCE_DIR}/"
)