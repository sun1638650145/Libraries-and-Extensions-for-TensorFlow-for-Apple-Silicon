# Build TensorFlow Text from source

## Note ⚠️

Please use `Xcode 14.3` and `Apple clang version 14.0.3 (clang-1403.0.22.14.1)` or later versions is required.

## Prerequisites

It is assumed here that you have the necessary Unix-Like knowledge, [`brew`](https://brew.sh) and [`conda`](https://github.com/conda-forge/miniforge) have been installed in your terminal, and the installation and use methods of `brew` and `conda` will not be repeated here; most importantly, this tutorial is completely based on Apple silicon build, so make sure your Mac is Apple silicon.

## Step by Step

1. Create a new Env and install the dependencies provided by Apple.

   ```shell
   conda create -n tensorflow-macos python=3.12 # Python 3.9, 3.10 and 3.11 are also supported.
   conda activate tensorflow-macos
   ````
   
2. Install the `tensorflow`.

   ```shell
   pip install tensorflow==2.16.1
   ````

3. Install `bazel 6.5.0`.

   ```shell
   wget https://github.com/bazelbuild/bazel/releases/download/6.5.0/bazel-6.5.0-darwin-arm64 -O bazel
   chmod +x bazel
   sudo mv bazel /usr/local/bin/
   bazel --version # Make sure the version is 6.5.0.
   ````

4. Download and extract `text 2.16.1`.

   ```shell
   wget https://github.com/tensorflow/text/archive/refs/tags/v2.16.1.zip
   unzip ./v2.16.1.zip
   cd text-2.16.1
   ````

5. Modify some parameters of the source code to ensure correct build.

   * `oss_scripts/configure.sh` to modify line 49:

     ```shell
     pip install tensorflow==2.16.1
     ````
   
   * `oss_scripts/pip_package/setup.py` to modify line 75:
   
     ```python
     install_requires=[
         (
             'tensorflow>=2.16.1, <2.17',
         ),
     ],
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
