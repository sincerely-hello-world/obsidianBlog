```
self.obs = [car_aim(x=0,y=0,label='上一次火源位置'),car_aim(x=0,y=0,label='现在火源位置')] # 本地检测到火源位置,上一次火源位置
# ⚠️ 绝对不要写 self.obs[0] = self.obs[1]
# 正确做法：只同步 X 和 Y 的数值，保持对象和 label 的独立性
self.obs[0].x = self.obs[1].x
self.obs[0].y = self.obs[1].y
self.timer_obs_wait = self.create_timer(2.0, self.obstacle_wait)
```