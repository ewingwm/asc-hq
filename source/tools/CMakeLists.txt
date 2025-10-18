cmake_minimum_required(VERSION 3.10)
project(asc-tools)

include_directories(${CMAKE_SOURCE_DIR} ${CMAKE_SOURCE_DIR}/source)

file(GLOB TOOL_SRCS "${CMAKE_CURRENT_SOURCE_DIR}/*.cpp")

foreach(src ${TOOL_SRCS})
  get_filename_component(toolname ${src} NAME_WE)
  add_executable(${toolname} ${src})
  target_compile_features(${toolname} PRIVATE cxx_std_17)
endforeach()

message(STATUS "Added tools executables for ${CMAKE_CURRENT_SOURCE_DIR}")
