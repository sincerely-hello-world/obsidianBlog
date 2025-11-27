功能包创建
```
ros2 pkg create my_pkg --build-type ament_python
```
功能包的目录结构：
```bash
./my_pkg/
├── my_pkg
│   └── __init__.py
├── package.xml  # 依赖项配置， 类似于CMakeList
├── resource
│   └── my_pkg
├── setup.cfg
├── setup.py     # 主要的启动配置
└── test
    ├── test_copyright.py
    ├── test_flake8.py
    └── test_pep257.py
```


## package.xml
```xml
<?xml version="1.0"?>
<?xml-model href="http://download.ros.org/schema/package_format3.xsd" schematypens="http://www.w3.org/2001/XMLSchema"?>
<package format="3">
  <name>my_pkg</name>
  <version>0.0.0</version>
  <description>TODO: Package description</description>
  <maintainer email="focal@todo.todo">focal</maintainer>
  <license>TODO: License declaration</license>

  <test_depend>ament_copyright</test_depend>
  <test_depend>ament_flake8</test_depend>
  <test_depend>ament_pep257</test_depend>
  <test_depend>python3-pytest</test_depend>

  <depend>rclpy</depend>
  <export>
    <build_type>ament_python</build_type>
  </export>
</package>

```