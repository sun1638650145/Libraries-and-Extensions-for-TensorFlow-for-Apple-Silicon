# 从源码构建 TensorFlow

## 注意 ⚠️

从源码构建 TensorFlow 仅为更便利地构建其他的 TensorFlow 库和拓展, 日常使用推荐[tensorflow-macos](https://pypi.org/project/tensorflow-macos/).

## 必要条件

请使用`Xcode 14.3`和`Apple clang version 14.0.3 (clang-1403.0.22.14.1)`, 之前的版本可以确认`ld`命令存在问题. 同时假设您有丰富的编译开发经验, 关于工具链和环境的其他部分就不再赘述.

## Step by Step

1. 创建新的环境.

    ```shell
    conda create -n tensorflow-macos python=3.10  # 这里Python版本也可以使用Python 3.8和3.9, 理论上也可以支持3.11但是大量库支持不完善暂未进行测试.
    conda activate tensorflow-macos
    ```

2. 安装`tensorflow`的依赖项.

    ```shell
    pip install -U pip numpy wheel packaging requests opt_einsum
    pip install -U keras_preprocessing --no-deps
    ```

3. 安装`bazel 5.3.0`.

    ```shell
    wget https://raw.githubusercontent.com/Homebrew/homebrew-core/59dff37de3c670a77c1e9f39b8a4b0f8884a391b/Formula/bazel.rb
    brew install ./bazel.rb
    bazel --version # 确保版本是5.3.0即可.
    ```
    
    * 通常情况下`brew`安装的`bazel`会是最新版的, 最新版往往和要求的版本不匹配, 这可能会出现很多意想不到的问题, 所以我们通过手动指定版本安装.

4. 下载并解压`tensorflow 2.12.0`.

    ```shell
    wget https://github.com/tensorflow/tensorflow/archive/refs/tags/v2.12.0.zip
    unzip v2.12.0.zip
    cd tensorflow-2.12.0
    ```

5. 配置build.

    ```shell
    ./configure  # 请全部使用默认选项.
    ```

6. 构建`tensorflow`(在作者本人的`M1 MacBook Pro 16GB`大概需要`1:45`小时).

    ```shell
    bazel build //tensorflow/tools/pip_package:build_pip_package           
    ```

7. 在当前目录下, 生成`whl`文件.

    ```shell
    ./bazel-bin/tensorflow/tools/pip_package/build_pip_package ./
    ```

8. 安装`whl`文件.

    ```shell
    pip install ./*.whl
    ```

## Tips

1. 编译过程中请保证你的网络稳定, 编译需要使用网络.
