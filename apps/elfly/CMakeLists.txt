cmake_minimum_required(VERSION 3.15 FATAL_ERROR)

project(elfly LANGUAGES CXX)


add_executable(elfly
    src/elfly.cpp
    src/usage.h
    )

target_include_directories(elfly PUBLIC src)
