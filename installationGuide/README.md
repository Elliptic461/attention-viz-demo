# OpenFold and Attention Visualization Demo Setup

This document provides instructions for setting up OpenFold and the Attention Visualization Demo in your HPC environment.

## Prerequisites

- Conda package manager 
- Access to the AlphaFold data directory (typically at `/storage/ice1/shared/d-pace_community/alphafold/alphafold_2.3.2_data` for Georgia Tech PACE ICE, and `/anvil/datasets/alphafold/db` for Purdue Anvil)
- Sufficient disk space, estimated at least 50GB of free disk space

## Installation

1. Open the `install.ipynb` notebook in Jupyter, or other compatible IDE
2. Click on the button labeled kernel (Top right)
3. Select "Jupyter kernel", then "openfold_env" kernel. If it doesn't appear, click the refresh button. 
4. For the second line of the first cell ("os.environ['ROOT_DIR'] = '~/scratch'"). Change the directory to where you want to store your Openfold and Attention Visualization Demo. 
5. Click "Run All" to execute the installation process. This will switch to a conda environment, install all dependences, and install OpenFold and the Attention Visualization Demo in your environment.

## Notes

- Ensure you have sufficient disk space in your root and conda install directories
- The installation process may take some time depending on your internet connection and system resources
- Make sure the AlphaFold data directory is accessible and contains the required model parameters
