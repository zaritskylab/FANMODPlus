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

# Windows
https://learn.microsoft.com/en-us/cpp/build/cmake-projects-in-visual-studio?view=msvc-170


# Credit(s)

* The original algorithm was developed by S. Wernicke and F. Rasche
    * See `doc/resources/*.pdf` for more information.
    * The original source of that implementation was: http://theinf1.informatik.uni-jena.de/motifs/

* Command line output modifications by Sebastian Bücker
    * See [this link](https://github.com/gabbage/fanmod-cmd);