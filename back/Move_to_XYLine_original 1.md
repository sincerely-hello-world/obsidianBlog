```c
static bool Move_to_XYLine_original_v1(float x, float y, float v)  //original 
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
		if (distance > 25)
		{
			float delta_vx = now_volx - v * delta_x / distance;
			float delta_vy = now_voly - v * delta_y / distance;

			if (fabsf(delta_vx) > 22)
			{
				now_volx = now_volx - 20 * sign_f(delta_vx) * fabsf(delta_x / distance);
			}
			else                                
			{
				now_volx = v * delta_x / distance;
			}

			if (fabsf(delta_vy) > 22)
			{
				now_voly = now_voly - 20 * sign_f(delta_vy) * fabsf(delta_y / distance);
			}
			else
			{
				now_voly = v * delta_y / distance;
			}
		}

		else if (distance > 7)
		{
			if (fabsf(now_volx) > 15){ now_volx = 0.5 * ( 12*sign_f(delta_x) - now_volx);}
			else{now_volx = 12 * delta_x / distance;}
 
			if (fabsf(now_voly) > 15){ now_voly = 0.5 * ( 12*sign_f(delta_y) - now_voly);}
			else{now_voly = 12 * delta_y / distance;}
			
			if (fabsf(delta_x) < 10)	 now_volx = delta_x * 1.5;
			if (fabsf(delta_y) < 10)   now_voly = delta_y * 1.5;

		}
		
		else 
		{
			Mode_Inf->target_x = x;
			Mode_Inf->target_y = y;
			l_step = 0;
			Lock_position_same();
			Lock_h_same();
			return true;
		}

		Position_Control_set_TargetVelocityXY(now_volx, now_voly);
	}

	return false;
}


```