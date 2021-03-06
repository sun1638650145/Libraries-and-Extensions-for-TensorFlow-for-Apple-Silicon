# 从源码构建TensorFlow Text

## 必要条件

这里假设了您有必要的类Unix知识, 已经在您的终端内安装好了[`brew`](https://brew.sh)和[`conda`](https://github.com/conda-forge/miniforge), 这里不再赘述`brew`和`conda`安装和使用方法; 最重要的是, 这个教程完全基于Apple Silicon(比如M1, M1 Pro, M1 Max或M1 Ultra)构建, 所以确保您手中的Mac是Apple Silicon.

## Step by Step

1. 创建新的环境并安装Apple提供的依赖项.

   ```shell
   conda create -n tensorflow-macos python=3.9 # 这里Python版本也可以使用Python 3.8
   conda activate tensorflow-macos
   conda install -c apple tensorflow-deps==2.9.0
   ```

2. 安装`tensorFlow-macos`和`tensorflow-metal`插件.

   ```shell
   pip install tensorflow-macos==2.9.0
   pip install tensorflow-metal==0.5.0
   ```

3. 安装`bazel 5.1.1`.

   ```shell
   wget https://raw.githubusercontent.com/Homebrew/homebrew-core/2940e900476b4452c8047c75dcbfc193c6f30341/Formula/bazel.rb
   brew install ./bazel.rb
   bazel --version # 确保版本是5.1.x即可.
   ```

   * 通常情况下`brew`安装的`bazel`会是最新版的, 最新版往往和`text`要求的版本不匹配, 这可能会出现很多意想不到的问题, 所以我们通过手动指定版本安装.

4. 下载并解压`text 2.9.0`.

   ```shell
   wget https://github.com/tensorflow/text/archive/refs/tags/v2.9.0.zip
   unzip ./v2.9.0.zip
   cd text-2.9.0
   ```

5. 修改源码的一些参数以此确保能正确构建.

   * `oss_scripts/configure.sh`修改第49行为

     ```shell
     pip install tensorflow-macos==2.9.0
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

