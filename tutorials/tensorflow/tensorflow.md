# Build TensorFlow from source

## Note ⚠️

Building TensorFlow from source is only recommended for the convenience of building other TensorFlow libraries and extensions. For daily use, we recommend using [tensorflow-macos](https://pypi.org/project/tensorflow-macos/).

## Prerequisites

Please use `Xcode 14.3` and `Apple clang version 14.0.3 (clang-1403.0.22.14.1)`. Previous versions may encounter issues with the `ld` command. It is also assumed that you have extensive experience in compilation and development and are familiar with other aspects of toolchains and environments. Further details will not be discussed here.

## Step by Step

1. Create a new Env.

    ```shell
    conda create -n tensorflow-macos python=3.10  # Python 3.8 and 3.9 are also supported here, and in theory, 3.11 should also be supported. However, extensive library support for 3.11 is incomplete and has not yet been tested.
    conda acivate tensorflow-macos
    ```

2. Install TensorFlow dependencies.

    ```shell
    pip install -U pip numpy wheel packaging requests opt_einsum
    pip install -U keras_preprocessing --no-deps
    ```

3. Install `bazel 5.3.0`.

    ```shell
    wget https://raw.githubusercontent.com/Homebrew/homebrew-core/59dff37de3c670a77c1e9f39b8a4b0f8884a391b/Formula/bazel.rb
    brew install ./bazel.rb
    bazel --version # Make sure the version is 5.3.0.
    ```
    
    * Normally, `bazel` installed via `brew` will be the latest version, which often does not match the required version. This can lead to many unexpected problems, so we manually specify the version to install.

4. Download and extract `tensorflow 2.12.0`.

    ```shell
    wget https://github.com/tensorflow/tensorflow/archive/refs/tags/v2.12.0.zip
    unzip v2.12.0.zip
    cd tensorflow-2.12.0
    ```

5. Configure the build.

    ```shell
    ./configure  # Please use all default options.
    ```

6. Build `tensorflow` (which takes approximately `1:45` hours on the author's M1 MacBook Pro `16`GB).

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
