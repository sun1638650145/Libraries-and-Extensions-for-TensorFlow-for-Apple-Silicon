# 从源码构建 TensorFlow

## 必要条件

请使用`Xcode 15.3`和`Apple clang version 15.0.0 (clang-1500.3.9.4)`及以上版本, 之前的版本可以确认`ld`命令存在问题. 同时假设您有丰富的编译开发经验, 关于工具链和环境的其他部分就不再赘述.

## Step by Step

1. 创建新的环境.

    ```shell
    conda create -n tensorflow-macos python=3.12 # 这里Python版本也可以使用Python 3.9, 3.10和3.11.
    conda activate tensorflow-macos
    ```

2. 安装`bazel 6.5.0`.

    ```shell
    wget https://github.com/bazelbuild/bazel/releases/download/6.5.0/bazel-6.5.0-darwin-arm64 -O bazel
    chmod +x bazel
    sudo mv bazel /usr/local/bin/
    bazel --version # 确保版本是6.5.0即可.
    ```

3. 下载并解压`tensorflow 2.17.0`.

    ```shell
    wget https://github.com/tensorflow/tensorflow/archive/refs/tags/v2.17.0.zip
    unzip v2.17.0.zip
    cd tensorflow-2.17.0
    ```

4. 设置环境变量`TF_PYTHON_VERSION`.

    ```shell
    export TF_PYTHON_VERSION=3.12 # 请和上面的Python版本对应.
    ```

5. 配置build.

    ```shell
    ./configure # 请全部使用默认选项.
    ```

6. 构建`tensorflow`(在作者本人的`M1 MacBook Pro 16GB`大概需要`70`分钟).

    ```shell
    bazel build //tensorflow/tools/pip_package:wheel
    ```

7. 安装`whl`文件.

     ```shell
     pip install ./bazel-bin/tensorflow/tools/pip_package/wheel_house/*.whl
     ```

## Tips

1. 编译过程中请保证你的网络稳定, 编译需要使用网络.
