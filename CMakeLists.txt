cmake_minimum_required(VERSION 2.8)
project(fensterchen)

if(MSVC)
  set(BINARY_DIRECTORY build)
endif()

if(("${CMAKE_CXX_COMPILER_ID}" STREQUAL "Clang") OR ("${CMAKE_CXX_COMPILER_ID}" STREQUAL "GNU") OR (("${CMAKE_CXX_COMPILER_ID}" STREQUAL "Intel") AND UNIX))
  set(CMAKE_XCODE_ATTRIBUTE_CLANG_CXX_LANGUAGE_STANDARD "c++0x")
  set(CMAKE_XCODE_ATTRIBUTE_CLANG_CXX_LIBRARY "libc++")
  set(CMAKE_CXX_FLAGS "-std=c++0x")
endif()

add_definitions(-DFENSTERCHEN_SOURCE_DIR="${CMAKE_CURRENT_SOURCE_DIR}")
option(FENSTERCHEN_IN_SOURCE_BUILD "FENSTERCHEN_IN_SOURCE_BUILD" OFF)
if(FENSTERCHEN_IN_SOURCE_BUILD)
  add_definitions(-DFENSTERCHEN_BINARY_DIR="../")
else()
  add_definitions(-DFENSTERCHEN_BINARY_DIR="${CMAKE_CURRENT_BINARY_DIR}")
endif()

include_directories(${CMAKE_CURRENT_SOURCE_DIR}/external)
add_definitions(-DGLEW_STATIC)

set(GLFW_DIRECTORY glfw-3.0.3)
include_directories(${CMAKE_CURRENT_SOURCE_DIR}/external/${GLFW_DIRECTORY}/include)

set(GLFW_INSTALL OFF CACHE STRING "" FORCE)
set(GLFW_BUILD_EXAMPLES OFF CACHE STRING "" FORCE)
set(GLFW_BUILD_TESTS OFF CACHE STRING "" FORCE)
add_subdirectory(${CMAKE_CURRENT_SOURCE_DIR}/external/${GLFW_DIRECTORY})
include_directories(${CMAKE_CURRENT_SOURCE_DIR}/external/${GLFW_DIRECTORY}/include)

set(GLM_DIRECTORY glm-0.9.5.3)
include_directories(${CMAKE_CURRENT_SOURCE_DIR}/external/${GLM_DIRECTORY})

################################
# Add libraries to executables

set(LIBRARIES glfw ${GLFW_LIBRARIES})

set(CATCH_DIRECTORY catch-1.1)
include_directories(${CMAKE_CURRENT_SOURCE_DIR}/external/${CATCH_DIRECTORY})

################################
# Add output directory

if(MSVC)
  set(EXECUTABLE_OUTPUT_PATH ${CMAKE_CURRENT_BINARY_DIR}/build/)
  if(CMAKE_BUILD_TYPE STREQUAL "Debug")
    set(COPY_OUTPUT_PATH ${CMAKE_CURRENT_BINARY_DIR}/build/Debug/ )
  else(CMAKE_BUILD_TYPE STREQUAL "Debug")
    set(COPY_OUTPUT_PATH ${CMAKE_CURRENT_BINARY_DIR}/build/Release/ )
  endif(CMAKE_BUILD_TYPE STREQUAL "Debug")
else(MSVC)
  if(CMAKE_BUILD_TYPE STREQUAL "Debug")
    set(EXECUTABLE_OUTPUT_PATH ${CMAKE_CURRENT_BINARY_DIR}/build/Debug )
  else(CMAKE_BUILD_TYPE STREQUAL "Debug")
    set(EXECUTABLE_OUTPUT_PATH ${CMAKE_CURRENT_BINARY_DIR}/build/Release )
  endif(CMAKE_BUILD_TYPE STREQUAL "Debug")
endif(MSVC)

if(MSVC)
  add_definitions(-D_CRT_SECURE_NO_WARNINGS)
endif(MSVC)

include_directories(${CMAKE_CURRENT_SOURCE_DIR}/framework)
add_subdirectory(framework)

add_subdirectory(source)
