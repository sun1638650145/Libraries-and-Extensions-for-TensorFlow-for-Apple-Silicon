# 从源码构建TensorFlow Text

## 注意 ⚠️

`TensorFlow Text`已经提供Python `3.9-3.11`版本的Apple 芯片预编译`whl`文件, 你可以直接从这个[页面](https://pypi.org/project/tensorflow-text/)下载. 关于`Python 3.12`的支持问题请关注这个[issue](https://github.com/tensorflow/text/issues/1297).

## 必要条件

这里假设了您有必要的类Unix知识, 已经在您的终端内安装好了[`brew`](https://brew.sh)和[`conda`](https://github.com/conda-forge/miniforge), 这里不再赘述`brew`和`conda`安装和使用方法; 最重要的是, 这个教程完全基于Apple 芯片构建, 所以确保您手中的Mac是Apple 芯片.

## Step by Step

1. 创建新的环境并安装Apple提供的依赖项.

   ```shell
   conda create -n tensorflow-macos python=3.11 # 这里Python版本也可以使用Python 3.9或3.10.
   conda activate tensorflow-macos
   ```
   
2. 安装`tensorflow`.

   ```shell
   pip install tensorflow==2.17.0
   ```

3. 安装`bazel 6.5.0`.

   ```shell
   wget https://github.com/bazelbuild/bazel/releases/download/6.5.0/bazel-6.5.0-darwin-arm64 -O bazel
   chmod +x bazel
   sudo mv bazel /usr/local/bin/
   bazel --version # 确保版本是6.5.0即可.
   ```

4. 下载并解压`text 2.17.0`.

   ```shell
   wget https://github.com/tensorflow/text/archive/refs/tags/v2.17.0.zip
   unzip ./v2.17.0.zip
   cd text-2.17.0
   ```

5. 运行脚本构建.

   ```shell
   ./oss_scripts/run_build.sh
   ```
   
6. [千万不要忘记安装whl文件.](https://github.com/sun1638650145/Libraries-and-Extensions-for-TensorFlow-for-Apple-Silicon/issues/2)

   ```shell
   pip install ./*.whl
   ```

## Tips&Refer

1. `text`需要和`tensorflow`的次要版本对应(比如`tensorflow-macos==2.7.0`和`tensorflow-text==2.7.3`)
2. 编译过程中请保证你的网络稳定, 编译需要使用网络.
3. [本人添加Apple Silicon支持的PR.](https://github.com/tensorflow/text/pull/756)
