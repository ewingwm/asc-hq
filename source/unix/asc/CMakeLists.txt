cmake_minimum_required(VERSION 3.10)
project(asc-unix-asc)

include_directories(${CMAKE_SOURCE_DIR} ${CMAKE_SOURCE_DIR}/source ${CMAKE_SOURCE_DIR}/source/libs/paragui/include)

set(ASC_COMMON_SOURCES
  ${CMAKE_SOURCE_DIR}/source/soundList.cpp
  ${CMAKE_SOURCE_DIR}/source/typen.cpp
  ${CMAKE_SOURCE_DIR}/source/applicationstarter.cpp
  ${CMAKE_SOURCE_DIR}/source/strtmesg.cpp
  ${CMAKE_SOURCE_DIR}/source/stack.cpp
  ${CMAKE_SOURCE_DIR}/source/spfst.cpp
  ${CMAKE_SOURCE_DIR}/source/sgstream.cpp
  ${CMAKE_SOURCE_DIR}/source/palette.cpp
  ${CMAKE_SOURCE_DIR}/source/newfont.cpp
  ${CMAKE_SOURCE_DIR}/source/misc.cpp
  ${CMAKE_SOURCE_DIR}/source/loadpcxc.cpp
  ${CMAKE_SOURCE_DIR}/source/loaders.cpp
  ${CMAKE_SOURCE_DIR}/source/tos.cpp
  ${CMAKE_SOURCE_DIR}/source/loadbi3.cpp
  ${CMAKE_SOURCE_DIR}/source/gamedlg.cpp
  ${CMAKE_SOURCE_DIR}/source/dlg_box.cpp
  ${CMAKE_SOURCE_DIR}/source/dialog.cpp
  ${CMAKE_SOURCE_DIR}/source/controls.cpp
  ${CMAKE_SOURCE_DIR}/source/basestrm.cpp
  ${CMAKE_SOURCE_DIR}/source/basegfx.cpp
  ${CMAKE_SOURCE_DIR}/source/attack.cpp
  ${CMAKE_SOURCE_DIR}/source/replaymapdisplay.cpp
  ${CMAKE_SOURCE_DIR}/source/memory-measurement.cpp
  ${CMAKE_SOURCE_DIR}/source/gameoptions.cpp
  ${CMAKE_SOURCE_DIR}/source/astar2.cpp
  ${CMAKE_SOURCE_DIR}/source/vehicletype.cpp
  ${CMAKE_SOURCE_DIR}/source/objects.cpp
  ${CMAKE_SOURCE_DIR}/source/stdio-errorhandler.cpp
  ${CMAKE_SOURCE_DIR}/source/buildingtype.cpp
  ${CMAKE_SOURCE_DIR}/source/containerbase.cpp
  ${CMAKE_SOURCE_DIR}/source/mapalgorithms.cpp
  ${CMAKE_SOURCE_DIR}/source/viewcalculation.cpp
  ${CMAKE_SOURCE_DIR}/source/gamemap.cpp
  ${CMAKE_SOURCE_DIR}/source/password.cpp
  ${CMAKE_SOURCE_DIR}/source/replay.cpp
  ${CMAKE_SOURCE_DIR}/source/research.cpp
  ${CMAKE_SOURCE_DIR}/source/fieldimageloader.cpp
  ${CMAKE_SOURCE_DIR}/source/mapdisplay.cpp
  ${CMAKE_SOURCE_DIR}/source/resourcenet.cpp
  ${CMAKE_SOURCE_DIR}/source/ascstring.cpp
  ${CMAKE_SOURCE_DIR}/source/graphicset.cpp
  ${CMAKE_SOURCE_DIR}/source/vehicle.cpp
  ${CMAKE_SOURCE_DIR}/source/buildings.cpp
  ${CMAKE_SOURCE_DIR}/source/networkinterface.cpp
  ${CMAKE_SOURCE_DIR}/source/textfileparser.cpp
  ${CMAKE_SOURCE_DIR}/source/terraintype.cpp
  ${CMAKE_SOURCE_DIR}/source/objecttype.cpp
  ${CMAKE_SOURCE_DIR}/source/textfiletags.cpp
  ${CMAKE_SOURCE_DIR}/source/itemrepository.cpp
  ${CMAKE_SOURCE_DIR}/source/stringtokenizer.cpp
  ${CMAKE_SOURCE_DIR}/source/music.cpp
  ${CMAKE_SOURCE_DIR}/source/messages.cpp
  ${CMAKE_SOURCE_DIR}/source/paradialog.cpp
  ${CMAKE_SOURCE_DIR}/source/textfile_evaluation.cpp
  ${CMAKE_SOURCE_DIR}/source/containerbasetype.cpp
  ${CMAKE_SOURCE_DIR}/source/messagedlg.cpp
  ${CMAKE_SOURCE_DIR}/source/simplestream.cpp
  ${CMAKE_SOURCE_DIR}/source/mappolygons.cpp
  ${CMAKE_SOURCE_DIR}/source/prehistoricevents.cpp
  ${CMAKE_SOURCE_DIR}/source/gameevents.cpp
  ${CMAKE_SOURCE_DIR}/source/gameeventsystem.cpp
  ${CMAKE_SOURCE_DIR}/source/gameevent_dialogs.cpp
  ${CMAKE_SOURCE_DIR}/source/statisticdialog.cpp
  ${CMAKE_SOURCE_DIR}/source/clipboard.cpp
  ${CMAKE_SOURCE_DIR}/source/geometry.cpp
  ${CMAKE_SOURCE_DIR}/source/guidimension.cpp
  ${CMAKE_SOURCE_DIR}/source/guiiconhandler.cpp
  ${CMAKE_SOURCE_DIR}/source/guifunctions.cpp
  ${CMAKE_SOURCE_DIR}/source/iconrepository.cpp
  ${CMAKE_SOURCE_DIR}/source/overviewmapimage.cpp
  ${CMAKE_SOURCE_DIR}/source/mapfield.cpp
  ${CMAKE_SOURCE_DIR}/source/overviewmappanel.cpp
  ${CMAKE_SOURCE_DIR}/source/dashboard.cpp
  ${CMAKE_SOURCE_DIR}/source/asc-mainscreen.cpp
  ${CMAKE_SOURCE_DIR}/source/explosivemines.cpp
  ${CMAKE_SOURCE_DIR}/source/mainscreenwidget.cpp
  ${CMAKE_SOURCE_DIR}/source/networksupervisor.cpp
  ${CMAKE_SOURCE_DIR}/source/statistics.cpp
  ${CMAKE_SOURCE_DIR}/source/legacynetworkloaders.cpp
  ${CMAKE_SOURCE_DIR}/source/player.cpp
  ${CMAKE_SOURCE_DIR}/source/cannedmessages.cpp
  ${CMAKE_SOURCE_DIR}/source/reactionfire.cpp
  ${CMAKE_SOURCE_DIR}/source/mapimageexport.cpp
  ${CMAKE_SOURCE_DIR}/source/containercontrols.cpp
  ${CMAKE_SOURCE_DIR}/source/containerbase-functions.cpp
  ${CMAKE_SOURCE_DIR}/source/windowing.cpp
  ${CMAKE_SOURCE_DIR}/source/turncontrol.cpp
  ${CMAKE_SOURCE_DIR}/source/unitset.cpp
  ${CMAKE_SOURCE_DIR}/source/playercolor.cpp
  ${CMAKE_SOURCE_DIR}/source/pg_mapdisplay.cpp
  ${CMAKE_SOURCE_DIR}/source/autotraining.cpp
  ${CMAKE_SOURCE_DIR}/source/researchexecution.cpp
  ${CMAKE_SOURCE_DIR}/source/luacommandwriter.cpp
  ${CMAKE_SOURCE_DIR}/source/campaignactionrecorder.cpp
  ${CMAKE_SOURCE_DIR}/source/loggingoutput.cpp
  ${CMAKE_SOURCE_DIR}/source/versionidentifier.cpp
  ${CMAKE_SOURCE_DIR}/source/package.cpp
  ${CMAKE_SOURCE_DIR}/source/packagemanager.cpp
  ${CMAKE_SOURCE_DIR}/source/packagerepository.cpp
  ${CMAKE_SOURCE_DIR}/source/dataversioncheck.cpp
  ${CMAKE_SOURCE_DIR}/source/i18n.cpp
  ${CMAKE_SOURCE_DIR}/source/accessconstraints-game.cpp
  ${CMAKE_SOURCE_DIR}/source/contextutils.cpp
  ${CMAKE_SOURCE_DIR}/source/unitcostcalculator-standard.cpp
  ${CMAKE_SOURCE_DIR}/source/weaponrangelayer.cpp
)

  find_package(Boost COMPONENTS filesystem system QUIET)
  if(NOT Boost_FOUND)
    message(STATUS "Boost not found by find_package; attempting fallback detection in /usr/include and /usr/lib")
    # Try to find the Boost header in common include locations (handles multi-arch dirs)
    find_path(BOOST_HEADER_PATH NAMES boost/scoped_array.hpp
              PATHS /usr/include /usr/local/include /usr/include/x86_64-linux-gnu
                    /usr/include/* NO_DEFAULT_PATH)
    if(BOOST_HEADER_PATH)
      message(STATUS "Found Boost header at: ${BOOST_HEADER_PATH}")
      set(Boost_INCLUDE_DIRS ${BOOST_HEADER_PATH})
      # Try locate libs
      find_library(BOOST_FILESYSTEM_LIB NAMES boost_filesystem boost_filesystem-mt
                   HINTS /usr/lib /usr/lib64 /usr/lib/x86_64-linux-gnu)
      find_library(BOOST_SYSTEM_LIB NAMES boost_system boost_system-mt
                   HINTS /usr/lib /usr/lib64 /usr/lib/x86_64-linux-gnu)
      if(BOOST_FILESYSTEM_LIB AND BOOST_SYSTEM_LIB)
        set(Boost_LIBRARIES "${BOOST_FILESYSTEM_LIB};${BOOST_SYSTEM_LIB}")
        set(Boost_FOUND TRUE)
        message(STATUS "Found Boost libs: ${Boost_LIBRARIES}")
      else()
        message(WARNING "Found Boost headers at ${BOOST_HEADER_PATH} but couldn't locate boost libraries under standard lib paths")
      endif()
    else()
      message(STATUS "Fallback: Boost header boost/scoped_array.hpp not found in common include paths")
    endif()
  endif()
  find_package(SDL2 QUIET)

add_library(asc_common STATIC ${ASC_COMMON_SOURCES})
target_compile_features(asc_common PUBLIC cxx_std_17)
target_compile_definitions(asc_common PRIVATE _UNIX_)

# Build the main asc executable
add_executable(asc ${CMAKE_SOURCE_DIR}/source/sg.cpp ${CMAKE_SOURCE_DIR}/source/gamedialog.cpp)
target_link_libraries(asc PRIVATE asc_common)

# Include system and bundled library include dirs
if(Boost_FOUND)
  target_include_directories(asc_common PRIVATE ${Boost_INCLUDE_DIRS})
  target_link_libraries(asc_common PRIVATE ${Boost_LIBRARIES})
else()
  message(WARNING "Boost not found; some optional features may be disabled. Install Boost or set BOOST_ROOT to pick it up.")
endif()

if(SDL2_FOUND)
  target_include_directories(asc_common PRIVATE ${SDL2_INCLUDE_DIRS})
  target_link_libraries(asc_common PRIVATE ${SDL2_LIBRARIES})
else()
  message(WARNING "SDL2 not found by CMake; you may need to install SDL2 or adjust includes")
endif()

# loki is shipped in source/libs/loki-0.1.6
target_include_directories(asc_common PRIVATE ${CMAKE_SOURCE_DIR}/source/libs/loki-0.1.6/include)
# Build unittester (noinst in autotools)
add_executable(unittester ${ASC_COMMON_SOURCES})
target_link_libraries(unittester PRIVATE asc_common)

# Link asc_common against other internal libraries if they were configured
set(ASC_INTERNAL_LIBS)
foreach(lib ai util graphics dialogs widgets actions tasks lua video unittests sdl_lib paragui SDLmm triangul)
  if(TARGET ${lib})
    list(APPEND ASC_INTERNAL_LIBS ${lib})
  endif()
endforeach()

if(ASC_INTERNAL_LIBS)
  message(STATUS "Linking asc_common with internal libs: ${ASC_INTERNAL_LIBS}")
  target_link_libraries(asc_common PRIVATE ${ASC_INTERNAL_LIBS})
endif()

message(STATUS "Added asc and unittester targets in source/unix/asc")
