![](assets/node/file-20251124141657013.png)

```
import rclpy
from rclpy.node import Nodeb

def main():
	rclpy.init()
	node = Node("python_node")
	node.get_logger().info('你好 Python 节点!')
	rclpy.spin(node)
	rclpy.shutdown()
if_name__==__main__":
	main()
```

```
python3 ros2_nodefile.py
```