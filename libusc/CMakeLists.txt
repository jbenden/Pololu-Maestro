cmake_minimum_required(VERSION 3.11.4)

project(usc)

add_library(usc SHARED libusc.h libusc.c)
target_link_libraries(usc PRIVATE usb-1.0)
target_include_directories(usc PUBLIC .)
set_target_properties(usc PROPERTIES FOLDER "libusc")

install(TARGETS usc DESTINATION lib)

