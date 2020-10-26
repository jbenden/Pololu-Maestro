cmake_minimum_required(VERSION 3.11.4)
project(Maestro)

set(CMAKE_EXPORT_COMPILE_COMMANDS ON)

if(WIN32 OR APPLE)
    add_subdirectory(libusb)
else()
    include(FindPkgConfig)
    pkg_search_module(LIBUSB REQUIRED libusb-1.0)
    include_directories(${LIBUSB_INCLUDE_DIRS})
endif()

add_subdirectory(libusc)
add_subdirectory(examples)

