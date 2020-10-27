cmake_minimum_required(VERSION 3.11.4)

project(usc)

set(LIBUSC_VERSION "1.0.0")
set(LIBUSC_LIB_NAME libusc)
if (CMAKE_BUILD_TYPE STREQUAL "Debug")
	set(LIBUSC_LIB_NAME ${LIBUSC_LIB_NAME}${FMT_DEBUG_POSTFIX})
endif ()
set(LIBUSC_LIB_DIR ${CMAKE_INSTALL_LIBDIR})
set(LIBUSC_INC_DIR ${CMAKE_INSTALL_INCLUDEDIR})
set(LIBUSC_DEBUG_POSTFIX d CACHE STRING "Debug library postfix.")
set(pkgconfig ${PROJECT_BINARY_DIR}/libusc-1.pc)

include(GNUInstallDirs)

add_library(usc SHARED libusc.h libusc.c)
target_link_libraries(usc PRIVATE usb-1.0)
target_include_directories(usc PUBLIC .)
set_target_properties(usc PROPERTIES FOLDER "libusc"
	DEBUG_POSTFIX "${LIBUSC_DEBUG_POSTFIX}")

install(TARGETS usc DESTINATION "${LIBUSC_LIB_DIR}")

set(libdir_for_pc_file "\${exec_prefix}/${LIBUSC_LIB_DIR}")
set(includedir_for_pc_file "\${prefix}/${LIBUSC_INC_DIR}")
configure_file(
    "${PROJECT_SOURCE_DIR}/libusc-1.pc.in"
    "${pkgconfig}"
@ONLY)
install(FILES "${pkgconfig}" DESTINATION "${CMAKE_INSTALL_LIBDIR}/pkgconfig")
