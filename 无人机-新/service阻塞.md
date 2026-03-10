阻塞式回调
```
def send_command(self, cmd:String):
        if rclpy.ok() and self.client_command.wait_for_service(timeout_sec = 0.5)==False:
            self.get_logger().info(f"发送{cmd}失败: 服务未就绪")
        # 1️⃣ 先创建 future
        request = ControlService.Request()
        request.req = cmd
        # 非zu
        future = self.client_command.call_async(request).add_done_callback(lambda fut: self._on_command_future_done(fut, cmd))
        # 2️⃣ 阻塞等待完成（注意：这个函数返回 None！）
        # ⚠️ 关键：使用 node 的 context 来等待，而不是全局 spin
        rclpy.spin_until_future_complete(self, future, timeout_sec=0.5)
        # 3️⃣ 检查结果
        info = None
        if future.done() and not future.exception():
            response = future.result()
            info = f"发送{response.req}成功:{response.echo}"
            self.cmd_result.emit(True, f"发送{response.req}成功:{response.echo}")  # 假设 res 是返回字段
            self.get_logger().info(info)
        else:
            err = str(future.exception()) if future.exception() else "Timeout"
            info = f"发送{request.req}失败: {err}"
            self.cmd_result.emit(False, f"发送{request.req}失败: {err}")
            self.get_logger().warning(info)


def _on_command_future_done(self, future, original_cmd):
	# 這段會在 executor 的 thread 執行（非主執行緒）
	try:
		response = future.result()  # 會 raise 如果有 exception
		msg = f"{original_cmd} 发送成功 → {response.echo}"   
		self.cmd_result.emit(True, msg)
		self.get_logger().info(msg)
	except Exception as e:
		err_msg = f"{original_cmd} 发送失敗: {str(e)}"
		self.cmd_result.emit(False, err_msg)
		self.get_logger().error(err_msg)
```