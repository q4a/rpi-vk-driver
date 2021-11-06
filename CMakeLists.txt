cmake_minimum_required (VERSION 3.10.2)

cmake_policy(SET CMP0048 NEW)

if(NOT CMAKE_BUILD_TYPE)
    set(CMAKE_BUILD_TYPE "Release" CACHE STRING "Choose the type of build, options are: Debug, Release, or MinSizeRel." FORCE)
    message(STATUS "CMAKE_BUILD_TYPE not set, defaulting to Release.")
endif()

set(CMAKE_MODULE_PATH ${CMAKE_MODULE_PATH} ${CMAKE_CURRENT_SOURCE_DIR}/cmake)

if(NOT BUILD_NUMBER)
    set(BUILD_NUMBER 1.0.0)
endif()

project (rpi-vulkan-driver
	VERSION ${BUILD_NUMBER}
	DESCRIPTION "Driver for Broadcom Videocore IV GPU"
	LANGUAGES C CXX
)

#
# Options
#
option(BUILD_INSTALL_TESTS "Install Test cases" OFF)


#
# Dependencies
#

find_package(Vulkan)

if (NOT Vulkan_FOUND)
    find_library(Vulkan_LIBRARY NAMES vulkan HINTS "$ENV{VULKAN_SDK}/lib" REQUIRED)
    if (Vulkan_LIBRARY)
        set(Vulkan_FOUND ON)
    endif()
endif ()

find_package(PkgConfig REQUIRED)
pkg_check_modules(System REQUIRED libdrm expat zlib)

set(CMAKE_THREAD_PREFER_PTHREAD TRUE)
set(THREADS_PREFER_PTHREAD_FLAG TRUE)
find_package(Threads REQUIRED)

#
# Global
#

set(CMAKE_C_STANDARD 99)
set(CMAKE_C_EXTENSIONS OFF)

set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_EXTENSIONS OFF)

set(CMAKE_POSITION_INDEPENDENT_CODE ON)

if(${CMAKE_BUILD_TYPE} STREQUAL "Debug")
	add_definitions(-DDEBUG_BUILD)
    add_compile_options(-Wall)
else()
    # filter as-needed here
    add_compile_options(-Wall) #-Wno-unused-variable -Wno-unused-parameter -Wno-unused-but-set-variable)
    #add_compile_options(-Werror)
endif()

add_subdirectory(brcm)
add_subdirectory(QPUassembler)
add_subdirectory(driver)

include(packaging)

include(CTest)

if(BUILD_TESTING)
    # for tests
    include_directories(${CMAKE_SOURCE_DIR})
    add_subdirectory(test)
endif()

add_custom_target(uninstall 
    "${CMAKE_COMMAND}" -P "${CMAKE_SOURCE_DIR}/cmake/uninstall.cmake"
)

add_custom_target(uninstall-vulkan
    "${CMAKE_COMMAND}" -P "${CMAKE_BINARY_DIR}/vulkan-headers-prefix/src/vulkan-headers-build/cmake_uninstall.cmake"
)

add_custom_target(clean-vulkan
    "${CMAKE_COMMAND}" -E remove_directory "${CMAKE_BINARY_DIR}/vulkan-headers-prefix"
    "${CMAKE_COMMAND}" -E remove_directory "${CMAKE_BINARY_DIR}/vulkan-loader-prefix"
)
