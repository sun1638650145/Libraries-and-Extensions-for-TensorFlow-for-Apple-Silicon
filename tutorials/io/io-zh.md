# 从源码构建TensorFlow I/O

## 注意 ⚠️

`TensorFlow I/O`已经提供全部Python版本的Apple Silicon预编译`whl`文件, 你可以直接从这个[页面](https://pypi.org/project/tensorflow-io/)下载.

## 必要条件

这里假设了您有必要的类Unix知识, 已经在您的终端内安装好了[`brew`](https://brew.sh)和[`conda`](https://github.com/conda-forge/miniforge), 这里不再赘述`brew`和`conda`安装和使用方法; 最重要的是, 这个教程完全基于Apple Silicon构建, 所以确保您手中的Mac是Apple Silicon.

## Step by Step

1. 创建新的环境并安装Apple提供的依赖项.

   ```shell
   conda create -n tensorflow-macos python=3.11 # 这里Python版本也可以使用Python 3.8, 3.9和3.10.
   conda activate tensorflow-macos
   ```

2. 安装`tensorflow`和`tensorflow-metal`插件.

   ```shell
   pip install tensorflow==2.13.0 # 从tensorflow 2.13开始官方支持Apple silicon.
   pip install tensorflow-metal==1.0.1
   ```

3. 安装`bazel 5.1.1`.

   ```shell
   wget https://raw.githubusercontent.com/Homebrew/homebrew-core/2940e900476b4452c8047c75dcbfc193c6f30341/Formula/bazel.rb
   brew install ./bazel.rb
   bazel --version # 确保版本是5.1.1即可.
   ```

   * 通常情况下`brew`安装的`bazel`会是最新版的, 最新版往往和`io`要求的版本不匹配, 这可能会出现很多意想不到的问题, 所以我们通过手动指定版本安装.

4. 下载并解压`io 0.34.0`.

   ```shell
   wget https://github.com/tensorflow/io/archive/refs/tags/v0.34.0.zip
   unzip ./v0.34.0.zip
   cd io-0.34.0
   ```

5. 运行脚本构建.

   ```shell
   ./configure.sh
   bazel build -s --verbose_failures $BAZEL_OPTIMIZATION //tensorflow_io/... //tensorflow_io_gcs_filesystem/...
   ```

6. 生成`tensorflow-io`和`tensorflow-io-gcs-filesystem`的`whl`文件.

   ```shell
   python setup.py bdist_wheel --data bazel-bin
   python setup.py bdist_wheel --data bazel-bin --project tensorflow-io-gcs-filesystem
   ```

7. 千万不要忘记安装`whl`文件, 需要先安装`tensorflow-io-gcs-filesystem`然后再安装`tensorflow-io`.

   ```shell
   pip install dist/tensorflow_io_gcs_filesystem-*.whl
   pip install dist/tensorflow_io-*.whl
   ```

## Tips&Refer

1. `io`需要和`tensorflow`的版本对应. 具体对应关系在[这里](https://github.com/tensorflow/io/blob/master/README.md#tensorflow-version-compatibility).
2. 编译过程中请保证你的网络稳定, 编译需要使用网络.
