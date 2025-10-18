file(GLOB UTIL_SRC "${CMAKE_CURRENT_SOURCE_DIR}/*.cpp")
if(UTIL_SRC)
  add_library(util STATIC ${UTIL_SRC})
  target_include_directories(util PRIVATE ${CMAKE_SOURCE_DIR}/source)

endif()
pkg_check_modules(SIGCPP REQUIRED sigc++-2.0)
include_directories(${SIGCPP_INCLUDE_DIRS})