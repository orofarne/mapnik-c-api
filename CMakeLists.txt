cmake_minimum_required(VERSION 2.8)

project(mapnik-c-api CXX)

set(CMAKE_MODULE_PATH "${CMAKE_SOURCE_DIR}/cmake")

OPTION(TESTS "enable_testing" ON)
OPTION(STATICLIB "build_static_library" OFF)

# Boost
find_package(Boost 1.48.0 REQUIRED)
include_directories(${Boost_INCLUDE_DIRS})

# ICU
find_package(ICU REQUIRED)
include_directories(${ICU_INCLUDE_DIRS})

# Cairo
find_package(Cairo REQUIRED)
include_directories(${CAIRO_INCLUDE_DIRS})

# Setup mapnik
find_package( Mapnik REQUIRED)
include_directories(${MAPNIK_INCLUDE_DIRS})
set(CMAKE_CXX_FLAGS ${MAPNIK_CXXFLAGS})
set(CMAKE_MODULE_LINKER_FLAGS ${MAPNIK_LDFLAGS})
set(CMAKE_SHARED_LINKER_FLAGS ${MAPNIK_LDFLAGS})

# Build
# Dynamic library
add_library(mapnik_c SHARED mapnik_c_api.cpp)
target_link_libraries(mapnik_c ${MAPNIK_LIBRARIES})

# Static library
if(STATICLIB)
  add_library(mapnik_c_static STATIC mapnik_c_api.cpp)
  target_link_libraries(mapnik_c_static ${MAPNIK_LIBRARIES})
endif(STATICLIB)

if(TESTS)
  add_subdirectory(test)
endif(TESTS)
