cmake_minimum_required(VERSION 2.6)

project(Cork)

list(APPEND CMAKE_MODULE_PATH "${PROJECT_SOURCE_DIR}/cmake")

include(FindGMP)

set(PROJECT_LINK_LIBRARIES "")

if(GMP_FOUND)
  set(PROJECT_LINK_LIBRARIES ${PROJECT_LINK_LIBRARIES} ${GMP_LIBRARIES})
  include_directories(${GMP_INCLUDES})
endif()

if(${CMAKE_CXX_COMPILER_ID} MATCHES "Clang" OR ${CMAKE_CXX_COMPILER_ID} MATCHES "GNU")
  set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11")
  set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -Wall")
  set(CMAKE_CXX_FLAGS_DEBUG "-O0 -g")
  set(CMAKE_CXX_FLAGS_RELEASE "-O3 -DNDEBUG")
endif()

# NO_TIMER;REDUCED;CDT_ONLY;TRILIBRARY;ANSI_DECLARATORS;
add_definitions(-DNO_TIMER -DREDUCED -DCDT_ONLY -DTRILIBRARY -DANSI_DECLARATORS)

include_directories("${PROJECT_SOURCE_DIR}/src/accel")
include_directories("${PROJECT_SOURCE_DIR}/src/file_formats")
include_directories("${PROJECT_SOURCE_DIR}/src/isct")
include_directories("${PROJECT_SOURCE_DIR}/src/math")
include_directories("${PROJECT_SOURCE_DIR}/src/mesh")
include_directories("${PROJECT_SOURCE_DIR}/src/rawmesh")
include_directories("${PROJECT_SOURCE_DIR}/src/util")

# Source list
file(STRINGS sourcelist PROJECT_SOURCE_FILES)

add_executable(cork ${PROJECT_SOURCE_FILES} "src/main.cpp")
target_link_libraries(cork ${PROJECT_LINK_LIBRARIES})

add_executable(off2obj ${PROJECT_SOURCE_FILES} "src/off2obj.cpp")
target_link_libraries(off2obj ${PROJECT_LINK_LIBRARIES})

install(TARGETS cork DESTINATION "bin")
install(TARGETS off2obj DESTINATION "bin")
