# fandmod+
Fork of the fanmod tool that supports command line execution with cross-platform build process. 
FANMOD+ increases the space (i.e., number of bits) for the canonical representation from 64-bit fixed allocation to 128-bit dynamic allocation (i.e., physical memory allocation extended when used) (Schäling, 2011). 
FANMOD+ subgraph canonical representation enables the encoding of more colors and larger motifs, supporting the analysis of modern spatial multiplex single cell omics data.

The fanmod output, input and options are covering only the use case of CISM method (Zamir et al. 2025).

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
Compile with visual studio
https://learn.microsoft.com/en-us/cpp/build/cmake-projects-in-visual-studio?view=msvc-170

# Flags

-i <path to input graph file>

Format of the input file should be:
<left node id> <right node id> <left node color> <right node color>

-o <path to output file>

-r <number of null graphs iterations>

-s <size of motif>

For instance, 3 is 3-nodes.

--colored_vertcies <colored vertices>

# Output file format
The initial lines of the file contains statistical information

After that for each motif the out constains information on each candidate subgraph:
    ID,Frequency,Count,Mean-Freq,Standard-Dev,Z-Score,p-Value

    ID: the canonical unique represenation of the subgraph
    Frequency: the number of subgraph instances / total number of subgraphs
    Count: the number of subgraph instances
    Mean-Freq: the mean frequencies of the null graphs
    Standard-Dev: the std frequencies of the null graphs
    Z-Score: (Frequency-Mean-Freq)/Standard-Dev
    p-value: how many times the number of instances was higher than in the null model, divides by the the number of itereations.

    -------------------------------------
    adjacency matrix with color encoding (not encoded well with more than 9 colors)

    leda encoding of the subgraph https://www.algorithmic-solutions.info/leda_guide/graphs/leda_native_graph_fileformat.html
    #+leda format
    #-leda format



# Credit(s)

* The original algorithm was developed by S. Wernicke and F. Rasche
    * See `doc/resources/*.pdf` for more information.
    * The original source of that implementation was: http://theinf1.informatik.uni-jena.de/motifs/

* Command line output modifications by Sebastian Bücker
    * See [this link](https://github.com/gabbage/fanmod-cmd);
