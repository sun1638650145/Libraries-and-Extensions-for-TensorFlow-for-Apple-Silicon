# Build TensorFlow I/O from source

## Note ⚠️

`TensorFlow I/O` already provides Apple Silicon precompiled `whl` files for all Python versions, which you can download directly from this [page](https://pypi.org/project/tensorflow-io/).

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

3. Install `bazel 5.1.1`.

   ```shell
   wget https://raw.githubusercontent.com/Homebrew/homebrew-core/2940e900476b4452c8047c75dcbfc193c6f30341/Formula/bazel.rb
   brew install ./bazel.rb
   bazel --version # Make sure the version is 5.1.1.
   ````

   * Normally, the version of `bazel` installed via `brew` is the latest one, which may not match the version required by the `io`. This can lead to unexpected issues, so we need to manually specify the version for installation.

4. Download and extract `io 0.34.0`.

   ```shell
   wget https://github.com/tensorflow/io/archive/refs/tags/v0.34.0.zip
   unzip ./v0.34.0.zip
   cd io-0.34.0
   ````

5. Run the script.

   ```shell
   ./configure.sh
   bazel build -s --verbose_failures $BAZEL_OPTIMIZATION //tensorflow_io/... //tensorflow_io_gcs_filesystem/...
   ````

6. Generate `whl` files for `tensorflow-io` and `tensorflow-io-gcs-filesystem`.

   ```shell
   python setup.py bdist_wheel --data bazel-bin
   python setup.py bdist_wheel --data bazel-bin --project tensorflow-io-gcs-filesystem
   ```

7. Please do not forget to install the `whl` file, and you need to install `tensorflow-io-gcs-filesystem` first and then install `tensorflow-io`.

   ```shell
   pip install dist/tensorflow_io_gcs_filesystem-*.whl
   pip install dist/tensorflow_io-*.whl
   ```

## Tips&Refer

1. `io` need to correspond with the version of `tensorflow`. The specific correspondence is [here](https://github.com/tensorflow/io/blob/master/README.md#tensorflow-version-compatibility).
2. Please ensure that your network is stable during the compilation process, and the compilation needs to use the network.
