# Compiling FANMODPlus with Local Boost on Linux

This guide details the steps to compile the FANMODPlus project on a Linux system. It includes instructions for downloading, compiling, and installing Boost locally within the project directory, ensuring no `sudo` privileges are needed for Boost management.

## 1. Prerequisites

Before you begin, ensure you have the following system-wide packages installed. You might need `sudo` for this initial setup if they are not already present (You most likely have these prerequisites already installed).

*   **Build Essentials:** A C++ compiler (like g++), make, etc.
*   **CMake:** Version 3.22 or newer. Check with `cmake --version`.
*   **Git:** For cloning the repository.
*   **Boost Build Dependencies:** `wget` (or `curl`), `tar`, `python3`, `libbz2-dev`, `zlib1g-dev`.

```bash
sudo apt update
sudo apt install build-essential cmake git wget tar python3 libbz2-dev zlib1g-dev
```

> _First, check your CMake version with `cmake --version`. If it's older than 3.22, you may need to install a newer version manually from cmake.org or via pip: `pip install --user cmake`._


## 2. Project Setup


1. Navigate to the directory where you want to set up the project.


2. If you don't have the `FANMODPlus` source code, clone it (adjust URL if necessary):
```bash
git clone https://github.com/zaritskylab/FANMODPlus.git
cd FANMODPlus
```
The following steps for downloading and building Boost should be performed from within this `FANMODPlus` project directory.


## 3. Downloading and Build Boost Locally

We will download Boost `1.81.0` and build it specifically for this project.

1. **Download Boost 1.81.0:**
```bash
wget https://archives.boost.io/release/1.81.0/source/boost_1_81_0.tar.gz
```

2. **Extract Boost:**

```bash
tar xzf boost_1_81_0.tar.gz
```
3. **Configure and Install Boost:** Navigate into the extracted Boost directory. We will configure it to install into an `install` subdirectory within the `boost_1_81_0` directory (i.e., `FANMODPlus/boost_1_81_0/install`) and only build the `program_options` library.


```bash
cd boost_1_81_0

# Configure Boost.
./bootstrap.sh --prefix=$(pwd)/install --with-libraries=program_options

# Build and install Boost (this can take some time)
# -j$(nproc) uses all available CPU cores for faster compilation
./b2 install -j$(nproc)

cd ..
```

## 4. Configure and Build FANMODPlus
The `CMakeLists.txt` in the `FANMODPlus` root directory is set up to find Boost. We'll now configure the project build to use the Boost library we just compiled locally.

1. **Create a build directory and navigate into it:** It's good practice to build outside the source directory.

```bash
mkdir build
cd build
```

2. **Run CMake to configure the project:** We explicitly tell CMake where to find our local Boost installation. The path ../boost_1_81_0/install is relative to the build directory.

```bash
cmake .. -DCMAKE_BUILD_TYPE=Release
```
You should see output indicating that Boost 1.81.0 was found in your `boost_1_81_0/install` directory.

3. **Compile the project:**

```bash
cmake --build . --config Release -- -j$(nproc)
```

The `LocalFANMOD` executable will typically be created in the `build` directory (e.g., `FANMODPlus/build/LocalFANMOD`).
