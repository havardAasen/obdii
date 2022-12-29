cmake_minimum_required(VERSION 3.12)
project(obdii LANGUAGES C)

option(BUILD_SHARED
        "Build a shared version of the obdii library." ON)
option(BUILD_STATIC
        "Build a static version of the obdii library." ON)
option(PYTHON_BINDINGS
        "Install Python3 bindings for obdii library." ON)


set(CMAKE_C_STANDARD 99)
add_compile_options(-Wall -Wextra)

include(GNUInstallDirs)
include(CheckIncludeFile)
include(CMakeDependentOption)

check_include_file(linux/can.h HAVE_LINUX_CAN_H)
if(NOT HAVE_LINUX_CAN_H)
    message(SEND_ERROR "The header file 'linux/can.h' is required")
endif()

find_package(Python COMPONENTS Interpreter)
cmake_dependent_option(PYTHON_BINDINGS
        "if enabled, the Python 3 bindings will be installed" ON "Python_FOUND" OFF)

add_subdirectory(src)

if(PYTHON_BINDINGS)
    add_subdirectory(python)
endif()
