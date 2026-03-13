

## 起飞 Takeoff_h

```c
static bool Takeoff_h(float height, int v)
{
	float position_base = 10;                                 //初始响应高度（加速高度的判决，由飞机底盘决定）
	int vol_limit_takeoff = v;                                  //起飞加速度限制，由飞机重量决定
	 
	Mode_Inf->target_x = 0;
	Mode_Inf->target_y = 0;
		Lock_position(0.0f, 0.0f);

	now_volx = (Mode_Inf->target_x - t265_x) * 1.5;
	now_voly = (Mode_Inf->target_y - t265_y) * 1.5;

	if(t265_z <= position_base){
		now_volz = 60; // 60
		Position_Control_set_TargetVelocityZ(now_volz);
		Position_Control_set_TargetVelocityXY(now_volx, now_voly);
		
	}
	else if(t265_z < height - 4 * position_base){
		Position_Control_set_TargetVelocityXY(now_volx, now_voly);
		Position_Control_set_TargetVelocityZ( now_volz + 2);
		if(now_volz < vol_limit_takeoff){
			now_volz = now_volz + 2;
		}
		else{
			now_volz = now_volz - 2;
		}
	}
	else if(t265_z < height - 5){
		float now_hd = height - t265_z;
			now_volz = 4.0 * now_hd / 3.0;
		Position_Control_set_TargetVelocityXY(now_volx, now_voly);
			Position_Control_set_TargetVelocityZ(now_volz);
	}	
	else{
		Lock_h(height);
		Lock_position(0.0f, 0.0f);
		//Mode_Inf->zt++;
		return true;
	}
	return false;
}

```

## 降落 Land_g


```c
static bool Land_g(float v)
{
    static unsigned char soft_land_phase = 0;  // 0: 正常降落, 1: 软停止阶段
	static unsigned char soft_land_count = 0;
    static unsigned int target_width = 0;  // 目标油门，范围10000停转~20000全速转动的PWM控制脉冲值
    static unsigned int stop_width = 10000;  // 停止油门，10000为电机停止的PWM值
    UnLock_h();
    UnLock_position();
    Mode_Inf->target_z = -1;

    if (get_is_inFlight() == false) { //已降落 关停所有输出
        set_inFlight_false();
        PWM_PullDownAll();
        soft_land_phase = 0;  // 重置阶段
        return true;
    }
    // 正常降落阶段
    if (soft_land_phase == 0) {  // 正常降落阶段
        if (t265_z > 20.0f) {
            if (now_volz > -v) { now_volz -= 2;  } 
            else               { now_volz = -v;  }
        } 
        else if (t265_z > -20.0f) 
        {
            if (now_volz < -20.0f) { //限制下降速度，避免摔机
                now_volz += 1;
            } else {
                now_volz = -20;
            }
            // 落地判断
            if (fabsf(get_VelocityENU().z) < 5 && t265_z < 6.0f) {

                unsigned int pwm1 = PWMPulseWidthGet(PWM0_BASE, PWM_OUT_1);
                unsigned int pwm2 = PWMPulseWidthGet(PWM0_BASE, PWM_OUT_0);
                unsigned int pwm3 = PWMPulseWidthGet(PWM0_BASE, PWM_OUT_7);
                unsigned int pwm4 = PWMPulseWidthGet(PWM0_BASE, PWM_OUT_6);
                // 取最大值
								unsigned int max_pwm = pwm1;
								if (pwm2 > max_pwm) max_pwm = pwm2;
								if (pwm3 > max_pwm) max_pwm = pwm3;
								if (pwm4 > max_pwm) max_pwm = pwm4;
								target_width = max_pwm; // 使用最大的一路pwm作为起始油门
	
                soft_land_phase = 1; // 进入软停止阶段，不要立即PullDown
            }
        } else {//保险措施，避免失控，紧急刹停
            now_volz = -50;
            set_inFlight_false();
            PWM_PullDownAll();
            soft_land_phase = 0;
            return true;
        }
        now_volx = (Mode_Inf->target_x - t265_x) * 2;
        now_voly = (Mode_Inf->target_y - t265_y) * 2;
        Position_Control_set_TargetVelocityZ(now_volz);
        Position_Control_set_TargetVelocityXY(now_volx, now_voly);
    }   // 正常降落阶段 (soft_land_phase == 0) 
    // 软停止阶段
    if (soft_land_phase == 1) {  // 软停止阶段
        // 渐进降低油门，每20ms减小一点（避免突变）
        // 每次循环间隔20ms/ 50Hz，10000/1000 = 10
			  // 每次减15，根据您的PWM范围调整（e.g., 从20000之下减到stop_width=10000）
				if( soft_land_count < 120 ) { PWM_PulseWidthReduce_All(15); target_width -= 15; ++soft_land_count;}
				else                        { PWM_PulseWidthReduce_All(5);  target_width -= 5; }	
 
        if (target_width <= stop_width) {  // 最低idle值 = 10000 // 检查是否达到最低
            target_width = stop_width;
            set_inFlight_false();
            PWM_PullDownAll();   // 最终拉低,停机
			soft_land_count = 0; // 重置变量
            soft_land_phase = 0; // 重置变量
            return true;
        }
        // 设置所有电机到目标油门（同步，保持平衡）
        // 假设有函数直接设电机PWM，或通过mixer
        // 这里用简化版：直接设相同值，但实际应叠加姿态控制输出
			
		now_volx = (Mode_Inf->target_x - t265_x) ;
        now_voly = (Mode_Inf->target_y - t265_y) ;
		Position_Control_set_TargetVelocityXY(now_volx, now_voly);
    } // 软停止阶段  (soft_land_phase == 1) 

    return false;
}
```

## 移动 Move_xyz()
```c
static uint8_t Move_xyz(float x, float y, float z, float v)
{
	uint8_t fanhui = 0;
	static uint8_t l_step = 0;
	float delta_x = x - t265_x;
	float delta_y = y - t265_y;
	float delta_z = z - t265_z;
	float distance = sqrt(delta_x * delta_x + delta_y * delta_y + delta_z * delta_z);
	
	if (distance < 10)
	{
		fanhui = 1;
	}
	else if (distance < 20)
	{
		fanhui = -1;
	}

	if (l_step == 0)
	{
		UnLock_position();
		UnLock_h();
		Mode_Inf->target_x = x;
		Mode_Inf->target_y = y;
		Mode_Inf->target_z = z;
		l_step++;
	}
	else if (l_step == 1)
	{
		UnLock_position();
		UnLock_h();
		Mode_Inf->target_x = x;
		Mode_Inf->target_y = y;
		Mode_Inf->target_z = z;
		
		if (distance > 55)
		{
			// 远距离移动，使用方向向量乘以速度
			float delta_vx = now_volx - v * delta_x / distance;
			float delta_vy = now_voly - v * delta_y / distance;
			float delta_vz = now_volz - v * delta_z / distance;

			// X方向速度控制
			if (fabs(delta_vx) > 22)
			{
				now_volx = now_volx - 20 * sign_f(delta_vx) * fabs(delta_x / distance);
			}
			else
			{
				now_volx = v * delta_x / distance;
			}

			// Y方向速度控制
			if (fabs(delta_vy) > 22)
			{
				now_voly = now_voly - 20 * sign_f(delta_vy) * fabs(delta_y / distance);
			}
			else
			{
				now_voly = v * delta_y / distance;
			}
			
			// Z方向速度控制
			if (fabs(delta_vz) > 20)
			{
				now_volz = now_volz - 8 * sign_f(delta_vz) * fabs(delta_z / distance);
			}
			else
			{
				now_volz = v * delta_z / distance;
			}
		}
		else if (distance > 7)
		{
			// 中距离移动，使用比例控制
			float delta_vx = now_volx - delta_x;
			float delta_vy = now_voly - delta_y;
			float delta_vz = now_volz - delta_z;
			
			// X方向速度控制
			if (fabs(delta_vx) > 22)
			{
				now_volx = now_volx - 20 * sign_f(delta_vx) * fabs(delta_x / distance);
			}
			else
			{
				now_volx = delta_x;
			}
			if (fabsf(now_volx) < 20)
			{
				now_volx = delta_x * 2;
			}
			
			// Y方向速度控制
			if (fabs(delta_vy) > 22)
			{
				now_voly = now_voly - 20 * sign_f(delta_vy) * fabs(delta_y / distance);
			}
			else
			{
				now_voly = delta_y;
			}
			if (fabsf(now_voly) < 20)
			{
				now_voly = delta_y * 2;
			}
			
			// Z方向速度控制
			if (fabs(delta_vz) > 10)
			{
				now_volz = now_volz - 8 * sign_f(delta_vz) * fabs(delta_z / distance);
			}
			else
			{
				now_volz = delta_z;
			}
			if (fabsf(now_volz) < 10)
			{
				now_volz = delta_z * 2;
			}

		}
		else
		{
			// 到达目标位置
			Mode_Inf->target_x = x;
			Mode_Inf->target_y = y;
			Mode_Inf->target_z = z;
			l_step = 0;
			Lock_position_same();
			Lock_h_same();
			fanhui = 2;
		}
		
		// 设置速度
		Position_Control_set_TargetVelocityXY(now_volx, now_voly);
		Position_Control_set_TargetVelocityZ(now_volz);
	}
```


```
帮我写一份在单片机这边串口通信的接收端的函数，可以做到在串口中断内被调用，依次按照char接收并解码这个字符串：开头固定为"@L" ， 然后是一个二位数类型的字符串 “23” 或者 “00”，或者“52”， 这个数被解析出后存储到一个叫my_path_length的变量（实际不会超过64）；然后会有一个 “[” ；接着是 和path_length数量相同的 xyz三轴坐标组，每组的形式为：“+1234-7774+8651|”, 符号有正有负，然后的三个固定长的带符号的数字比如 +1234被解析为 int x = +1234 -7774被解析为int y=-7774,+8651被解析为int z = +8651 。每组坐标解析完成后，依次存入一个叫 int my_path[64][3]的变量， 比如第一组坐标 x存入 [0][0],y存入[0][1],z存入[0][2]。 xyz坐标组发送结束后，会带有一个尾部帧“]”。

好了，这是解析的基本要求, 我建议是串口将这串口将数据解析存入buffer，然后，设置接收完成标志变量，在主循环内运行解析。

与此同时，请帮我在单片机这边收集接收到的这条字符串消息汇总到一个string中，完整的存储下来，我后续需要发回给上位机验证。

接下来，请帮我写一份测试程序，while循环模拟上述串口中断的解析过程！ 解析过程请封装为函数的形式！ 我只需要最后的串口中断中接收字符存入buffer的函数，主循环中解析buffer的函数 以及my_path_length，my_path，以及汇总信息的字符串string用于回发验证。

```