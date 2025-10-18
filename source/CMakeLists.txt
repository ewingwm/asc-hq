cmake_minimum_required(VERSION 3.10)
project(asc-source)

# Export include path to the top-level source dir
include_directories(${CMAKE_SOURCE_DIR} ${CMAKE_SOURCE_DIR}/source)

pkg_check_modules(SIGCPP REQUIRED sigc++-2.0)
include_directories(${SIGCPP_INCLUDE_DIRS})
find_package(Freetype REQUIRED)
include_directories(${FREETYPE_INCLUDE_DIRS})
# Collect some common libraries and source folders as static libraries
add_subdirectory(libs)
add_subdirectory(unix)
add_subdirectory(tools)

# Allow building documentation if doxygen exists
find_package(Doxygen QUIET)
if(DOXYGEN_FOUND)
  add_custom_target(doc ALL COMMAND ${DOXYGEN_EXECUTABLE} ${CMAKE_SOURCE_DIR}/source/doxygen.config WORKING_DIRECTORY ${CMAKE_SOURCE_DIR}/source)
endif()
