# Build TensorFlow from source

## Prerequisites

Please use `Xcode 15.3` and `Apple clang version 15.0.0 (clang-1500.3.9.4)` or later versions is required. Previous versions may encounter issues with the `ld` command. It is also assumed that you have extensive experience in compilation and development and are familiar with other aspects of toolchains and environments. Further details will not be discussed here.

## Step by Step

1. Create a new Env.

    ```shell
    conda create -n tensorflow-macos python=3.13 # Python 3.9, 3.10, 3.11 and 3.12 are also supported.
    conda activate tensorflow-macos
    ```

2. Install `bazel 7.4.1`.

    ```shell
    wget https://github.com/bazelbuild/bazel/releases/download/7.4.1/bazel-7.4.1-darwin-arm64 -O bazel
    chmod +x bazel
    sudo mv bazel /usr/local/bin/
    bazel --version # Make sure the version is 7.4.1.
    ```

3. Download and extract `tensorflow 2.20.0`.

    ```shell
    wget https://github.com/tensorflow/tensorflow/archive/refs/tags/v2.20.0.zip
    unzip v2.20.0.zip
    cd tensorflow-2.20.0
    ```

4. Set the environment variable `TF_PYTHON_VERSION`.

    ```shell
    export TF_PYTHON_VERSION=3.13 # Corresponding to the Python version above, please.
    ```

5. Configure the build.

    ```shell
    ./configure # Please use all default options.
    ```

6. Build `tensorflow` (which takes approximately `55` minutes on the author's M4 Mac mini `32`GB).

    ```shell
    bazel build //tensorflow/tools/pip_package:wheel
    ```

7. Install the `whl` file.

    ```shell
    mv ./bazel-bin/tensorflow/tools/pip_package/wheel_house/*.whl ./bazel-bin/tensorflow/tools/pip_package/wheel_house/tensorflow-2.20.0-cp313-cp313-macosx_12_0_arm64.whl # Corresponding to the tensorflow and Python version above, please.
    pip install ./bazel-bin/tensorflow/tools/pip_package/wheel_house/*.whl
    ```

## Tips

1. Please ensure that your network is stable during the compilation process, and the compilation needs to use the network.
