set( CMAKE_CXX_COMPILER "C:/mingw64/bin/g++.exe" )
cmake_minimum_required(VERSION 3.0)
project(RaylibNineYearsGameJam CXX) # TODO rename this later when a game title is chosen

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

# Setting parameters for raylib
set(BUILD_EXAMPLES OFF CACHE BOOL "" FORCE) # don't build the supplied examples

if (EMSCRIPTEN)
	# Raylib's CMakeLists.txt doesn't check if (EMSCRIPTEN) for some reason
	set(PLATFORM "Web" CACHE BOOL "" FORCE)
endif ()

add_subdirectory(lib/raylib)

add_executable(RaylibNineYearsGameJam
	main.cpp
)

if (EMSCRIPTEN)
	target_link_options(RaylibNineYearsGameJam PRIVATE -sUSE_GLFW=3 -sWASM=1 -sTOTAL_MEMORY=64MB)
	# No spaces between the arguments, because
	# otherwise CMake molests them and removes the -s between the latter two
	# The linker sees "-s USE_GLFW=3 WASM=1 TOTAL_MEMORY=64MB" otherwise
	# Maybe there's a better solution
	# see this page: https://cmake.org/cmake/help/latest/command/target_link_options.html#option-de-duplication
	set(CMAKE_EXECUTABLE_SUFFIX ".html")
	target_link_options(RaylibNineYearsGameJam PRIVATE --preload-file ${CMAKE_CURRENT_SOURCE_DIR}/assets@/assets)
endif ()

target_link_libraries(RaylibNineYearsGameJam PRIVATE raylib)
