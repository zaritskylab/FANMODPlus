# CMakeList.txt : Top-level CMake project file, do global configuration
# and include sub-projects here.
#
cmake_minimum_required (VERSION 3.22)	

if (CMAKE_CXX_COMPILER_ID MATCHES "MSVC")
	string(REPLACE "/O2" "/Od" CMAKE_CXX_FLAGS_RELEASE "${CMAKE_CXX_FLAGS_RELEASE}")
endif()

project ("LocalFANMOD")

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED True)

add_compile_definitions(_UNICODE)
add_compile_definitions(_SILENCE_STDEXT_HASH_DEPRECATION_WARNINGS _CRT_SECURE_NO_WARNINGS)
add_compile_definitions(_WIN64)
add_compile_definitions(WORDSIZE=64 HAVE_HWLZCNT NAUTY_CPU_DEFINED)

#set(Boost_DEBUG ON)
set(Boost_USE_STATIC_LIBS   ON)
set(BOOST_ROOT "${CMAKE_CURRENT_SOURCE_DIR}/boost_1_81_0/install")

find_package(Boost 1.81.0 REQUIRED COMPONENTS program_options)
if(Boost_FOUND)
  include_directories(${Boost_INCLUDE_DIRS})
endif()

# Include sub-projects.
if (CMAKE_CXX_COMPILER_ID MATCHES "MSVC")
	include_directories(w_include)
	add_compile_options("/favor:AMD64")
	add_compile_options("/arch:AVX2")
	set(CMAKE_EXPORT_COMPILE_COMMANDS ON)
endif()

add_subdirectory (include)
include_directories(include)

add_subdirectory (helpers)
add_subdirectory (nauty)

# Add source to this project's executable.
add_executable (${PROJECT_NAME} "maingraph.cpp" "graph64.cpp" "output.cpp" "random.cpp" "main.cpp")

target_link_libraries(${PROJECT_NAME} nauty getopt Boost::program_options)
