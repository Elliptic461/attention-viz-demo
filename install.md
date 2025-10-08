# OpenFold and Attention Visualization Demo Installation Guide

This guide provides instructions for setting up OpenFold and the Attention Visualization Demo, including all necessary environment configurations and dependencies.

## Prerequisites

- Linux-based operating system
- NVIDIA GPU with CUDA support (required for OpenFold)
- Conda package manager (Miniconda or Anaconda)
- Git

## Installation Steps

1. **Initialize Conda** (if not already done):
   ```bash
   conda init
   ```
   Restart your terminal after running this command.

2. **Clone the Attention Visualization Demo repository**:
   ```bash
   git clone https://github.com/vizfold/attention-viz-demo.git ~/scratch/ attention-viz-demo
   ```
   
3. **Clone the OpenFold repository**:
   ```bash
   git clone https://github.com/aqlaboratory/openfold.git ~/scratch/openfold
   ```

4. **Create and activate the OpenFold conda environment**:
   ```bash
   cd ~/scratch/openfold
   conda env create -n openfold_env -f environment.yml
   conda activate openfold_env
   
   # Install additional required packages
   conda install -y ipykernel
   python -m ipykernel install --user --name=openfold_env
   ```

4. **Set up compiler and library paths**:
   ```bash
   # Create necessary directories and symlinks
   mkdir -p $CONDA_PREFIX/x86_64-conda-linux-gnu/lib
   ln -sf $CONDA_PREFIX/libexec/gcc/x86_64-conda-linux-gnu/12.4.0/cc1plus $CONDA_PREFIX/bin/
   ln -sf $CONDA_PREFIX/lib/gcc/x86_64-conda-linux-gnu/12.4.0/crtbeginS.o $CONDA_PREFIX/x86_64-conda-linux-gnu/lib/
   ln -sf $CONDA_PREFIX/lib/gcc/x86_64-conda-linux-gnu/12.4.0/crtendS.o $CONDA_PREFIX/x86_64-conda-linux-gnu/lib/
   ln -sf $CONDA_PREFIX/x86_64-conda-linux-gnu/sysroot/usr/lib64/crti.o $CONDA_PREFIX/x86_64-conda-linux-gnu/lib/
   ln -sf $CONDA_PREFIX/x86_64-conda-linux-gnu/sysroot/usr/lib64/crtn.o $CONDA_PREFIX/x86_64-conda-linux-gnu/lib/
   
   # Install additional compiler tools
   conda install -y gcc_linux-64 libgcc-ng
   
   # Set environment variables for compilation
   export GCC_LTO_PLUGIN="$CONDA_PREFIX/libexec/gcc/x86_64-conda-linux-gnu/12.4.0/liblto_plugin.so"
   export CFLAGS="-O2 -fno-lto --sysroot=$CONDA_PREFIX/x86_64-conda-linux-gnu/sysroot"
   export CXXFLAGS="$CXXFLAGS -fno-use-linker-plugin -O2 -fno-lto --sysroot=$CONDA_PREFIX/x86_64-conda-linux-gnu/sysroot"
   export LDFLAGS="$LDFLAGS -L$CONDA_PREFIX/lib/gcc/x86_64-conda-linux-gnu/12.4.0 -L$CONDA_PREFIX/x86_64-conda-linux-gnu/sysroot/usr/lib64"
   export CPATH="$CONDA_PREFIX/include:$CPATH"
   export LIBRARY_PATH="$CONDA_PREFIX/lib:$LIBRARY_PATH"
   export LD_LIBRARY_PATH="$CONDA_PREFIX/lib:$LD_LIBRARY_PATH"
   ```

5. **Install OpenFold and dependencies**:
   ```bash
   # Install OpenFold in development mode
   pip install -e .
   
   # Install third-party dependencies
   scripts/install_third_party_dependencies.sh
   ```

6. **Set up Attention Visualization Demo**:
   ```bash
   cd ~/scratch/attention-viz-demo
   
   # Create necessary directories and symlinks
   mkdir -p openfold/resources
   ln -sf ~/scratch/openfold/openfold/data ./openfold/
   ln -sf /storage/ice1/shared/d-pace_community/alphafold/alphafold_2.3.2_data/params ./openfold/resources/
   
   # Download required chemical properties file
   wget -N --no-check-certificate -P openfold/resources/ \
     https://git.scicore.unibas.ch/schwede/openstructure/-/raw/7102c63615b64735c4941278d92b554ec94415f8/modules/mol/alg/src/stereo_chemical_props.txt
   ```

7. **Install additional visualization tools**:
   ```bash
   # Set strict channel priority for consistent package resolution
   conda config --set channel_priority strict
   
   # Install PyTorch with CUDA support
   conda install -y pytorch==2.5.0 pytorch-cuda=12.4 -c pytorch -c nvidia
   
   # Install PyMOL for molecular visualization
   conda install -y -c conda-forge -c pytorch -c nvidia pymol-open-source
   
   # Install matplotlib
   conda install -y conda-forge::matplotlib
   
   # Reset channel priority
   conda config --remove-key channel_priority
   ```