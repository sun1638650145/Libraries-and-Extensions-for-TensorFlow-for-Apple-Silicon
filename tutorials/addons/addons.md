# Build TensorFlow Addons from source

## Necessary conditions

It is assumed here that you have the necessary Unix-Like knowledge, [`brew`](https://brew.sh) and [`conda`](https://github.com/conda-forge/miniforge) have been installed in your terminal, and the installation and use methods of `brew` and `conda` will not be repeated here; most importantly, this tutorial is completely based on Apple Silicon (such as M1, M1 Pro, M1 Max or M1 Ultra) build, so make sure your Mac is Apple Silicon.

## Step by Step

1. Create a new Env and install the dependencies provided by Apple.

   ```shell
   conda create -n tensorflow-macos python=3.10 # This Python version can also use 3.8 and 3.9
   conda activate tensorflow-macos
   conda install -c apple tensorflow-deps==2.9.0
   ````

2. Install the `tensorflow-macos` and `tensorflow-metal` plugins.

   ```shell
   pip install tensorflow-macos==2.9.2
   pip install tensorflow-metal==0.5.0
   ````

3. Install `bazel 4.2.x`.

   ```shell
   wget https://raw.githubusercontent.com/Homebrew/homebrew-core/210bef2d570f0635e07009f4c5242b5e6645ae31/Formula/bazel.rb
   brew install ./bazel.rb
   bazel --version # Make sure the version is 4.2.x.
   ````

   * Usually, the `bazel` installed by `brew` will be the latest version. The latest version often does not match the version required by `text`, which may cause many unexpected problems, so we install it by specifying the version manually.

4. Download and extract `addons 0.17.1`.

   ```shell
   wget https://github.com/tensorflow/addons/archive/refs/tags/v0.17.1.zip
   unzip ./v0.17.1.zip
   cd addons-0.17.1
   ````

5. Run the script.

   ```shell
   python3 ./configure.py
   bazel build build_pip_pkg
   bazel-bin/build_pip_pkg artifacts
   ````

6. Please do not forget to install the `whl` file.

   ```python
   pip install artifacts/*.whl
   ```

## Tips&Refer

1. `Addons` needs the version of `tensorflow`. The specific correspondence is [here](https://github.com/tensorflow/addons/blob/a5cd76d341c594f464a5c9be8e572ed5bd3f3b8b/README.md?plain=1#L80).
2. Please ensure that your network is stable during the compilation process, and the compilation needs to use the network.
2. [I add Python 3.10 support for r0.17 of Apple silicon.](https://github.com/tensorflow/addons/pull/2718)
