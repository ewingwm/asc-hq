cmake_minimum_required(VERSION 3.10)
project(asc-data)

file(GLOB DATA_FILES "${CMAKE_CURRENT_SOURCE_DIR}/*")

add_custom_target(data_files SOURCES ${DATA_FILES})
