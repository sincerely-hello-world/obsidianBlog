\
```
mkdir -p ~/ros2_launch_ws/src
cd ~/ros2_launch_ws
colcon build
source install/setup.bash
```

```
cd src 
ros2 pkg create my_launch_pkg --build-type ament_python 
cd my_launch_pkg
```


launch如何写？

先修改位于
```
GroundStation/
├── GroundStation
│   ├── __init__.py
│   ├── logical.py
│   ├── main.ui
│   ├── __pycache__
│   ├── qrcode.py
│   ├── ros2_node_Ground.py
│   ├── subDialog1.ui
│   ├── Ui_main.py
│   └── Ui_subDialog1.py
├── launch
│   └── nodes.launch.py
├── package.xml
├── resource
│   └── GroundStation
├── setup.cfg
├── setup.py     # < 这里需要修改
├── test
│   ├── test_copyright.py
│   ├── test_flake8.py
│   └── test_pep257.py
```

setup.py     # < 这里需要修改
```python
from setuptools import setup
import os
from glob import glob

package_name = 'GroundStation'

setup(
    name=package_name,
    version='0.0.0',
    packages=[package_name],
    data_files=[
        ('share/ament_index/resource_index/packages',
            ['resource/' + package_name]),
        ('share/' + package_name, ['package.xml']),
        (os.path.join('share', package_name, 'launch'), glob(os.path.join('launch', '*.launch.py'))), # < 添加此行，让构建完成后的 luanch文件能被找到
        
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
            'logical = GroundStation.logical:main',
            'qrcode = GroundStation.qrcode:main'
        ],
    },
)

```