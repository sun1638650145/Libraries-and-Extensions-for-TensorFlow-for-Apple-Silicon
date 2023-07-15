# Build TensorFlow Addons from source

## Note ⚠️

`TensorFlow Addons` already provides Apple Silicon precompiled `whl` files for all Python versions, which you can download directly from this [page](https://pypi.org/project/tensorflow-addons/).

## Prerequisites

It is assumed here that you have the necessary Unix-Like knowledge, [`brew`](https://brew.sh) and [`conda`](https://github.com/conda-forge/miniforge) have been installed in your terminal, and the installation and use methods of `brew` and `conda` will not be repeated here; most importantly, this tutorial is completely based on Apple Silicon build, so make sure your Mac is Apple Silicon.

## Step by Step

1. Create a new Env and install the dependencies provided by Apple.

   ```shell
   conda create -n tensorflow-macos python=3.11 # Python 3.8, 3.9 and 3.10 are also supported here.
   conda activate tensorflow-macos
   ````
   
2. Install the `tensorflow` and `tensorflow-metal` plugins.

   ```shell
   pip install tensorflow==2.13.0 # Starting from tensorflow 2.13, official support for Apple silicon is available.
   pip install tensorflow-metal==1.0.1
   ````

3. Install `bazel 5.3.0`.

   ```shell
   wget https://raw.githubusercontent.com/Homebrew/homebrew-core/59dff37de3c670a77c1e9f39b8a4b0f8884a391b/Formula/bazel.rb
   brew install ./bazel.rb
   bazel --version # Make sure the version is 5.3.0.
   ````

   * Normally, the version of `bazel` installed via `brew` is the latest one, which may not match the version required by the `addons`. This can lead to unexpected issues, so we need to manually specify the version for installation.

4. Download and extract `addons 0.21.0`.

   ```shell
   wget https://github.com/tensorflow/addons/archive/refs/tags/v0.21.0.zip
   unzip ./v0.21.0.zip
   cd addons-0.21.0
   ````

5. Run the script.

   ```shell
   python3 ./configure.py
   bazel build build_pip_pkg
   bazel-bin/build_pip_pkg artifacts
   ````

6. Please do not forget to install the `whl` file.

   ```shell
   pip install artifacts/*.whl
   ```

## Tips&Refer

1. `Addons` need to correspond with the version of `tensorflow`. The specific correspondence is [here](https://github.com/tensorflow/addons/blob/a5cd76d341c594f464a5c9be8e572ed5bd3f3b8b/README.md?plain=1#L80).
2. Please ensure that your network is stable during the compilation process, and the compilation needs to use the network.
3. [I add Python 3.10 support for r0.17 of Apple Silicon.](https://github.com/tensorflow/addons/pull/2718)
