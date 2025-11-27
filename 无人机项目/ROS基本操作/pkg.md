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