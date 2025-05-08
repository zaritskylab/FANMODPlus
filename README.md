# FANMOD+
FANMOD+ is a tool that is based on the original FANMOD (Fast Network Motif Detection) tool - a software tool used for identifying and analyzing network motifs in complex networks. Network motifs are small, recurring patterns or subgraphs that occur more frequently in real-world networks than would be expected in random networks. They are important for understanding the structural and functional building blocks of complex systems, such as biological, social, or technological networks. (Sebastian Wernicke, Florian Rasche, FANMOD: a tool for fast network motif detection, Bioinformatics, Volume 22, Issue 9, May 2006, Pages 1152–1153, https://doi.org/10.1093/bioinformatics/btl038)

FANMOD+ is similar (fork from the fanmod repository) in functionality to FANMOD but extends to support: 
(1) command line execution with cross-platform build process. 
(2) more node colors up to 128 and larger motifs - supporting the analysis of modern spatial multiplex single cell omics data.
(3) output in LEDA format to support loading the subgraphs in python using networkx lib (https://www.algorithmic-solutions.info/leda_guide/graphs/leda_native_graph_fileformat.html)

# Why not use the original FANMOD tool
If you need to extract motifs in a graph with a high number of node colors (more than 15), the original FANMOD tool implementation will reach its limit.

# The core change
FANMOD+ increases the space (i.e., number of bits) for the canonical representation from 64-bit fixed allocation to 128-bit dynamic allocation (i.e., physical memory allocation extended when used) (Schäling, 2011). 

The fanmod output, input and options are covering only the use case of CISM method (Zamir et al. 2025).

# Changes
1. Removing GUI wx dependencies
2. Adding CMAKE compilation files
3. Changed the in-memory data structure that stores the graph canonical labeling of a single motif/graph from 64-bit fixed allocation to 512-bit dynamic allocation (i.e., physical memory allocation extended when used, Schäling B. The boost C++ libraries, 2nd ed. Laguna Hills: XML Press; 2014.).


# Build & Run
## Prerequisites

Before building FANMOD+, ensure you have the following installed:

*   A C++ compiler (e.g., g++) with C++17 support
*   CMake (version 3.22 or later)
*   Git (for cloning the repository)

# Linux
```bash
mkdir build && cd build
cmake ..
make
```

For more detailed instructions, especially for building with a local Boost library, please see the [Linux Compilation Guide](Compile.md)

# Linux using Docker
```bash
docker build -t fanmod_plus_build/fanmod_plus_build:0.1 .
docker run -it --rm --name=fanmod_plus --mount type=bind,source=${PWD},target="/src" fanmod_plus_build/fanmod_plus_build:0.1 bash
```

inside the container -
```bash
    cd src
    mkdir build && cd build
    cmake ..
    make 
```
> _The binary should be created under the build folder (LocalFANMOD)_

**Note:** The minimum required version of glibc is 2.27. You can check your glibc version with `ldd --version`.


# Windows

Compile with visual studio
https://learn.microsoft.com/en-us/cpp/build/cmake-projects-in-visual-studio?view=msvc-170

# Flags
```bash
    -i <path to input graph file>

    Format of the input file should be:
    <left node id> <right node id> <left node color> <right node color>

    -o <path to output file>

    -r <number of null graphs iterations>

    -s <size of motif>

    For instance, 3 is 3-nodes.

    -colored_vertcies <colored vertices>
```
# Output file format
The initial lines of the file contains statistical information

For each motif the output contains information on each candidate subgraph:

    ID,Frequency,Count,Mean-Freq,Standard-Dev,Z-Score,p-Value

    ID: the canonical unique representation of the subgraph
    Frequency: the number of subgraph instances / total number of subgraphs
    Count: the number of subgraph instances
    Mean-Freq: the mean frequencies of the null graphs
    Standard-Dev: the std frequencies of the null graphs
    Z-Score: (Frequency-Mean-Freq)/Standard-Dev
    p-value: how many times the number of instances is higher than in the null model, divided by the the number of iterations.

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
