# Build TensorFlow Text from source

## Note ⚠️

Please use `Xcode 14.3` and `Apple clang version 14.0.3 (clang-1403.0.22.14.1)`.

## Prerequisites

It is assumed here that you have the necessary Unix-Like knowledge, [`brew`](https://brew.sh) and [`conda`](https://github.com/conda-forge/miniforge) have been installed in your terminal, and the installation and use methods of `brew` and `conda` will not be repeated here; most importantly, this tutorial is completely based on Apple Silicon build, so make sure your Mac is Apple Silicon.

## Step by Step

1. Create a new Env and install the dependencies provided by Apple.

   ```shell
   conda create -n tensorflow-macos python=3.11 # Python 3.8, 3.9 and 3.10 are also supported.
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

   * Usually, the `bazel` installed by `brew` will be the latest version. The latest version often does not match the version required by `text`, which may cause many unexpected problems, so we install it by specifying the version manually.

4. Download and extract `text 2.13.0`.

   ```shell
   wget https://github.com/tensorflow/text/archive/refs/tags/v2.13.0.zip
   unzip ./v2.13.0.zip
   cd text-2.13.0
   ````

5. Modify some parameters of the source code to ensure correct build.

   * `oss_scripts/configure.sh` to modify line 49:

     ```shell
     pip install tensorflow-macos==2.13.0
     ````
   
   * (Optional, if you have not installed `bazel` through `brew`, please skip it) `oss_scripts/run_build.sh` modify line 18:
   
       ```shell
       tf_bazel_version='5.3.0-homebrew'
       ```
   
6. Run the script.

   ```shell
   ./oss_scripts/run_build.sh
   ````

7. [Please do not forget to install the whl file.](https://github.com/sun1638650145/Libraries-and-Extensions-for-TensorFlow-for-Apple-Silicon/issues/2)

   ```shell
   pip install ./*.whl
   ```

## Tips&Refer

1. `Text` needs to correspond to a minor version of `tensorflow` (e.g. `tensorflow-macos==2.7.0` and `tensorflow-text==2.7.3`)
2. Please ensure that your network is stable during the compilation process, and the compilation needs to use the network.
3. [I add a PR for Apple Silicon support.](https://github.com/tensorflow/text/pull/756)
