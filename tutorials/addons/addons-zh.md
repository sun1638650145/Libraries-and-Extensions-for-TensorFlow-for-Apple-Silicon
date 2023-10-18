# 从源码构建TensorFlow Addons

## 注意 ⚠️

`TensorFlow Addons`已经提供全部Python版本的Apple 芯片预编译`whl`文件, 你可以直接从这个[页面](https://pypi.org/project/tensorflow-addons/)下载.

## 必要条件

这里假设了您有必要的类Unix知识, 已经在您的终端内安装好了[`brew`](https://brew.sh)和[`conda`](https://github.com/conda-forge/miniforge), 这里不再赘述`brew`和`conda`安装和使用方法; 最重要的是, 这个教程完全基于Apple 芯片构建, 所以确保您手中的Mac是Apple 芯片.

## Step by Step

1. 创建新的环境并安装Apple提供的依赖项.

   ```shell
   conda create -n tensorflow-macos python=3.11 # 这里Python版本也可以使用Python 3.9或3.10.
   conda activate tensorflow-macos
   ```
   
2. 安装`tensorflow`和`tensorflow-metal`插件.

   ```shell
   pip install tensorflow==2.14.0
   pip install tensorflow-metal==1.1.0
   ```

3. 安装`bazel 6.1.0`.

   ```shell
   wget https://raw.githubusercontent.com/Homebrew/homebrew-core/a9b3083e23806aebe61f7c39d393734a6949eaa5/Formula/bazel.rb
   brew install ./bazel.rb
   bazel --version # 确保版本是6.1.0即可.
   ```

   * 通常情况下`brew`安装的`bazel`会是最新版的, 最新版往往和`addons`要求的版本不匹配, 这可能会出现很多意想不到的问题, 所以我们通过手动指定版本安装.

4. 下载并解压`addons 0.22.0`.

   ```shell
   wget https://github.com/tensorflow/addons/archive/refs/tags/v0.22.0.zip
   unzip ./v0.22.0.zip
   cd addons-0.22.0
   ```

5. 运行脚本构建.

   ```shell
   python3 ./configure.py
   bazel build build_pip_pkg
   bazel-bin/build_pip_pkg artifacts
   ```

6. 千万不要忘记安装`whl`文件.

   ```shell
   pip install artifacts/*.whl
   ```

## Tips&Refer

1. `addons`需要和`tensorflow`的版本对应. 具体对应关系在[这里](https://github.com/tensorflow/addons/blob/a5cd76d341c594f464a5c9be8e572ed5bd3f3b8b/README.md?plain=1#L80).
2. 编译过程中请保证你的网络稳定, 编译需要使用网络.
3. [本人添加Apple Silicon Python 3.10支持PR.](https://github.com/tensorflow/addons/pull/2718)
