# 从源码构建TensorFlow Text

## 注意 ⚠️

请使用`Xcode 14.3`和`Apple clang version 14.0.3 (clang-1403.0.22.14.1)`.

## 必要条件

这里假设了您有必要的类Unix知识, 已经在您的终端内安装好了[`brew`](https://brew.sh)和[`conda`](https://github.com/conda-forge/miniforge), 这里不再赘述`brew`和`conda`安装和使用方法; 最重要的是, 这个教程完全基于Apple Silicon构建, 所以确保您手中的Mac是Apple Silicon.

## Step by Step

1. 创建新的环境并安装Apple提供的依赖项.

   ```shell
   conda create -n tensorflow-macos python=3.10 # 这里Python版本也可以使用Python 3.8和3.9
   conda activate tensorflow-macos
   conda install -c apple tensorflow-deps==2.10.0  # 目前Apple没有发布tensorflow-deps 2.11和2.12, 2.10就是最新版.
   ```

2. 安装`tensorFlow-macos`和`tensorflow-metal`插件.

   ```shell
   pip install tensorflow-macos==2.12.0
   pip install tensorflow-metal==0.8.0
   ```

3. 安装`bazel 5.3.0`.

   ```shell
   wget https://raw.githubusercontent.com/Homebrew/homebrew-core/59dff37de3c670a77c1e9f39b8a4b0f8884a391b/Formula/bazel.rb
   brew install ./bazel.rb
   bazel --version # 确保版本是5.3.0即可.
   ```

   * 通常情况下`brew`安装的`bazel`会是最新版的, 最新版往往和`text`要求的版本不匹配, 这可能会出现很多意想不到的问题, 所以我们通过手动指定版本安装.

4. 下载并解压`text 2.12.0`.

   ```shell
   wget https://github.com/tensorflow/text/archive/refs/tags/v2.12.0.zip
   unzip ./v2.12.0.zip
   cd text-2.12.0
   ```

5. 修改源码的一些参数以此确保能正确构建.

   * `oss_scripts/configure.sh`修改第49行为

     ```shell
     pip install tensorflow-macos==2.12.0
     ```

   * (可选, 如果你没有通过`brew`安装`bazel`请直接跳过)`oss_scripts/run_build.sh`修改第19行为

       ```shell
       tf_bazel_version='5.3.0-homebrew'
       ```

6. 运行脚本构建.

   ```shell
   ./oss_scripts/run_build.sh
   ```

7. [千万不要忘记安装whl文件.](https://github.com/sun1638650145/Libraries-and-Extensions-for-TensorFlow-for-Apple-Silicon/issues/2)

   ```python
   pip install ./*.whl
   ```

## Tips&Refer

1. `text`需要和`tensorflow`的次要版本对应(比如`tensorflow-macos==2.7.0`和`tensorflow-text==2.7.3`)
2. 编译过程中请保证你的网络稳定, 编译需要使用网络.
3. [本人添加Apple Silicon支持的PR.](https://github.com/tensorflow/text/pull/756)

