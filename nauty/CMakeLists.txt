# CMakeList.txt : CMake project for LocalQuateXelero, include source and define
# project specific logic here.
#
cmake_minimum_required (VERSION 3.8)

# Add source to this project's executable.
add_library(nauty "nauty.c" "naututil.c" "naugraph.c" "schreier.c" "naurng.c" "nausparse.c" "nautil.c" "nautinv.c")

# TODO: Add tests and install targets if needed.