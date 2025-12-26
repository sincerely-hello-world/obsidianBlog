```python
import rospy
import serial
def main():
	rospy.init_node('serial_node')
	ser =serial.Serial('/dev/ttyUsB0'，9600) # 打开串口,波特率为9600
	rate =rospy.Rate(10)#设置发送频率
	
	while not rospy.is_shutdown():
		data =ser.readline()#读取串口数据
		rospy.loginfo(data)
		rate.sleep()
```
 