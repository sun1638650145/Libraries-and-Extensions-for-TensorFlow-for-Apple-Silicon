# Build TensorFlow I/O from source

## Prerequisites

It is assumed here that you have the necessary Unix-Like knowledge, [`brew`](https://brew.sh) and [`conda`](https://github.com/conda-forge/miniforge) have been installed in your terminal, and the installation and use methods of `brew` and `conda` will not be repeated here; most importantly, this tutorial is completely based on Apple silicon build, so make sure your Mac is Apple silicon.

## Step by Step

1. Create a new Env and install the dependencies provided by Apple.

   ```shell
   conda create -n tensorflow-macos python=3.11 # Python 3.9 and 3.10 are also supported.
   conda activate tensorflow-macos
   ````

2. Install the `tensorflow` and `tensorflow-metal` plugins.

   ```shell
   pip install tensorflow==2.15.0
   pip install tensorflow-metal==1.1.0
   ````

3. Install `bazel 6.1.0`.

   ```shell
   wget https://raw.githubusercontent.com/Homebrew/homebrew-core/a9b3083e23806aebe61f7c39d393734a6949eaa5/Formula/bazel.rb
   brew install ./bazel.rb
   bazel --version # Make sure the version is 6.1.0.
   ````

   * Normally, the version of `bazel` installed via `brew` is the latest one, which may not match the version required by the `io`. This can lead to unexpected issues, so we need to manually specify the version for installation.

4. Download and extract `io 0.36.0`.

   ```shell
   wget https://github.com/tensorflow/io/archive/refs/tags/v0.36.0.zip
   unzip ./v0.36.0.zip
   cd io-0.36.0
   ````

5. Set the environment variable `TF_PYTHON_VERSION`.

   ```shell
   export TF_PYTHON_VERSION=3.11 # Python 3.9 and 3.10 are also supported.
   ```

6. Run the script.

   ```shell
   python tools/build/configure.py
   bazel build \
     ${BAZEL_OPTIMIZATION} \
     -- //tensorflow_io_gcs_filesystem/... //tensorflow_io:python/ops/libtensorflow_io.so //tensorflow_io:python/ops/libtensorflow_io_plugins.so
   ````

7. Generate `whl` files for `tensorflow-io` and `tensorflow-io-gcs-filesystem`.

   ```shell
   python setup.py --data bazel-bin -q bdist_wheel --plat-name macosx_12_0_arm64
   rm -rf build
   python setup.py --project tensorflow-io-gcs-filesystem --data bazel-bin -q bdist_wheel --plat-name macosx_12_0_arm64
   ```

8. Please do not forget to install the `whl` file, and you need to install `tensorflow-io-gcs-filesystem` first and then install `tensorflow-io`.

   ```shell
   pip install dist/tensorflow_io_gcs_filesystem-*.whl
   pip install dist/tensorflow_io-*.whl
   ```

## Tips&Refer

1. `io` need to correspond with the version of `tensorflow`. The specific correspondence is [here](https://github.com/tensorflow/io/blob/master/README.md#tensorflow-version-compatibility).
2. Please ensure that your network is stable during the compilation process, and the compilation needs to use the network.
