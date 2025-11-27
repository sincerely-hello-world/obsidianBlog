## 新建功能包
```
ros2 pkg create my_pkg --build-type ament_python
```
## 包的目录结构：
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
## setup.py
```python
from setuptools import setup

package_name = 'my_pkg' # zi'dong'h

setup(
    name=package_name,
    version='0.0.0',
    packages=[package_name],
    data_files=[
        ('share/ament_index/resource_index/packages',
            ['resource/' + package_name]),
        ('share/' + package_name, ['package.xml']),
    ],
    install_requires=['setuptools'],
    zip_safe=True,
    maintainer='focal',
    maintainer_email='focal@todo.todo',
    description='TODO: Package description',
    license='TODO: License declaration',
    tests_require=['pytest'],
    entry_points={
        'console_scripts': [
        ],
    },
)

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

  <depend>rclpy</depend>   在package字段，添加依赖项，一般为包名
  <export>
    <build_type>ament_python</build_type>   
    对应参数 ros2 pkg create my_pkg --build-type ament_python 
  </export>
</package>
```