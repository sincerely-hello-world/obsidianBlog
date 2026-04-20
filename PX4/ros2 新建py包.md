1. 创建
```bash
mkdir  <your_ros2_ws>  <your_ros2_ws>/src
cd     <your_ros2_ws>/src
ros2 pkg create --build-type ament_python <package_name>


ros2 pkg create --build-type ament_python my_offboard 
```

2. 修改 setup.py
```python
import os
from glob import glob


from setuptools import find_packages, setup

package_name = 'my_offboard'

setup(

    name=package_name,
    version='0.0.0',
    packages=find_packages(exclude=['test']),
    data_files=[
        ('share/ament_index/resource_index/packages',
            ['resource/' + package_name]),
        # ('share/ament_index/resource_index/packages',
        #     ['resource/' + 'visualize.rviz']),
        ('share/' + package_name, ['package.xml']),
        (os.path.join('share', package_name), glob('launch/*launch.[pxy][yma]*')),
        (os.path.join('share', package_name), glob('launch/*launch.*')),
        (os.path.join('share', package_name), glob('resource/*rviz'))
    ],

    install_requires=['setuptools'],
    zip_safe=True,
    maintainer='sincerely',
    maintainer_email='sincerely@todo.todo',
    description='TODO: Package description',
    license='TODO: License declaration',
    extras_require={
        'test': [
            'pytest',
        ],
    },
    entry_points={
        'console_scripts': [
            'myoffboard_control = my_offboard.myoffboard_control:main',
            'myoffboard_control2 = my_offboard.myoffboard_control2:main',
            'odometry = my_offboard.odometry:main',
            'mykey = my_offboard.mykey:main',
        ],
    },
)
```

3.  修改 package.xml
```xml
<?xml version="1.0"?>
<?xml-model href="http://download.ros.org/schema/package_format3.xsd" schematypens="http://www.w3.org/2001/XMLSchema"?>
<package format="3">
  <name>px4_offboard</name>
  <version>0.0.0</version>
  <description>ROS2 python interface for px4</description>
  <maintainer email="jalim@ethz.ch">jaeyoung</maintainer>
  <license>BSD-3</license>

  <depend>px4_msgs</depend> 

  <exec_depend>ros2launch</exec_depend>
  <test_depend>ament_copyright</test_depend>
  <test_depend>ament_flake8</test_depend>
  <test_depend>ament_pep257</test_depend>
  <test_depend>python3-pytest</test_depend>
  <export>
    <build_type>ament_python</build_type>
  </export>
</package>
```