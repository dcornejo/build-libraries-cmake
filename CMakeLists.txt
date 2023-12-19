cmake_minimum_required(VERSION 3.27)
project(dummylib LANGUAGES C VERSION 1.0.0)

set(CMAKE_C_STANDARD 11)
set(VERSION_NUM ${CMAKE_PROJECT_VERSION})
set(SOURCE_FILES library.c)  # Source file
set(HEADER_FILES library.h)
set(STATIC_LIB dummylib_static)  # Static library name
set(SHARED_LIB dummylib)  # Shared library name
set(DUMMYLIB_TARGET dummylib_target)  # Custom target name
set(COMPILE_OPTIONS -Wall -Werror)  # Extracted compiler options

include(CMakePrintHelpers)

# Define libraries
add_library(${STATIC_LIB} STATIC ${SOURCE_FILES})
add_library(${SHARED_LIB} SHARED ${SOURCE_FILES})

# Use extracted compiler options
target_compile_options(${STATIC_LIB} PRIVATE ${COMPILE_OPTIONS})
target_compile_options(${SHARED_LIB} PRIVATE ${COMPILE_OPTIONS})

set_property(TARGET ${SHARED_LIB} PROPERTY POSITION_INDEPENDENT_CODE ON)
set_target_properties(${SHARED_LIB} PROPERTIES SOVERSION ${VERSION_NUM})

# Define custom target depending on both libraries
add_custom_target(${DUMMYLIB_TARGET} DEPENDS ${STATIC_LIB} ${SHARED_LIB})

install(TARGETS ${SHARED_LIB} RUNTIME DESTINATION lib)
install(FILES ${HEADER_FILES} DESTINATION include/dummy)

if (${CMAKE_SYSTEM_NAME} STREQUAL "Linux")
    set(CPACK_GENERATOR "DEB")
    set(CPACK_DEBIAN_PACKAGE_DESCRIPTION "dummy library")
    set(CPACK_DEBIAN_PACKAGE_MAINTAINER "dave@dogwood.com") #required
    message("Linux Build")
endif ()

include(CPack)

