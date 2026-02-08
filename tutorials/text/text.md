# Build TensorFlow Text from source

## Prerequisites

It is assumed here that you have the necessary Unix-Like knowledge, [`brew`](https://brew.sh) and [`conda`](https://github.com/conda-forge/miniforge) have been installed in your terminal, and the installation and use methods of `brew` and `conda` will not be repeated here; most importantly, this tutorial is completely based on Apple silicon build, so make sure your Mac is Apple silicon.

## Step by Step

1. Create a new Env and install the dependencies provided by Apple.

   ```shell
   conda create -n tensorflow-macos python=3.13 # Python 3.9, 3.10, 3.11 and 3.12 are also supported.
   conda activate tensorflow-macos
   ````
   
2. Install the `tensorflow`.

   ```shell
   pip install tensorflow==2.20.0
   ````

3. Install `bazel 7.4.1`.

   ```shell
   wget https://github.com/bazelbuild/bazel/releases/download/7.4.1/bazel-7.4.1-darwin-arm64 -O bazel
   chmod +x bazel
   sudo mv bazel /usr/local/bin/
   bazel --version # Make sure the version is 7.4.1.
   ````

4. Download and extract `text 2.20.0`.

   ```shell
   wget https://github.com/tensorflow/text/archive/refs/tags/v2.20.0.zip
   unzip ./v2.20.0.zip
   cd text-2.20.0
   ````

5. Run the script.

   ```shell
   ./oss_scripts/run_build.sh
   ````
   
6. [Please do not forget to install the whl file.](https://github.com/sun1638650145/Libraries-and-Extensions-for-TensorFlow-for-Apple-Silicon/issues/2)

   ```shell
   pip install ./*.whl
   ```

## Tips&Refer

1. `Text` needs to correspond to a minor version of `tensorflow` (e.g. `tensorflow-macos==2.7.0` and `tensorflow-text==2.7.3`)
2. Please ensure that your network is stable during the compilation process, and the compilation needs to use the network.
3. [I add a PR for Apple Silicon support.](https://github.com/tensorflow/text/pull/756)
