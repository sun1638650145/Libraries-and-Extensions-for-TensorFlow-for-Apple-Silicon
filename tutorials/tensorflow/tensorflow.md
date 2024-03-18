# Build TensorFlow from source

## Prerequisites

Please use `Xcode 15.3` and `Apple clang version 15.0.0 (clang-1500.3.9.4)` or later versions is required. Previous versions may encounter issues with the `ld` command. It is also assumed that you have extensive experience in compilation and development and are familiar with other aspects of toolchains and environments. Further details will not be discussed here.

## Step by Step

1. Create a new Env.

    ```shell
    conda create -n tensorflow-macos python=3.12 # Python 3.9, 3.10 and 3.11 are also supported.
    conda activate tensorflow-macos
    ```

2. Install `bazel 6.5.0`.

    ```shell
    wget https://github.com/bazelbuild/bazel/releases/download/6.5.0/bazel-6.5.0-darwin-arm64 -O bazel
    chmod +x bazel
    sudo mv bazel /usr/local/bin/
    bazel --version # Make sure the version is 6.5.0.
    ```

3. Download and extract `tensorflow 2.16.1`.

    ```shell
    wget https://github.com/tensorflow/tensorflow/archive/refs/tags/v2.16.1.zip
    unzip v2.16.1.zip
    cd tensorflow-2.16.1
    ```

4. Set the environment variable `TF_PYTHON_VERSION`.

    ```shell
    export TF_PYTHON_VERSION=3.12 # Corresponding to the Python version above, please.
    ```

5. Configure the build.

    ```shell
    ./configure # Please use all default options.
    ```

6. Build `tensorflow` (which takes approximately `70` minutes on the author's M1 MacBook Pro `16`GB).

    ```shell
    bazel build //tensorflow/tools/pip_package:build_pip_package
    ```

7. In the current directory, generate the `whl` file.

    ```shell
    ./bazel-bin/tensorflow/tools/pip_package/build_pip_package ./
    ```

8. Install the `whl` file.

     ```shell
     pip install ./*.whl
     ```

## Tips

1. Please ensure that your network is stable during the compilation process, and the compilation needs to use the network.
