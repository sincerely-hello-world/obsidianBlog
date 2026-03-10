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
GroundStation 包目录
└── |    GroundStation 包名
	│   ├── __init__.py
	│   ├── logical.py
	│   ├── main.ui
	│   ├── nodes.launch.py
	│   ├── __pycache__
	│   ├── qrcode.py
	│   ├── ros2_node_Ground.py
	│   ├── subDialog1.ui
	│   ├── Ui_main.py
	│   └── Ui_subDialog1.py
	├── launch
	├── package.xml
	├── resource
	│   └── GroundStation
	├── setup.cfg
	├── setup.py
	└── test
 