
```python
import rclpy
from rclpy.node import Node
import requests
from example_interfaces.msg import String
from queue import Queue

class NovelPubNode(Node):
    def __init__(self, node_name):
        super().__init__(node_name)
        self.novels_queue_ = Queue() # 创建队列，存放小说
        # 创建话题发布者，发布小说
        self.novel_publisher_ = self.create_publisher(String, 'novel', 10)
        self.timer_ = self.create_timer(5, self.timer_callback) # 创建定时器

    def download_novel(self, url):
        response = requests.get(url)
        response.encoding = 'utf-8'
        self.get_logger().info(f'下载完成：{url}')
        for line in response.text.splitlines(): # 按行分割，放入队列
            self.novels_queue_.put(line)

    def timer_callback(self):
        if self.novels_queue_.qsize() > 0: # 队列中有数据，取出发布一行
            msg = String() # 实例化一个消息
            msg.data = self.novels_queue_.get() # 对消息结构体进行赋值
            self.novel_publisher_.publish(msg) # 发布消息
            self.get_logger().info(f'发布了一行小说：{msg.data}') 

def main():
    rclpy.init()
    node = NovelPubNode('novel_pub')
    node.download_novel('http://localhost:8848/novel1.txt')
    rclpy.spin(node)
    rclpy.shutdown()
```


```bash

ros2 topic info /novel # 查看节点信息
# 输出类似： 
# >> ros2 topic info  /novel  
# Type: example_interfaces/msg/String
# Publisher count: 1
# Subscription count: 0 
---
```

```bash
ros2 topic echo /chatter std_msgs/msg/String 
# 这表示：请订阅 `/chatter` 话题，并按照 `std_msgs/msg/String` 这种格式来解析收到的数据。
# 但在大多数情况下，只写话题名就够了：
ros2 topic echo /chatter
# 输出类似：
#ros2 topic echo /novel  example_interfaces/msg/String
#data: '332526'
#---
#data: nxmachine
#---
#data: setup.py
#---
#data: SD card clone by dd on ubuntu
#---
---
```


```bash
ros2 topic echo --help
usage: ros2 topic echo [-h]
                       [--qos-profile {unknown,system_default,sensor_data,services_default,parameters,parameter_events,action_status_default}]
                       [--qos-depth N] [--qos-history {system_default,keep_last,keep_all,unknown}]
                       [--qos-reliability {system_default,reliable,best_effort,unknown}]
                       [--qos-durability {system_default,transient_local,volatile,unknown}] [--csv]
                       [--full-length] [--truncate-length TRUNCATE_LENGTH] [--no-arr] [--no-str]
                       topic_name [message_type]

Output messages from a topic

positional arguments:
  topic_name            Name of the ROS topic to listen to (e.g. '/chatter')
  message_type          Type of the ROS message (e.g. 'std_msgs/msg/String')

optional arguments:
  -h, --help            show this help message and exit
  --qos-profile {unknown,system_default,sensor_data,services_default,parameters,parameter_events,action_status_default}
                        Quality of service preset profile to subscribe with (default: sensor_data)
  --qos-depth N         Queue size setting to subscribe with (overrides depth value of --qos-profile option)
  --qos-history {system_default,keep_last,keep_all,unknown}
                        History of samples setting to subscribe with (overrides history value of --qos-
                        profile option, default: keep_last)
  --qos-reliability {system_default,reliable,best_effort,unknown}
                        Quality of service reliability setting to subscribe with (overrides reliability
                        value of --qos-profile option, default: best_effort)
  --qos-durability {system_default,transient_local,volatile,unknown}
                        Quality of service durability setting to subscribe with (overrides durability value
                        of --qos-profile option, default: volatile)
  --csv                 Output all recursive fields separated by commas (e.g. for plotting)
  --full-length, -f     Output all elements for arrays, bytes, and string with a length > '--truncate-
                        length', by default they are truncated after '--truncate-length' elements with
                        '...''
  --truncate-length TRUNCATE_LENGTH, -l TRUNCATE_LENGTH
                        The length to truncate arrays, bytes, and string to (default: 128)
  --no-arr              Don't print array fields of messages
  --no-str              Don't print string fields of messages
```