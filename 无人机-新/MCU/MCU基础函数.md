
## 降落 Land_g

```
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