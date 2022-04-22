# Build TensorFlow Text from source

## Necessary conditions

It is assumed here that you have the necessary Unix-Like knowledge, [`brew`](https://brew.sh) and [`conda`](https://github.com/conda-forge/miniforge) have been installed in your terminal, and the installation and use methods of `brew` and `conda` will not be repeated here; most importantly, this tutorial is completely based on Apple Silicon (such as M1, M1 Pro or M1 Max) build, so make sure your Mac is Apple Silicon.

## Step by Step

1. Create a new Env and install the dependencies provided by Apple.

   ```shell
   conda create -n tensorflow-macos python=3.9 # This Python version can also use 3.8
   conda activate tensorflow-macos
   conda install -c apple tensorflow-deps==2.8.0
   ````

2. Install the `tensorflow-macos` and `tensorflow-metal` plugins.

   ```shell
   pip install tensorflow-macos
   pip install tensorflow-metal
   ````

3. Install `bazel 4.2.2`.

   ```shell
   wget https://raw.githubusercontent.com/Homebrew/homebrew-core/210bef2d570f0635e07009f4c5242b5e6645ae31/Formula/bazel.rb
   brew install ./bazel.rb
   bazel --version # make sure the version is 4.2.2
   ````

   * Usually, the `bazel` installed by `brew` will be the latest version. The latest version often does not match the version required by `text`, which may cause many unexpected problems, so we install it by specifying the version manually.

4. Download and extract `text 2.8.2`.

   ```shell
   wget https://github.com/tensorflow/text/archive/refs/tags/v2.8.2.zip
   unzip ./v2.8.2.zip
   cd text-2.8.2
   ````

5. Modify some parameters of the source code to ensure correct build.

   * `oss_scripts/configure.sh` to modify line 49:

     ```shell
     pip install tensorflow-macos==2.8.0
     ````

   * `oss_scripts/run_build.sh` comment out line 17:

     ```shell
     # source oss_scripts/prepare_tf_dep.sh
     ````

6. Run the script.

   ```shell
   ./oss_scripts/run_build.sh
   ````

## Tips&Refer

1. `Text` needs to correspond to a minor version of `tensorflow` (eg `tensorflow-macos==2.7.0` and `tensorflow-text==2.7.3`)
2. Please ensure that your network is stable during the compilation process, and the compilation needs to use the network.
3. [I add a PR for Apple Silicon support.](https://github.com/tensorflow/text/pull/756)
