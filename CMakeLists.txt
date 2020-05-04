# Works with 3.11 and tested through 3.15
cmake_minimum_required(VERSION 3.11...3.15)

set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

# Project name
project(DRONE_PROJECT
    DESCRIPTION "Drone project"
    LANGUAGES CXX)

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

#  Unit test and test options, turn on with 'cmake -DUnitTests=ON
include(CMakeDependentOption)
option(Apps "Build native apps (e.g. elfly, elland, etc)" ON)
option(UnitTests "Build unit tests" ON)
option(Tests "Build tests" ON)


# Support folders in the ide
set_property(GLOBAL PROPERTY USE_FOLDERS OFF) # Use folders

# Setting up our specialized CMake modules
set(CMAKE_MODULE_PATH ${CMAKE_MODULE_PATH} ${PROJECT_SOURCE_DIR}/tools/cmake)


if(CMAKE_PROJECT_NAME STREQUAL PROJECT_NAME)

    #Let's ensure -std=c++xx instead of -std=g++xx
    set(CMAKE_CXX_EXTENSIONS OFF)




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


if(UNIX OR Apps)
    add_subdirectory(apps/elfly)
endif()

#add_executable(DRONE_PROJECT main.cpp)
