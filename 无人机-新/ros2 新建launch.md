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
```

```