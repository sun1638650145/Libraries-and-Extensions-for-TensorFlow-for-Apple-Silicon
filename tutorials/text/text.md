# Build TensorFlow Text from source

## Necessary conditions

It is assumed here that you have the necessary Unix-Like knowledge, [`brew`](https://brew.sh) and [`conda`](https://github.com/conda-forge/miniforge) have been installed in your terminal, and the installation and use methods of `brew` and `conda` will not be repeated here; most importantly, this tutorial is completely based on Apple Silicon (such as M1, M1 Pro, M1 Max and M1 Ultra) build, so make sure your Mac is Apple Silicon.

## Step by Step

1. Create a new Env and install the dependencies provided by Apple.

   ```shell
   conda create -n tensorflow-macos python=3.10 # This Python version can also use 3.8 and 3.9
   conda activate tensorflow-macos
   conda install -c apple tensorflow-deps==2.9.0
   ````

2. Install the `tensorflow-macos` and `tensorflow-metal` plugins.

   ```shell
   pip install tensorflow-macos==2.9.0
   pip install tensorflow-metal==0.5.0
   ````

3. Install `bazel 5.1.1`.

   ```shell
   wget https://raw.githubusercontent.com/Homebrew/homebrew-core/2940e900476b4452c8047c75dcbfc193c6f30341/Formula/bazel.rb
   brew install ./bazel.rb
   bazel --version # Make sure the version is 5.1.x.
   ````

   * Usually, the `bazel` installed by `brew` will be the latest version. The latest version often does not match the version required by `text`, which may cause many unexpected problems, so we install it by specifying the version manually.

4. Download and extract `text 2.9.0`.

   ```shell
   wget https://github.com/tensorflow/text/archive/refs/tags/v2.9.0.zip
   unzip ./v2.9.0.zip
   cd text-2.9.0
   ````

5. Modify some parameters of the source code to ensure correct build.

   * `oss_scripts/configure.sh` to modify line 49:

     ```shell
     pip install tensorflow-macos==2.9.0
     ````

6. Run the script.

   ```shell
   ./oss_scripts/run_build.sh
   ````

7. [Please do not forget to install the whl file.](https://github.com/sun1638650145/Libraries-and-Extensions-for-TensorFlow-for-Apple-Silicon/issues/2)

   ```python
   pip install ./*.whl
   ```

## Tips&Refer

1. `Text` needs to correspond to a minor version of `tensorflow` (eg `tensorflow-macos==2.7.0` and `tensorflow-text==2.7.3`)
2. Please ensure that your network is stable during the compilation process, and the compilation needs to use the network.
3. [I add a PR for Apple Silicon support.](https://github.com/tensorflow/text/pull/756)
