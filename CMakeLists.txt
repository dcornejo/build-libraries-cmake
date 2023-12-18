cmake_minimum_required(VERSION 3.28)
project(dummylib LANGUAGES C VERSION 1.0.0)
set(CMAKE_C_STANDARD 11)

# Introduce SOURCE_FILES variable for library source
set(SOURCE_FILES library.c)

# Define static library variable
set(STATIC_LIB dummylib_static)

# Define shared library variable
set(SHARED_LIB dummylib)

# Use the variables instead in the following command
add_library(${STATIC_LIB} STATIC ${SOURCE_FILES})
add_library(${SHARED_LIB} SHARED ${SOURCE_FILES})
add_custom_target(dummylib_target DEPENDS ${STATIC_LIB} ${SHARED_LIB})

set_target_properties(dummylib PROPERTIES SOVERSION ${CMAKE_PROJECT_VERSION})
