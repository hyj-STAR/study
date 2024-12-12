# LED点灯

## 代码：

#include "led.h"
#include "delay.h"
#include "sys.h"

//主函数
int main (void)
{
	delay_init(); //初始化延时函数
	NVIC_PriorityGroupConfig(NVIC_PriorityGroup_2); //中断优先级
	LED_Init(); //LED初始化
	while(1) //主循环函数
	{
		LED = 1; //LED灯亮
		delay_ms(500); //延时保持500毫秒
		LED = 0; //LED灯灭
		delay_ms(500); //延时保持500毫秒
	}
}

#include "led.h"   //引入头文件

void LED_Init(void)  //初始化LED引脚
{
	GPIO_InitTypeDef GPIO_InitStructure;  //初始化结构体
	RCC_APB2PeriphClockCmd(RCC_APB2Periph_GPIOC,ENABLE);  //使能时钟,必须要先使能IO的时钟
	
	GPIO_InitStructure.GPIO_Pin = GPIO_Pin_13;  //初始化第13引脚
	GPIO_InitStructure.GPIO_Mode = GPIO_Mode_Out_PP;  //推挽输出
	GPIO_InitStructure.GPIO_Speed = GPIO_Speed_50MHz;  //IO口速度为50MHz
	GPIO_Init(GPIOC,&GPIO_InitStructure);
	GPIO_SetBits(GPIOC,GPIO_Pin_13);  //初始化引脚为高电平输出，相当于LED = 1;
}

#ifndef _LED_H_
#define _LED_H_
#include "sys.h"
#define LED PCout(13) //定义输出引脚为13


void LED_Init(void); //初始化LED引脚
#endif