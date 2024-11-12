# fandmod+
Fork of the fanmod tool that supports command line execution with cross-platform build process.

# Changes
1. Removing GUI wx dependencies
2. Adding CMAKE compilation files
3. Changed the in-memory data structure that stores the graph canonical labeling of a single motif/graph from 64-bit fixed allocation to 512-bit dynamic allocation (i.e., physical memory allocation extended when used, Schäling B. The boost C++ libraries, 2nd ed. Laguna Hills: XML Press; 2014.).


# Build & Run

# Linux
cmake .

cmake --build .

# Linux using Docker

    docker build -t fanmod_plus_build/fanmod_plus_build:0.1 .

    docker run -it --rm --name=fanmod_plus --mount type=bind,source=${PWD},target="/src" fanmod_plus_build/fanmod_plus_build:0.1 bash

inside the container -

    cd src
    mkdir build && cd build
    cmake ..
    make

---> The binary should be created under the build folder (LocalFANMOD)
Please note that the minmum required version of glibc is 2.27.
The glibc version could checked with "ldd --version" command.


# Windows
https://learn.microsoft.com/en-us/cpp/build/cmake-projects-in-visual-studio?view=msvc-170

# Credit(s)

* The original algorithm was developed by S. Wernicke and F. Rasche
    * See `doc/resources/*.pdf` for more information.
    * The original source of that implementation was: http://theinf1.informatik.uni-jena.de/motifs/

* Command line output modifications by Sebastian Bücker
    * See [this link](https://github.com/gabbage/fanmod-cmd);
