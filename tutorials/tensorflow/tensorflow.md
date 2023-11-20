# Build TensorFlow from source

## Note ⚠️

Building TensorFlow from source is only recommended for the convenience of building other TensorFlow libraries and extensions. For daily use, we recommend using [tensorflow-macos](https://pypi.org/project/tensorflow-macos/).

## Prerequisites

Please use `Xcode 14.3` and `Apple clang version 14.0.3 (clang-1403.0.22.14.1)` or later versions is required. Previous versions may encounter issues with the `ld` command. It is also assumed that you have extensive experience in compilation and development and are familiar with other aspects of toolchains and environments. Further details will not be discussed here.

## Step by Step

1. Create a new Env.

    ```shell
    conda create -n tensorflow-macos python=3.11 # Python 3.9 and 3.10 are also supported.
    conda activate tensorflow-macos
    ```

2. Install TensorFlow dependencies.

    ```shell
    pip install -U pip numpy wheel packaging requests opt_einsum
    pip install -U keras_preprocessing --no-deps
    ```

3. Install `bazel 6.1.0`.

    ```shell
    wget https://raw.githubusercontent.com/Homebrew/homebrew-core/a9b3083e23806aebe61f7c39d393734a6949eaa5/Formula/bazel.rb
    brew install ./bazel.rb
    bazel --version # Make sure the version is 6.1.0.
    ```

    * Normally, `bazel` installed via `brew` will be the latest version, which often does not match the required version. This can lead to many unexpected problems, so we manually specify the version to install.

4. Install `coreutils`.

    ```shell
    brew install coreutils
    echo 'export PATH="/opt/homebrew/opt/coreutils/libexec/gnubin:$PATH"' >> .zshrc
    source .zshrc
    realpath --help # Make sure it points to the GNU version of realpath.
    conda activate tensorflow-macos
    ```

    * There might be a compilation failure caused by macOS's `realpath`, so we're using the GNU version of `realpath` to resolve it.

5. Download and extract `tensorflow 2.14.0`.

    ```shell
    wget https://github.com/tensorflow/tensorflow/archive/refs/tags/v2.14.0.zip
    unzip v2.14.0.zip
    cd tensorflow-2.14.0
    ```

6. Add additional linker flag `-ld_classic` at line 2393 in `tensorflow/tensorflow.bzl`.

    ```bazel
    clean_dep("//tensorflow:macos"): [
        # TODO: the -w suppresses a wall of harmless warnings about hidden typeinfo symbols
        # not being exported.  There should be a better way to deal with this.
        "-ld_classic",
        "-Wl,-w",
        "-Wl,-exported_symbols_list,$(location %s.lds)" % vscriptname,
    ],
    ```

7. Set the environment variable `TF_PYTHON_VERSION`.

    ```shell
    export TF_PYTHON_VERSION=3.11 # Python 3.9 and 3.10 are also supported.
    ```

8. Configure the build.

    ```shell
    ./configure # Please use all default options.
    ```

9. Build `tensorflow` (which takes approximately `70` minutes on the author's M1 MacBook Pro `16`GB).

    ```shell
    bazel build //tensorflow/tools/pip_package:build_pip_package           
    ```

10. In the current directory, generate the `whl` file.

    ```shell
    ./bazel-bin/tensorflow/tools/pip_package/build_pip_package ./
    ```

11. Install the `whl` file.

     ```shell
     pip install ./*.whl
     ```

## Tips

1. Please ensure that your network is stable during the compilation process, and the compilation needs to use the network.
