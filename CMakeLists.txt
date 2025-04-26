cmake_minimum_required(VERSION 3.5)

project(libasd)

enable_testing()

if(MSVC)
    add_definitions("/Zc:__cplusplus") # define __cplusplus value correctly
    add_definitions("/utf-8")           # enable to use u8"" literal
else()
    add_definitions("-Wall -Wextra -Wpedantic")
endif()

include_directories(${PROJECT_SOURCE_DIR})

find_package(pybind11 REQUIRED)

option(LIBASD_BUILD_TESTS ON)
if(LIBASD_BUILD_TESTS)
    find_package(Boost 1.62.0)
    if(Boost_FOUND)
        message(STATUS "Boost libraries found in ${Boost_INCLUDE_DIRS}")
        include_directories(${Boost_INCLUDE_DIRS})
    else()
        message(STATUS "If you have Boost (e.g. /opt/boost-1.??.??) and CMake couldn't find Boost, try")
        message(STATUS "`cmake path/to/CMakeLists.txt -DBOOST_ROOT=/path/to/boost`")
        message(STATUS "current BOOST_ROOT = ${BOOST_ROOT}")
    endif()

    if(Boost_FOUND)
        add_subdirectory(test)
    endif()
endif()

add_subdirectory(python)

