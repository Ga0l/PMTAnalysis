cmake_minimum_required(VERSION 3.0 FATAL_ERROR)
project(PMTAnalysis)

set(CMAKE_CXX_STANDARD 11)

# You need to tell CMake where to find the ROOT installation. This can be done in a number of ways:
#   - ROOT built with classic configure/make use the provided $ROOTSYS/etc/cmake/FindROOT.cmake
#   - ROOT built with CMake. Add in CMAKE_PREFIX_PATH the installation prefix for ROOT
list(APPEND CMAKE_PREFIX_PATH $ENV{ROOTSYS})

#---Locate the ROOT package and defines a number of variables (e.g. ROOT_INCLUDE_DIRS)
set(CMAKE_MODULE_PATH ${CMAKE_MODULE_PATH} $ENV{ROOTSYS}/etc/cmake)
find_package(ROOT REQUIRED COMPONENTS RIO Net)
#find_package(ROOT CONFIG REQUIRED)
if(NOT ROOT_FOUND)
  message(STATUS "ROOT package not found.")
  return()
endif()

#---Define useful ROOT functions and macros (e.g. ROOT_GENERATE_DICTIONARY)
#include(${ROOT_USE_FILE})

#----------------------------------------------------------------------------
# Locate sources and headers for this project
# ## NOT NEEDED FOR DICT
include_directories(${PROJECT_SOURCE_DIR}/include
                    ${ROOT_INCLUDE_DIRS})

#----------------------------------------------------------------------------
# Locate sources and headers for this project
# NB: headers are included so they will show up in IDEs
#
file(GLOB sources ${PROJECT_SOURCE_DIR}/src/*.cpp)
file(GLOB headers ${PROJECT_SOURCE_DIR}/include/*.h)

#----------------------------------------------------------------------------
# Add the executable
#
add_executable(PMTAnalysis main.cpp ${sources} ${headers})
target_link_libraries(PMTAnalysis ${ROOT_LIBRARIES})

#file( GLOB APP_SOURCES *.cpp )
#foreach( testsourcefile ${APP_SOURCES} )
#  # I used a simple string replace, to cut off .cpp.
#  string( REPLACE ".cpp" "" testname ${testsourcefile} )
#  add_executable( ${testname} ${testsourcefile} )
#  # Make sure YourLib is linked to each app
#  target_link_libraries( ${testname} ${ROOT_LIBRARIES} )
#endforeach( testsourcefile ${APP_SOURCES} )