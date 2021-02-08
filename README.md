# ARM64 Mac Native Deep Learning Set Up
How to set up a Deep Learning environment for the new ARM64 - M1 equipped Macs

## Part 1: MiniForge

There are some things that I assume you have already done/know how to do at this point. I assume your terminal is running natively, and you know some basic BASH. Currently, MiniConda/Anaconda doesn't offer an Apple Silicon version so we need to uninstall it. Enable view of hidden folders with `defaults write com.apple.finder AppleShowAllFiles true; killall Finder`, then navigate to `.anaconda` and delete it. I installed mine via homebrew so it might be there, or your home folder. Check your terminal and see if it says `(base)` upon startup(it shouldn't now),and you should not be able to activate any of your environments. If so, then you successfully deleted it. Next, you need to navigate to https://github.com/conda-forge/miniforge and download the Miniforge3-MacOSX-arm64 version. Navigate to the file in your Downloads using the Terminal and run `bash Miniforge3-MacOSX-arm64.sh`. Follow the installation prompt and check your terminal that you have `(base)`, then proceed to the next step. 

## Part 2: Tensorflow

1. Create a conda environment with python 3.8 `conda create -n m1_tensorflow python=3.8`
2. Download the latest zipped tar file from apple/tensorflow_macos: https://github.com/apple/tensorflow_macos/releases. This install is for version 0.1a1, so if you are installing a newer version at the time of following this guide, adjust that in the code in step 6. XV.
3. Unzip the tar file, I used the native Mac unzip tool. Put it in your home directory, or wherever you prefer. 
4. Get the working directory of the 'arm64' folder within that folder, once there, type: `pwd` in the terminal, and copy the output, you will neeed it for the next step. 
5. Activate the m1_tensorflow environment and set the following variables
    1. `libs=(location of arm64 above)` (I used `libs=“/Users/oresttokovenko/tensorflow_macos/arm64/`)
    2. `env=(location of your python virtual environment)` (I used `env=“/Users/oresttokovenko/miniforge3/envs/m1_tensorflow`)
6. Run the following commands in the m1_tensorflow conda environment. I spaced out the conda installs so that if there is an error, it will be easier to determine where that is. 
    1. `conda install cached-property`
    2. `conda install six`
    3. `pip install --upgrade -t "$env/lib/python3.8/site-packages/" --no-dependencies --force "$libs/grpcio-1.33.2-cp38-cp38-macosx_11_0_arm64.whl"`
    4. `pip install --upgrade -t "$env/lib/python3.8/site-packages/" --no-dependencies --force "$libs/h5py-2.10.0-cp38-cp38-macosx_11_0_arm64.whl"`
    5. `pip install --upgrade -t "$env/lib/python3.8/site-packages/" --no-dependencies --force "$libs/tensorflow_addons-0.11.2+mlcompute-cp38-cp38-macosx_11_0_arm64.whl"`
    6. `conda install -c conda-forge -y absl-py`
    7. `conda install -c conda-forge -y astunparse`
    8. `conda install -c conda-forge -y gast`
    9. `conda install -c conda-forge -y opt_einsum`
    10. `conda install -c conda-forge -y termcolor`
    11. `conda install -c conda-forge -y typing_extensions`
    12. `conda install -c conda-forge -y wheel`
    13. `conda install -c conda-forge -y typeguard`
    14. `pip install wrapt flatbuffers tensorflow_estimator google_pasta keras_preprocessing protobuf`
    15. `pip install --upgrade -t “$env/lib/python3.8/site-packages/” --no-dependencies --force “$libs/tensorflow_macos-0.1a1-cp38-cp38-macosx_11_0_arm64.whl”`
    16. `pip install tensorboard`

7. open Jupyter notebook or lab and run `import tensorflow as tf` and check if successful. If you have issues refer to https://github.com/apple/tensorflow_macos/issues/48#issuecomment-766152769 for extra help

## Part 3: PyTorch

1. Create a conda environment with python 3.8 `conda create -n m1_pytorch python=3.8`
2. run `conda install pytorch -c isuruf/label/pytorch -c conda-forge`
3. run `conda install typing-extensions`
4. open Jupyter notebook or lab and run `import torch` and check if successful. If you have issues refer to https://github.com/pytorch/pytorch/issues/48145 for extra help
5. Extra Step: Install PyTorch Lightning using `pip install pytorch-lightning`
6. Extra Step: Install Torchvision using `conda install torchvision -c pytorch`

## What doesn't yet work as of 01/23/2020 with MiniForge for ARM64 macOS

- scikit-learn surprise
- xgboost
