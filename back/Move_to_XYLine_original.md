```c
/*******************************************
函数名：Move_to_XYLine_original
作用：xy平面最短距离移动_线性
作者：Lcs
输入参数：float x 目标x位置 float y 目标y位置 float v 速度
输出参数：bool 是否到位
封装等级：3
*******************************************/
static bool Move_to_XYLine_original(float x, float y, float v)  //original 
{
	static uint8_t l_step = 0;
	float delta_x = x - t265_x;
	float delta_y = y - t265_y;
	float distance = sqrt(delta_x * delta_x + delta_y * delta_y);

	if (l_step == 0)
	{
		UnLock_position();
		Mode_Inf->target_x = x;
		Mode_Inf->target_y = y;
		l_step++;
	}
	else if (l_step == 1)
	{
		if (distance > 54)
		{
			float delta_vx = now_volx - v * delta_x / distance;
			float delta_vy = now_voly - v * delta_y / distance;

			if (fabs(delta_vx) > 22)
			{
				now_volx = now_volx - 20 * sign_f(delta_vx) * fabs(delta_x / distance);
			}
			else                                
			{
				now_volx = v * delta_x / distance;
			}

			if (fabs(delta_vy) > 22)
			{
				now_voly = now_voly - 20 * sign_f(delta_vy) * fabs(delta_y / distance);
			}
			else
			{
				now_voly = v * delta_y / distance;
			}
		}

		else if (distance > 7)
		{
			float delta_vx = now_volx - delta_x;
			float delta_vy = now_voly - delta_y;
			if (fabs(delta_vx) > 22)
			{
				now_volx = now_volx - 20 * sign_f(delta_vx) * fabs(delta_x / distance);
			}
			else
			{
				now_volx = delta_x; // 20 * delta_x / distance;
			}
			if (fabsf(now_volx) < 10)
			{
				now_volx = delta_x * 3;
			}
			else if (fabsf(now_volx) < 20)
			{
				now_volx = delta_x * 2;
			}
			if (fabs(delta_vy) > 22)
			{
				now_voly = now_voly - 20 * sign_f(delta_vy) * fabs(delta_y / distance);
			}
			else
			{
				now_voly = delta_y; // 20 * delta_y / distance;
			}
			if (fabsf(now_voly) < 10)
			{
				now_voly = delta_y * 3;
			}
			else if (fabsf(now_voly) < 20)
			{
				now_voly = delta_y * 2;
			}
		}
		else
		{
			Mode_Inf->target_x = x;
			Mode_Inf->target_y = y;
			l_step = 0;
			Lock_position_same();
			Lock_h_same();
			//Mode_Inf->zt++;
			return true;
		}
		Position_Control_set_TargetVelocityXY(now_volx, now_voly);
	}

	return false;
}


```