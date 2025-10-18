cmake_minimum_required(VERSION 3.10)
project(asc-libs)

if(EXISTS ${CMAKE_CURRENT_SOURCE_DIR}/triangul/CMakeLists.txt)
  add_subdirectory(triangul)
endif()
if(EXISTS ${CMAKE_CURRENT_SOURCE_DIR}/sdlmm/CMakeLists.txt)
  add_subdirectory(sdlmm)
endif()
if(EXISTS ${CMAKE_CURRENT_SOURCE_DIR}/paragui/CMakeLists.txt)
  add_subdirectory(paragui)
endif()

message(STATUS "Configured source/libs (subdirs added where CMakeLists.txt present)")
