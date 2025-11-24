![](assets/node/file-20251124141657013.png)

```python
import rclpy
from rclpy.node import Node

def main():
    rclpy.init()                     # 初始化 ROS 2 客户端库
    node = Node("python_node")       # 创建一个名为 "python_node" 的节点
    node.get_logger().info('你好 Python 节点!')  # 打印日志信息
    rclpy.spin(node)                 # 保持节点运行，处理回调（如订阅、服务等）
    node.destroy_node()              # （可选但推荐）显式销毁节点
    rclpy.shutdown()                 # 关闭 ROS 2 客户端库

if __name__ == '__main__':
    main()
```

```
python3 ros2_nodefile.py
```