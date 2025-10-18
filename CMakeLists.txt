cmake_minimum_required(VERSION 3.10)
project(asc-hq LANGUAGES C CXX)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

option(BUILD_TOOLS "Build command-line tools" ON)
option(BUILD_ASC "Build asc main executable" ON)

message(STATUS "Configuring asc-hq with CMake")

# Make the source tree and generated build dir available for includes
include_directories(${CMAKE_SOURCE_DIR} ${CMAKE_SOURCE_DIR}/source ${CMAKE_BINARY_DIR})

# Configure config.h from config.h.in (replaces autoconf-generated config.h)
option(GAME_DATADIR "Data dir for game files" "${CMAKE_INSTALL_PREFIX}/share/asc")
configure_file(${CMAKE_SOURCE_DIR}/config.h.in ${CMAKE_BINARY_DIR}/config.h @ONLY)

# Prefer SDL1 (legacy) if available; fall back to sdl12-compat, then SDL2 with shim.
find_package(PkgConfig REQUIRED)
pkg_check_modules(SDL1_PKG QUIET sdl)
pkg_check_modules(SDL12COMPAT_PKG QUIET sdl12-compat)
pkg_check_modules(SDL2_PKG QUIET sdl2)

if(SDL1_PKG_FOUND)
  message(STATUS "Found SDL (1.x) via pkg-config: ${SDL1_PKG_INCLUDE_DIRS}")
  include_directories(BEFORE ${SDL1_PKG_INCLUDE_DIRS})
  set(ASC_SDL_INCLUDE_DIR ${SDL1_PKG_INCLUDE_DIRS})
  set(ASC_SDL_LIBRARIES ${SDL1_PKG_LIBRARIES})
  set(ASC_USE_SDL1 TRUE)
elseif(SDL12COMPAT_PKG_FOUND)
  # sdl12-compat provides the SDL 1.2 API on top of SDL2
  message(STATUS "Found sdl12-compat (SDL1 API on SDL2): ${SDL12COMPAT_PKG_INCLUDE_DIRS}")
  include_directories(BEFORE ${SDL12COMPAT_PKG_INCLUDE_DIRS})
  set(ASC_SDL_INCLUDE_DIR ${SDL12COMPAT_PKG_INCLUDE_DIRS})
  set(ASC_SDL_LIBRARIES ${SDL12COMPAT_PKG_LIBRARIES})
  set(ASC_USE_SDL12COMPAT TRUE)
elseif(SDL2_PKG_FOUND)
  message(STATUS "Found SDL2 via pkg-config: ${SDL2_PKG_INCLUDE_DIRS}")
  # Create a small compatibility shim so existing '#include <SDL.h>' keeps working
  set(SDL_COMPAT_DIR ${CMAKE_BINARY_DIR}/sdl_compat)
  file(MAKE_DIRECTORY ${SDL_COMPAT_DIR})
  file(WRITE ${SDL_COMPAT_DIR}/SDL.h "#pragma once\n#include <SDL2/SDL.h>\n")
  include_directories(BEFORE ${SDL_COMPAT_DIR} ${SDL2_PKG_INCLUDE_DIRS})
  set(ASC_SDL_INCLUDE_DIR ${SDL2_PKG_INCLUDE_DIRS})
  set(ASC_SDL_LIBRARIES ${SDL2_PKG_LIBRARIES})
  add_definitions(-DUSE_SDL2)
  set(ASC_USE_SDL2 TRUE)
else()
  message(WARNING "No SDL found. Install SDL1 (libsdl1.2-dev), sdl12-compat, or SDL2 dev packages to build audio/video features.")
endif()

# Add main source directory (this will add more subdirectories)
add_subdirectory(source)

if(EXISTS ${CMAKE_SOURCE_DIR}/data/CMakeLists.txt)
  add_subdirectory(data)
endif()

if(EXISTS ${CMAKE_SOURCE_DIR}/installer/CMakeLists.txt)
  add_subdirectory(installer)
endif()
 # Replaces 'MyDependency' with the actual package name  
pkg_check_modules(SIGCPP REQUIRED sigc++-2.0)
include_directories(${SIGCPP_INCLUDE_DIRS})
message(STATUS "CMake configuration finished. Use 'cmake -S . -B build' then 'cmake --build build' to build.")
