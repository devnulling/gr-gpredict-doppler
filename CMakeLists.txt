cmake_minimum_required(VERSION 2.6)

project(gr-gpredict-doppler)


# Set build type
if(NOT CMAKE_BUILD_TYPE)
  set(CMAKE_BUILD_TYPE "Release")
  message(STATUS "Build type not specified: defaulting to release.")
endif(NOT CMAKE_BUILD_TYPE)
set(CMAKE_BUILD_TYPE ${CMAKE_BUILD_TYPE} CACHE STRING "")


# Ensure Modules are first on path
list(INSERT CMAKE_MODULE_PATH 0 ${CMAKE_SOURCE_DIR}/cmake/Modules)


# GNU Radio config
include(GrPlatform)

set(GR_RUNTIME_DIR bin)
set(GR_LIBRARY_DIR lib${LIB_SUFFIX})
set(GR_INCLUDE_DIR include/gpredict)
set(GR_DATA_DIR share)
set(GR_PKG_DATA_DIR ${GR_DATA_DIR}/${CMAKE_PROJECT_NAME})
set(GR_DOC_DIR ${GR_DATA_DIR}/doc)
set(GR_PKG_DOC_DIR ${GR_DOC_DIR}/${CMAKE_PROJECT_NAME})
set(GR_CONF_DIR etc)
set(GR_PKG_CONF_DIR ${GR_CONF_DIR}/${CMAKE_PROJECT_NAME}/conf.d)
set(GR_LIBEXEC_DIR libexec)
set(GR_PKG_LIBEXEC_DIR ${GR_LIBEXEC_DIR}/${CMAKE_PROJECT_NAME})
set(GRC_BLOCKS_DIR ${GR_PKG_DATA_DIR}/grc/blocks)

set(GR_REQUIRED_COMPONENTS RUNTIME)

########################################################################
# On Apple only, set install name and use rpath correctly, if not already set
########################################################################
if(APPLE)
   if(NOT CMAKE_INSTALL_NAME_DIR)
       set(CMAKE_INSTALL_NAME_DIR
           ${CMAKE_INSTALL_PREFIX}/${GR_LIBRARY_DIR} CACHE
           PATH "Library Install Name Destination Directory" FORCE)
   endif(NOT CMAKE_INSTALL_NAME_DIR)
   if(NOT CMAKE_INSTALL_RPATH)
       set(CMAKE_INSTALL_RPATH
           ${CMAKE_INSTALL_PREFIX}/${GR_LIBRARY_DIR} CACHE
           PATH "Library Install RPath" FORCE)
   endif(NOT CMAKE_INSTALL_RPATH)
   if(NOT CMAKE_BUILD_WITH_INSTALL_RPATH)
       set(CMAKE_BUILD_WITH_INSTALL_RPATH ON CACHE
           BOOL "Do Build Using Library Install RPath" FORCE)
   endif(NOT CMAKE_BUILD_WITH_INSTALL_RPATH)
endif(APPLE)

find_package(Gnuradio "3.7.7" REQUIRED)


# Uninstall target
configure_file(
  ${CMAKE_SOURCE_DIR}/cmake/cmake_uninstall.cmake.in
  ${CMAKE_CURRENT_BINARY_DIR}/cmake_uninstall.cmake
  @ONLY
)
add_custom_target(
  uninstall ${CMAKE_COMMAND} -P ${CMAKE_CURRENT_BINARY_DIR}/cmake_uninstall.cmake
)


# Add subdirectories
add_subdirectory(python)
add_subdirectory(grc)
