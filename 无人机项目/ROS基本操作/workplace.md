
最佳实践是为每个新的工作空间创建一个新的目录。
目录名称无关紧要，但是有助于表明工作空间的目的。


让我们选择
+ 目录名为`workspace_folder`，表示`开发工作空间`：
+ s'r
```
workspace_folder/
    src/
      cpp_package_1/
          CMakeLists.txt
          include/cpp_package_1/
          package.xml
          src/          # 源码文件

      py_package_1/
          package.xml
          resource/py_package_1
          setup.cfg
          setup.py
          py_package_1/  # 源码文件
      ...
```