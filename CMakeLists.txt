# Works with 3.11 and tested through 3.15
cmake_minimum_required(VERSION 3.11...3.15)

set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

# Check for the right version
if(${CMAKE_VERSION} VERSION_LESS 3.12)
    cmake_policy(VERSION ${CMAKE_MAJOR_VERSION}.${CMAKE_MINOR_VERSION})
else()
    cmake_policy(VERSION 3.15)
endif()


# We default to Release build type
# options are: Debug Release RelWithDebInfo MinSizeRel or None
if(NOT CMAKE_BUILD_TYPE)
    set(CMAKE_BUILD_TYPE "Release")
endif()

include(CMakeDependentOption)


# Prject name
project(DRONE_PROJECT
    DESCRIPTION "Drone project"
    LANGUAGES CXX)


if(CMAKE_PROJECT_NAME STREQUAL PROJECT_NAME)

    #Let's ensure -std=c++xx instead of -std=g++xx
    set(CMAKE_CXX_EXTENSIONS OFF)

    # Support folders in the ide
    set_property(GLOBAL PROPERTY USE_FOLDER ON)

    #Setting up testing
    include(CTest)

    find_package(Doxygen)
    if(Doxygen_FOUND)
        add_subdirectory(docs)
    else()
        message(STATUS " Doxygen not found, not building docs ")
    endif()
endif()


# FetchContent added in CMake 3.111
include(FetchContent)


add_subdirectory(src)

add_executable(DRONE_PROJECT main.cpp)
