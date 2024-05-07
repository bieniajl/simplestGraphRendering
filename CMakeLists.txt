# OBJECT library requires 2.8.8
cmake_minimum_required(VERSION 2.8.8 FATAL_ERROR)
project(simplestGraphRendering CXX C)

find_package(PkgConfig REQUIRED)
pkg_search_module(GLFW REQUIRED glfw3)
include_directories(${GLFW_INCLUDE_DIRS})

find_package(OpenGL REQUIRED)
include_directories(${OPENGL_INCLUDE_DIRS})
find_package(GLEW REQUIRED)
include_directories(${GLEW_INCLUDE_DIRS})

enable_testing()

include(CMakeDetermineCXXCompiler)

# default to RelWithDebInfo
if("${CMAKE_BUILD_TYPE}" STREQUAL "")
	set(CMAKE_BUILD_TYPE RelWithDebInfo CACHE STRING "Choose the type of build, options are: None(CMAKE_CXX_FLAGS or CMAKE_C_FLAGS used) Debug Release RelWithDebInfo MinSizeRel." FORCE)
endif()

if("${CMAKE_CXX_COMPILER_ID}" STREQUAL "Clang" OR "${CMAKE_CXX_COMPILER_ID}" STREQUAL "GNU")
	set(EXTRA_CXX_FLAGS "-Wall -Wextra -Wno-unused-parameter -pedantic" CACHE STRING "Extra flags used by the compiler during all build types.")
	set(EXTRA_EXE_LINKER_FLAGS "-Wl,--as-needed" CACHE STRING "Extra flags used by the linker.")
	set(EXTRA_EXE_LINKER_FLAGS_RELEASE "-flto" CACHE STRING "Extra flags used by the linker for the Release build type.")
	set(EXTRA_EXE_LINKER_FLAGS_RELWITHDEBINFO "-flto" CACHE STRING "Extra flags used by the linker for the RelWithDebInfo build type.")
else()
	set(EXTRA_CXX_FLAGS "" CACHE STRING "Extra flags used by the compiler during all build types.")
	set(EXTRA_EXE_LINKER_FLAGS "" CACHE STRING "Extra flags used by the linker.")
endif()

set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} ${EXTRA_CXX_FLAGS} -std=c++20")
set(CMAKE_EXE_LINKER_FLAGS "${CMAKE_EXE_LINKER_FLAGS} ${EXTRA_EXE_LINKER_FLAGS}")
set(CMAKE_EXE_LINKER_FLAGS_RELEASE "${CMAKE_EXE_LINKER_FLAGS_RELEASE} ${EXTRA_EXE_LINKER_FLAGS_RELEASE}")
set(CMAKE_EXE_LINKER_FLAGS_RELWITHDEBINFO "${CMAKE_EXE_LINKER_FLAGS_RELWITHDEBINFO} ${EXTRA_EXE_LINKER_FLAGS_RELWITHDEBINFO}")

option(VERBOSE "Verbose logging" OFF)

if(NOT VERBOSE)
	add_definitions(-DNVERBOSE)
endif()

add_executable(simple
	  src/simplestGraphRendering.cpp
)

include(FileEmbed.cmake)
FileEmbedSetup()
FileEmbedAdd(src/debug_f.glsl)
FileEmbedAdd(src/debug_v.glsl)
FileEmbedAdd(src/edge_f.glsl)
FileEmbedAdd(src/edge_v.glsl)
FileEmbedAdd(src/polygon_f.glsl)
FileEmbedAdd(src/polygon_v.glsl)
FileEmbedAdd(src/textLabel_f.glsl)
FileEmbedAdd(src/textLabel_v.glsl)

target_link_libraries(simple file_embed ${GLFW_LIBRARIES} ${OPENGL_LIBRARIES} ${GLEW_LIBRARIES})
